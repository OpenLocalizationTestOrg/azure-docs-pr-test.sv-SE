---
title: "aaaSet distributionsordning för Azure-resurser | Microsoft Docs"
description: "Beskriver hur tooset en resurs som är beroende av en annan resurs under distributionen tooensure resurser distribueras i rätt ordning för hello."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: 
ms.assetid: 34ebaf1e-480c-4b4d-9bf6-251bd3f8f2cf
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/03/2017
ms.author: tomfitz
ms.openlocfilehash: 2f658f4c85236966c46b34a65aafb8426c92806c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="define-hello-order-for-deploying-resources-in-azure-resource-manager-templates"></a><span data-ttu-id="ac9ba-103">Definiera hello ordning för att distribuera resurser i Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="ac9ba-103">Define hello order for deploying resources in Azure Resource Manager templates</span></span>
<span data-ttu-id="ac9ba-104">För en viss resurs kan det finnas andra resurser som måste finnas innan hello resurs har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-104">For a given resource, there can be other resources that must exist before hello resource is deployed.</span></span> <span data-ttu-id="ac9ba-105">Till exempel måste en SQLServer finnas innan du försöker toodeploy en SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-105">For example, a SQL server must exist before attempting toodeploy a SQL database.</span></span> <span data-ttu-id="ac9ba-106">Du definierar relationen genom att markera en resurs som är beroende av hello andra resurser.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-106">You define this relationship by marking one resource as dependent on hello other resource.</span></span> <span data-ttu-id="ac9ba-107">Du definierar ett samband med hello **dependsOn** elementet eller genom att använda hello **referens** funktion.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-107">You define a dependency with hello **dependsOn** element, or by using hello **reference** function.</span></span> 

<span data-ttu-id="ac9ba-108">Resource Manager utvärderar hello beroenden mellan resurser och distribuerar dem i ordningsföljden beroende.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-108">Resource Manager evaluates hello dependencies between resources, and deploys them in their dependent order.</span></span> <span data-ttu-id="ac9ba-109">När resurserna inte är beroende av varandra, distribuerar dem i Resource Manager parallellt.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-109">When resources are not dependent on each other, Resource Manager deploys them in parallel.</span></span> <span data-ttu-id="ac9ba-110">Behöver du bara toodefine beroenden för resurser som har distribuerats i hello samma mall.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-110">You only need toodefine dependencies for resources that are deployed in hello same template.</span></span> 

## <a name="dependson"></a><span data-ttu-id="ac9ba-111">dependsOn</span><span class="sxs-lookup"><span data-stu-id="ac9ba-111">dependsOn</span></span>
<span data-ttu-id="ac9ba-112">Hello dependsOn-elementet låter toodefine en resurs i mallen, som beroende på en eller flera resurser.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-112">Within your template, hello dependsOn element enables you toodefine one resource as a dependent on one or more resources.</span></span> <span data-ttu-id="ac9ba-113">Värdet kan vara en kommaavgränsad lista över resurser.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-113">Its value can be a comma-separated list of resource names.</span></span> 

<span data-ttu-id="ac9ba-114">hello visas följande exempel en skaluppsättning för virtuell dator som är beroende av en belastningsutjämnare, virtuella nätverk och en loop som skapar flera lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-114">hello following example shows a virtual machine scale set that depends on a load balancer, virtual network, and a loop that creates multiple storage accounts.</span></span> <span data-ttu-id="ac9ba-115">Dessa andra resurser visas inte i följande exempel hello, men de måste tooexist någon annanstans i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-115">These other resources are not shown in hello following example, but they would need tooexist elsewhere in hello template.</span></span>

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "[variables('namingInfix')]",
  "location": "[variables('location')]",
  "apiVersion": "2016-03-30",
  "tags": {
    "displayName": "VMScaleSet"
  },
  "dependsOn": [
    "[variables('loadBalancerName')]",
    "[variables('virtualNetworkName')]",
    "storageLoop",
  ],
  ...
}
```

<span data-ttu-id="ac9ba-116">I föregående exempel hello, finns ett beroende på hello-resurser som har skapats via en kopia skapas med namnet **storageLoop**.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-116">In hello preceding example, a dependency is included on hello resources that are created through a copy loop named **storageLoop**.</span></span> <span data-ttu-id="ac9ba-117">Ett exempel finns [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="ac9ba-117">For an example, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>

<span data-ttu-id="ac9ba-118">När du definierar beroenden kan du inkludera hello resource provider namnområde och resursen typen tooavoid tvetydighet.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-118">When defining dependencies, you can include hello resource provider namespace and resource type tooavoid ambiguity.</span></span> <span data-ttu-id="ac9ba-119">Till exempel tooclarify en belastningsutjämnare och virtuella nätverk som kan ha hello samma namn som andra resurser, Använd hello följande format:</span><span class="sxs-lookup"><span data-stu-id="ac9ba-119">For example, tooclarify a load balancer and virtual network that may have hello same names as other resources, use hello following format:</span></span>

```json
"dependsOn": [
  "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
  "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
]
``` 

<span data-ttu-id="ac9ba-120">Du kanske lutande toouse dependsOn toomap relationer mellan dina resurser, är det viktigt toounderstand varför du gör det.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-120">While you may be inclined toouse dependsOn toomap relationships between your resources, it's important toounderstand why you're doing it.</span></span> <span data-ttu-id="ac9ba-121">Till exempel toodocument hur resurser är sammankopplade, är dependsOn inte hello rätt metod.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-121">For example, toodocument how resources are interconnected, dependsOn is not hello right approach.</span></span> <span data-ttu-id="ac9ba-122">Du kan inte fråga vilka resurser som har definierats i hello dependsOn-elementet efter distributionen.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-122">You cannot query which resources were defined in hello dependsOn element after deployment.</span></span> <span data-ttu-id="ac9ba-123">Genom att använda dependsOn kan påverka du eventuellt distributionen eftersom Resource Manager inte distribuerar i parallella två resurser som är beroende av.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-123">By using dependsOn, you potentially impact deployment time because Resource Manager does not deploy in parallel two resources that have a dependency.</span></span> <span data-ttu-id="ac9ba-124">toodocument relationer mellan resurser, i stället använda [resurslänkningen](/rest/api/resources/resourcelinks).</span><span class="sxs-lookup"><span data-stu-id="ac9ba-124">toodocument relationships between resources, instead use [resource linking](/rest/api/resources/resourcelinks).</span></span>

## <a name="child-resources"></a><span data-ttu-id="ac9ba-125">Underordnade resurser</span><span class="sxs-lookup"><span data-stu-id="ac9ba-125">Child resources</span></span>
<span data-ttu-id="ac9ba-126">hello resurser egenskap kan toospecify underordnade resurser som är relaterade toohello resurs som definieras.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-126">hello resources property allows you toospecify child resources that are related toohello resource being defined.</span></span> <span data-ttu-id="ac9ba-127">Underordnade resurser kan bara vara definierade fem nivåers djup.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-127">Child resources can only be defined five levels deep.</span></span> <span data-ttu-id="ac9ba-128">Det är viktigt toonote som inte är en implicit beroende skapats mellan en underordnad resurs och hello överordnade resursen.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-128">It is important toonote that an implicit dependency is not created between a child resource and hello parent resource.</span></span> <span data-ttu-id="ac9ba-129">Om du behöver hello underordnade resursen toobe distribueras efter hello överordnade resursen, måste du uttryckligen ange sambandet med hello dependsOn-egenskap.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-129">If you need hello child resource toobe deployed after hello parent resource, you must explicitly state that dependency with hello dependsOn property.</span></span> 

<span data-ttu-id="ac9ba-130">Varje överordnad resurs accepterar endast vissa typer av resurser som underordnade resurser.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-130">Each parent resource accepts only certain resource types as child resources.</span></span> <span data-ttu-id="ac9ba-131">hello accepteras resurstyper som anges i hello [mallsschemat](https://github.com/Azure/azure-resource-manager-schemas) hello överordnade resurs.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-131">hello accepted resource types are specified in hello [template schema](https://github.com/Azure/azure-resource-manager-schemas) of hello parent resource.</span></span> <span data-ttu-id="ac9ba-132">hello namnet på resurstypen för underordnade innehåller hello namnet på resurstypen för hello överordnade **Microsoft.Web/sites/config** och **Microsoft.Web/sites/extensions** är både underordnade resurser till hello  **Microsoft.Web/sites**.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-132">hello name of child resource type includes hello name of hello parent resource type, such as **Microsoft.Web/sites/config** and **Microsoft.Web/sites/extensions** are both child resources of hello **Microsoft.Web/sites**.</span></span>

<span data-ttu-id="ac9ba-133">hello som följande exempel visar en SQLServer och SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-133">hello following example shows a SQL server and SQL database.</span></span> <span data-ttu-id="ac9ba-134">Observera att en explicit beroende definieras mellan hello SQL database och SQLServer, trots att hello-databasen är en underordnad server hello.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-134">Notice that an explicit dependency is defined between hello SQL database and SQL server, even though hello database is a child of hello server.</span></span>

```json
"resources": [
  {
    "name": "[variables('sqlserverName')]",
    "type": "Microsoft.Sql/servers",
    "location": "[resourceGroup().location]",
    "tags": {
      "displayName": "SqlServer"
    },
    "apiVersion": "2014-04-01-preview",
    "properties": {
      "administratorLogin": "[parameters('administratorLogin')]",
      "administratorLoginPassword": "[parameters('administratorLoginPassword')]"
    },
    "resources": [
      {
        "name": "[parameters('databaseName')]",
        "type": "databases",
        "location": "[resourceGroup().location]",
        "tags": {
          "displayName": "Database"
        },
        "apiVersion": "2014-04-01-preview",
        "dependsOn": [
          "[variables('sqlserverName')]"
        ],
        "properties": {
          "edition": "[parameters('edition')]",
          "collation": "[parameters('collation')]",
          "maxSizeBytes": "[parameters('maxSizeBytes')]",
          "requestedServiceObjectiveName": "[parameters('requestedServiceObjectiveName')]"
        }
      }
    ]
  }
]
```

## <a name="reference-function"></a><span data-ttu-id="ac9ba-135">referens-funktion</span><span class="sxs-lookup"><span data-stu-id="ac9ba-135">reference function</span></span>
<span data-ttu-id="ac9ba-136">Hej [referera funktionen](resource-group-template-functions-resource.md#reference) kan ett uttryck tooderive sitt värde från andra JSON namn och värdepar eller runtime-resurser.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-136">hello [reference function](resource-group-template-functions-resource.md#reference) enables an expression tooderive its value from other JSON name and value pairs or runtime resources.</span></span> <span data-ttu-id="ac9ba-137">Referensuttryck deklarerar implicit att en resurs beror på en annan.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-137">Reference expressions implicitly declare that one resource depends on another.</span></span> <span data-ttu-id="ac9ba-138">hello allmänna format är:</span><span class="sxs-lookup"><span data-stu-id="ac9ba-138">hello general format is:</span></span>

```json
reference('resourceName').propertyPath
```

<span data-ttu-id="ac9ba-139">I följande exempel hello, en CDN-slutpunkt beror uttryckligen på hello CDN-profilen och beror implicit på ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-139">In hello following example, a CDN endpoint explicitly depends on hello CDN profile, and implicitly depends on a web app.</span></span>

```json
{
    "name": "[variables('endpointName')]",
    "type": "endpoints",
    "location": "[resourceGroup().location]",
    "apiVersion": "2016-04-02",
    "dependsOn": [
            "[variables('profileName')]"
    ],
    "properties": {
        "originHostHeader": "[reference(variables('webAppName')).hostNames[0]]",
        ...
    }
```

<span data-ttu-id="ac9ba-140">Du kan använda det här elementet eller hello dependsOn elementet toospecify beroenden, men du behöver inte toouse både för hello samma beroende resurs.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-140">You can use either this element or hello dependsOn element toospecify dependencies, but you do not need toouse both for hello same dependent resource.</span></span> <span data-ttu-id="ac9ba-141">Använd en referera implicit tooavoid lägger till en onödiga beroende om möjligt.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-141">Whenever possible, use an implicit reference tooavoid adding an unnecessary dependency.</span></span>

<span data-ttu-id="ac9ba-142">Det finns fler toolearn [referera funktionen](resource-group-template-functions-resource.md#reference).</span><span class="sxs-lookup"><span data-stu-id="ac9ba-142">toolearn more, see [reference function](resource-group-template-functions-resource.md#reference).</span></span>

## <a name="recommendations-for-setting-dependencies"></a><span data-ttu-id="ac9ba-143">Rekommendationer för att ställa in beroenden</span><span class="sxs-lookup"><span data-stu-id="ac9ba-143">Recommendations for setting dependencies</span></span>

<span data-ttu-id="ac9ba-144">Använd hello följande riktlinjer när du bestämmer vilka beroenden tooset:</span><span class="sxs-lookup"><span data-stu-id="ac9ba-144">When deciding what dependencies tooset, use hello following guidelines:</span></span>

* <span data-ttu-id="ac9ba-145">Ange beroenden så lite som möjligt.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-145">Set as few dependencies as possible.</span></span>
* <span data-ttu-id="ac9ba-146">Ange en underordnad resurs som är beroende av den överordnade resursen.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-146">Set a child resource as dependent on its parent resource.</span></span>
* <span data-ttu-id="ac9ba-147">Använd hello **referens** fungerar tooset implicit beroenden mellan resurser behöver tooshare en egenskap.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-147">Use hello **reference** function tooset implicit dependencies between resources that need tooshare a property.</span></span> <span data-ttu-id="ac9ba-148">Lägg inte till ett explicit beroende (**dependsOn**) när du redan har definierat ett indirekt samband.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-148">Do not add an explicit dependency (**dependsOn**) when you have already defined an implicit dependency.</span></span> <span data-ttu-id="ac9ba-149">Den här metoden minskar hello risken att onödiga beroenden.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-149">This approach reduces hello risk of having unnecessary dependencies.</span></span> 
* <span data-ttu-id="ac9ba-150">Ange ett beroende när en resurs inte får vara **skapade** utan funktioner från en annan resurs.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-150">Set a dependency when a resource cannot be **created** without functionality from another resource.</span></span> <span data-ttu-id="ac9ba-151">Ange inte ett beroende om endast interagera hello resurser efter distributionen.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-151">Do not set a dependency if hello resources only interact after deployment.</span></span>
* <span data-ttu-id="ac9ba-152">Låt beroenden cascade utan att ange dem explicit.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-152">Let dependencies cascade without setting them explicitly.</span></span> <span data-ttu-id="ac9ba-153">Till exempel den virtuella datorn är beroende av ett virtuellt nätverksgränssnitt och hello virtuella nätverksgränssnittet beror på ett virtuellt nätverk och offentliga IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-153">For example, your virtual machine depends on a virtual network interface, and hello virtual network interface depends on a virtual network and public IP addresses.</span></span> <span data-ttu-id="ac9ba-154">Därför hello virtuella datorn är distribuerad när alla tre resurser, men uttryckligen anges inte hello virtuell dator som är beroende av alla tre resurser.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-154">Therefore, hello virtual machine is deployed after all three resources, but do not explicitly set hello virtual machine as dependent on all three resources.</span></span> <span data-ttu-id="ac9ba-155">Den här metoden klargör hello beroendeordningen och gör det enklare toochange hello mallen senare.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-155">This approach clarifies hello dependency order and makes it easier toochange hello template later.</span></span>
* <span data-ttu-id="ac9ba-156">Om ett värde kan fastställas innan distribution, försök att distribuera hello resursen utan ett beroende.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-156">If a value can be determined before deployment, try deploying hello resource without a dependency.</span></span> <span data-ttu-id="ac9ba-157">Om ett konfigurationsvärde måste hello namnet på en annan resurs, kanske du inte behöver ett beroende.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-157">For example, if a configuration value needs hello name of another resource, you might not need a dependency.</span></span> <span data-ttu-id="ac9ba-158">Den här vägledningen fungerar inte alltid eftersom vissa resurser verifiera hello att hello andra resurser.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-158">This guidance does not always work because some resources verify hello existence of hello other resource.</span></span> <span data-ttu-id="ac9ba-159">Om du får ett felmeddelande, lägger du till ett beroende.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-159">If you receive an error, add a dependency.</span></span> 

<span data-ttu-id="ac9ba-160">Resource Manager identifierar cirkulärt tjänstberoende vid verifiering av mallen.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-160">Resource Manager identifies circular dependencies during template validation.</span></span> <span data-ttu-id="ac9ba-161">Om du får ett felmeddelande om att det finns ett cirkulärt beroende, utvärdera din mall toosee om eventuella beroenden som inte behövs och kan tas bort.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-161">If you receive an error stating that a circular dependency exists, evaluate your template toosee if any dependencies are not needed and can be removed.</span></span> <span data-ttu-id="ac9ba-162">Om du tar bort beroenden inte fungerar kan undvika du Cirkelberoenden genom att flytta vissa distributionsåtgärder till underordnade resurser som distribueras efter hello-resurser som har hello cirkulärt beroende.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-162">If removing dependencies does not work, you can avoid circular dependencies by moving some deployment operations into child resources that are deployed after hello resources that have hello circular dependency.</span></span> <span data-ttu-id="ac9ba-163">Anta exempelvis att du distribuerar två virtuella datorer, men du måste ange egenskaper för var och en som refererar toohello andra.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-163">For example, suppose you are deploying two virtual machines but you must set properties on each one that refer toohello other.</span></span> <span data-ttu-id="ac9ba-164">Du kan distribuera dem i hello följande ordning:</span><span class="sxs-lookup"><span data-stu-id="ac9ba-164">You can deploy them in hello following order:</span></span>

1. <span data-ttu-id="ac9ba-165">vm1</span><span class="sxs-lookup"><span data-stu-id="ac9ba-165">vm1</span></span>
2. <span data-ttu-id="ac9ba-166">vm2</span><span class="sxs-lookup"><span data-stu-id="ac9ba-166">vm2</span></span>
3. <span data-ttu-id="ac9ba-167">Tillägg på vm1 beror på vm1 och vm2.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-167">Extension on vm1 depends on vm1 and vm2.</span></span> <span data-ttu-id="ac9ba-168">hello-tillägget anger värden för vm1 får från vm2.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-168">hello extension sets values on vm1 that it gets from vm2.</span></span>
4. <span data-ttu-id="ac9ba-169">Tillägg på vm2 beror på vm1 och vm2.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-169">Extension on vm2 depends on vm1 and vm2.</span></span> <span data-ttu-id="ac9ba-170">hello-tillägget anger värden för vm2 får från vm1.</span><span class="sxs-lookup"><span data-stu-id="ac9ba-170">hello extension sets values on vm2 that it gets from vm1.</span></span>

<span data-ttu-id="ac9ba-171">Information om utvärdera hello distributionsordning och beroende kodproblem finns [felsöka vanliga Azure-distribution med Azure Resource Manager](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="ac9ba-171">For information about assessing hello deployment order and resolving dependency errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ac9ba-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ac9ba-172">Next steps</span></span>
* <span data-ttu-id="ac9ba-173">toolearn om felsökning av beroenden under distributionen, se [felsöka vanliga Azure-distribution med Azure Resource Manager](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="ac9ba-173">toolearn about troubleshooting dependencies during deployment, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>
* <span data-ttu-id="ac9ba-174">toolearn om hur du skapar mallar för Azure Resource Manager finns [Webbsidemallar](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="ac9ba-174">toolearn about creating Azure Resource Manager templates, see [Authoring templates](resource-group-authoring-templates.md).</span></span> 
* <span data-ttu-id="ac9ba-175">En lista över hello tillgängliga funktioner i en mall finns [Mallfunktioner](resource-group-template-functions.md).</span><span class="sxs-lookup"><span data-stu-id="ac9ba-175">For a list of hello available functions in a template, see [Template functions](resource-group-template-functions.md).</span></span>

