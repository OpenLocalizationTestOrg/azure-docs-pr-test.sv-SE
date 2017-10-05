---
title: Hantera resurser med Azure CLI | Microsoft Docs
description: "Använda Azure-kommandoradsgränssnittet (CLI) för att hantera Azure-resurser och grupper"
editor: 
manager: timlt
documentationcenter: 
author: tfitzmac
services: azure-resource-manager
ms.assetid: bb0af466-4f65-4559-ac3a-43985fa096ff
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 08/22/2016
ms.author: tomfitz
ms.openlocfilehash: 3ad4e68b90979fd7f9d3ddf5278e65e19cb07152
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-azure-cli-to-manage-azure-resources-and-resource-groups"></a><span data-ttu-id="682cb-103">Använda Azure CLI för att hantera Azure-resurser och resursgrupper</span><span class="sxs-lookup"><span data-stu-id="682cb-103">Use the Azure CLI to manage Azure resources and resource groups</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="682cb-104">Portal</span><span class="sxs-lookup"><span data-stu-id="682cb-104">Portal</span></span>](resource-group-portal.md) 
> * [<span data-ttu-id="682cb-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="682cb-105">Azure CLI</span></span>](xplat-cli-azure-resource-manager.md)
> * [<span data-ttu-id="682cb-106">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="682cb-106">Azure PowerShell</span></span>](powershell-azure-resource-manager.md)
> * [<span data-ttu-id="682cb-107">REST-API</span><span class="sxs-lookup"><span data-stu-id="682cb-107">REST API</span></span>](resource-manager-rest-api.md)
> 
> 

<span data-ttu-id="682cb-108">Azure-kommandoradsgränssnittet (Azure CLI) är en av flera verktyg som du kan använda för att distribuera och hantera resurser med Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="682cb-108">The Azure Command-Line Interface (Azure CLI) is one of several tools you can use to deploy and manage resources with Resource Manager.</span></span> <span data-ttu-id="682cb-109">Den här artikeln innehåller vanliga sätt att hantera Azure-resurser och resursgrupper med hjälp av Azure CLI i Resource Manager-läge.</span><span class="sxs-lookup"><span data-stu-id="682cb-109">This article introduces common ways to manage Azure resources and resource groups by using the Azure CLI in Resource Manager mode.</span></span> <span data-ttu-id="682cb-110">Information om hur du använder CLI för att distribuera resurser finns [distribuera resurser med Resource Manager-mallar och Azure CLI](resource-group-template-deploy-cli.md).</span><span class="sxs-lookup"><span data-stu-id="682cb-110">For information about using the CLI to deploy resources, see [Deploy resources with Resource Manager templates and Azure CLI](resource-group-template-deploy-cli.md).</span></span> <span data-ttu-id="682cb-111">Information om Azure-resurser och Resource Manager finns i [översikt över Azure Resource Manager](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="682cb-111">For background about Azure resources and Resource Manager, visit the [Azure Resource Manager Overview](resource-group-overview.md).</span></span>

> [!NOTE]
> <span data-ttu-id="682cb-112">Om du vill hantera Azure-resurser med Azure CLI, behöver du [installerar Azure CLI](../cli-install-nodejs.md), och [logga in på Azure](../xplat-cli-connect.md) med hjälp av den `azure login` kommando.</span><span class="sxs-lookup"><span data-stu-id="682cb-112">To manage Azure resources with the Azure CLI, you need to [install the Azure CLI](../cli-install-nodejs.md), and [log in to Azure](../xplat-cli-connect.md) by using the `azure login` command.</span></span> <span data-ttu-id="682cb-113">Kontrollera att CLI finns i Resource Manager-läget (kör `azure config mode arm`).</span><span class="sxs-lookup"><span data-stu-id="682cb-113">Make sure the CLI is in Resource Manager mode (run `azure config mode arm`).</span></span> <span data-ttu-id="682cb-114">Om du har gjort detta kan är du redo att sätta igång.</span><span class="sxs-lookup"><span data-stu-id="682cb-114">If you've done these things, you're ready to go.</span></span>
> 
> 

## <a name="get-resource-groups-and-resources"></a><span data-ttu-id="682cb-115">Hämta resursgrupper och resurser</span><span class="sxs-lookup"><span data-stu-id="682cb-115">Get resource groups and resources</span></span>
### <a name="resource-groups"></a><span data-ttu-id="682cb-116">Resursgrupper</span><span class="sxs-lookup"><span data-stu-id="682cb-116">Resource groups</span></span>
<span data-ttu-id="682cb-117">Kör kommandot för att hämta en lista över alla resursgrupper i din prenumeration och deras platser.</span><span class="sxs-lookup"><span data-stu-id="682cb-117">To get a list of all resource groups in your subscription and their locations, run this command.</span></span>

    azure group list


### <a name="resources"></a><span data-ttu-id="682cb-118">Resurser</span><span class="sxs-lookup"><span data-stu-id="682cb-118">Resources</span></span>
 <span data-ttu-id="682cb-119">Att lista alla resurser i en grupp, t.ex ett med namnet *testRG*, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="682cb-119">To list all resources in a group, such as one with name *testRG*, use the following command:</span></span>

    azure resource list testRG

<span data-ttu-id="682cb-120">Så här visar du en enskild resurs i gruppen, t.ex. en virtuell dator med namnet *MyUbuntuVM*, använder du ett kommando som följande:</span><span class="sxs-lookup"><span data-stu-id="682cb-120">To view an individual resource within the group, such as a VM named *MyUbuntuVM*, use a command like the following:</span></span>

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

<span data-ttu-id="682cb-121">Observera den **Microsoft.Compute/virtualMachines** parameter.</span><span class="sxs-lookup"><span data-stu-id="682cb-121">Notice the **Microsoft.Compute/virtualMachines** parameter.</span></span> <span data-ttu-id="682cb-122">Den här parametern anger vilken typ av resurs som du begär information.</span><span class="sxs-lookup"><span data-stu-id="682cb-122">This parameter indicates the type of the resource you are requesting information on.</span></span>

> [!NOTE]
> <span data-ttu-id="682cb-123">När du använder den **azure-resurs** kommandon än den **lista** kommando, måste du ange API-versionen av resursen med det **-o** parameter.</span><span class="sxs-lookup"><span data-stu-id="682cb-123">When using the **azure resource** commands other than the **list** command, you must specify the API version of the resource with the **-o** parameter.</span></span> <span data-ttu-id="682cb-124">Om du är osäker på om API-versionen, se mallfilen och hitta fältet apiVersion för resursen.</span><span class="sxs-lookup"><span data-stu-id="682cb-124">If you're unsure about the API version, consult the template file and find the apiVersion field for the resource.</span></span> <span data-ttu-id="682cb-125">Mer information om API-versioner i Resource Manager finns i [resursproviders och typer](resource-manager-supported-services.md).</span><span class="sxs-lookup"><span data-stu-id="682cb-125">For more about API versions in Resource Manager, see [Resource providers and types](resource-manager-supported-services.md).</span></span>
> 
> 

<span data-ttu-id="682cb-126">När du visar information på en resurs kan det vara bra att använda den `--json` parameter.</span><span class="sxs-lookup"><span data-stu-id="682cb-126">When viewing details on a resource, it is often useful to use the `--json` parameter.</span></span> <span data-ttu-id="682cb-127">Denna parameter gör utdata mer lättläst eftersom vissa värden kapslade strukturer eller samlingar.</span><span class="sxs-lookup"><span data-stu-id="682cb-127">This parameter makes the output more readable, because some values are nested structures, or collections.</span></span> <span data-ttu-id="682cb-128">Exemplet nedan visar returnerar resultatet av den **visa** kommandot som JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="682cb-128">The following example demonstrates returning the results of the **show** command as a JSON document.</span></span>

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json

> [!NOTE]
> <span data-ttu-id="682cb-129">Om du vill spara JSON-data till filen med hjälp av den &gt; tecken för att dirigera utdata till en fil.</span><span class="sxs-lookup"><span data-stu-id="682cb-129">If you want, save the JSON data to file by using the &gt; character to direct the output to a file.</span></span> <span data-ttu-id="682cb-130">Exempel:</span><span class="sxs-lookup"><span data-stu-id="682cb-130">For example:</span></span>
> 
> `azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json > myfile.json`
> 
> 

### <a name="tags"></a><span data-ttu-id="682cb-131">Taggar</span><span class="sxs-lookup"><span data-stu-id="682cb-131">Tags</span></span>
[!INCLUDE [resource-manager-tag-resources-cli](../../includes/resource-manager-tag-resources-cli.md)]

## <a name="manage-resources"></a><span data-ttu-id="682cb-132">Hantera resurser</span><span class="sxs-lookup"><span data-stu-id="682cb-132">Manage resources</span></span>
<span data-ttu-id="682cb-133">Lägg till en resurs, till exempel ett lagringskonto i en resursgrupp, kör du ett kommando som liknar:</span><span class="sxs-lookup"><span data-stu-id="682cb-133">To add a resource such as a storage account to a resource group, run a command similar to:</span></span>

    azure resource create testRG MyStorageAccount "Microsoft.Storage/storageAccounts" "westus" -o "2015-06-15" -p "{\"accountType\": \"Standard_LRS\"}"

<span data-ttu-id="682cb-134">Förutom att ange API-versionen av resursen med det **-o** parameter, Använd den **-p** parametern för att skicka en JSON-formaterad sträng med alla obligatoriska eller ytterligare egenskaper.</span><span class="sxs-lookup"><span data-stu-id="682cb-134">In addition to specifying the API version of the resource with the **-o** parameter, use the **-p** parameter to pass a JSON-formatted string with any required or additional properties.</span></span>

<span data-ttu-id="682cb-135">Om du vill ta bort en befintlig resurs, till exempel en virtuell datorresurs, använder du ett kommando som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="682cb-135">To delete an existing resource such as a virtual machine resource, use a command like the following:</span></span>

    azure resource delete testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

<span data-ttu-id="682cb-136">Flytta befintliga resurser till en annan resursgrupp eller prenumeration genom att använda den **azure-resurs flytta** kommando.</span><span class="sxs-lookup"><span data-stu-id="682cb-136">To move existing resources to another resource group or subscription, use the **azure resource move** command.</span></span> <span data-ttu-id="682cb-137">I följande exempel visas hur du flyttar ett Redis-Cache till en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="682cb-137">The following example shows how to move a Redis Cache to a new resource group.</span></span> <span data-ttu-id="682cb-138">I den **-i** parameter, ange en kommaavgränsad lista med resurs-id är för att flytta.</span><span class="sxs-lookup"><span data-stu-id="682cb-138">In the **-i** parameter, provide a comma-separated list of the resource id's to move.</span></span>

    azure resource move -i "/subscriptions/{guid}/resourceGroups/OldRG/providers/Microsoft.Cache/Redis/examplecache" -d "NewRG"

## <a name="control-access-to-resources"></a><span data-ttu-id="682cb-139">Styr åtkomsten till resurser</span><span class="sxs-lookup"><span data-stu-id="682cb-139">Control access to resources</span></span>
<span data-ttu-id="682cb-140">Du kan använda Azure CLI för att skapa och hantera principer för att styra åtkomsten till Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="682cb-140">You can use the Azure CLI to create and manage policies to control access to Azure resources.</span></span> <span data-ttu-id="682cb-141">Bakgrund om principdefinitioner och tilldelning av principer till resurser, se [använda för att hantera resurser och åtkomstkontroll](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="682cb-141">For background about policy definitions and assigning policies to resources, see [Use policy to manage resources and control access](resource-manager-policy.md).</span></span>

<span data-ttu-id="682cb-142">Till exempel definiera följande princip för att neka alla begäranden om platsen inte är västra USA eller norra centrala USA och spara den på principen definition filen policy.json:</span><span class="sxs-lookup"><span data-stu-id="682cb-142">For example, define the following policy to deny all requests where location is not West US or North Central US, and save it to the policy definition file policy.json:</span></span>

    {
    "if" : {
        "not" : {
        "field" : "location",
        "in" : ["westus" ,  "northcentralus"]
        }
    },
    "then" : {
        "effect" : "deny"
    }
    }

<span data-ttu-id="682cb-143">Kör sedan den **principdefinitionen skapa** kommando:</span><span class="sxs-lookup"><span data-stu-id="682cb-143">Then run the **policy definition create** command:</span></span>

    azure policy definition create MyPolicy -p c:\temp\policy.json

<span data-ttu-id="682cb-144">Det här kommandot visar utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="682cb-144">This command shows output similar to the following:</span></span>

    + <span data-ttu-id="682cb-145">Skapar principdefinitionen MyPolicy data: Principnamn: MyPolicy data: PolicyDefinitionId: /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy</span><span class="sxs-lookup"><span data-stu-id="682cb-145">Creating policy definition MyPolicy data:    PolicyName:             MyPolicy data:    PolicyDefinitionId:     /subscriptions/########-####-####-####-############/providers/Microsoft.Authorization/policyDefinitions/MyPolicy</span></span>

    <span data-ttu-id="682cb-146">data: PolicyType: anpassade data: DisplayName: Odefinierad data: beskrivning: Odefinierad data: PolicyRule: fältet = plats, i = [westus northcentralus], gälla = neka</span><span class="sxs-lookup"><span data-stu-id="682cb-146">data:    PolicyType:             Custom data:    DisplayName:            undefined data:    Description:            undefined data:    PolicyRule:             field=location, in=[westus, northcentralus], effect=deny</span></span>

 <span data-ttu-id="682cb-147">Om du vill tilldela en princip för område som du vill använda den **PolicyDefinitionId** returneras från det föregående kommandot.</span><span class="sxs-lookup"><span data-stu-id="682cb-147">To assign a policy at the scope you want, use the **PolicyDefinitionId** returned from the previous command.</span></span> <span data-ttu-id="682cb-148">I följande exempel detta scope prenumerationen, men du kan ange omfång för resursgrupper eller enskilda resurser:</span><span class="sxs-lookup"><span data-stu-id="682cb-148">In the following example, this scope is the subscription, but you can scope to resource groups or individual resources:</span></span>

    <span data-ttu-id="682cb-149">Azure principtilldelningens skapa MyPolicyAssignment -p /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy -s /subscriptions/###-###-###-###-### /</span><span class="sxs-lookup"><span data-stu-id="682cb-149">azure policy assignment create MyPolicyAssignment -p /subscriptions/########-####-####-####-############/providers/Microsoft.Authorization/policyDefinitions/MyPolicy -s /subscriptions/########-####-####-####-############/</span></span>

<span data-ttu-id="682cb-150">Du kan hämta, ändra eller ta bort principdefinitioner genom att använda den **princip definition visa**, **definition principuppsättningen**, och **principdefinitionen ta bort** kommandon.</span><span class="sxs-lookup"><span data-stu-id="682cb-150">You can get, change, or remove policy definitions by using the **policy definition show**, **policy definition set**, and **policy definition delete** commands.</span></span>

<span data-ttu-id="682cb-151">På samma sätt kan du kan hämta, ändra eller ta bort principtilldelningar med den **princip tilldelningen visa**, **tilldelning principuppsättningen**, och **principtilldelning ta bort** kommandon.</span><span class="sxs-lookup"><span data-stu-id="682cb-151">Similarly, you can get, change, or remove policy assignments by using the **policy assignment show**, **policy assignment set**, and **policy assignment delete** commands.</span></span>

## <a name="export-a-resource-group-as-a-template"></a><span data-ttu-id="682cb-152">Exportera en resursgrupp som en mall</span><span class="sxs-lookup"><span data-stu-id="682cb-152">Export a resource group as a template</span></span>
<span data-ttu-id="682cb-153">Du kan visa Resource Manager-mallen för resursgruppen för en befintlig resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="682cb-153">For an existing resource group, you can view the Resource Manager template for the resource group.</span></span> <span data-ttu-id="682cb-154">Exportera mallen har två fördelar:</span><span class="sxs-lookup"><span data-stu-id="682cb-154">Exporting the template offers two benefits:</span></span>

1. <span data-ttu-id="682cb-155">Du kan enkelt automatisera framtida distributioner av lösningen eftersom infrastrukturen som definieras i mallen.</span><span class="sxs-lookup"><span data-stu-id="682cb-155">You can easily automate future deployments of the solution because all the infrastructure is defined in the template.</span></span>
2. <span data-ttu-id="682cb-156">Du kan bekanta dig med mallens syntax genom att titta på JSON som representerar din lösning.</span><span class="sxs-lookup"><span data-stu-id="682cb-156">You can become familiar with template syntax by looking at the JSON that represents your solution.</span></span>

<span data-ttu-id="682cb-157">Med hjälp av Azure CLI, kan du antingen exportera en mall som representerar det aktuella tillståndet för resursgruppen eller hämta mallen som användes för en viss distribution.</span><span class="sxs-lookup"><span data-stu-id="682cb-157">Using the Azure CLI, you can either export a template that represents the current state of your resource group, or download the template that was used for a particular deployment.</span></span>

* <span data-ttu-id="682cb-158">**Exportera mallen för en resursgrupp** -det här är användbart när du har gjort ändringar i en resursgrupp och behöver hämta JSON-representation av det aktuella tillståndet.</span><span class="sxs-lookup"><span data-stu-id="682cb-158">**Export the template for a resource group** - This is helpful when you made changes to a resource group, and need to retrieve the JSON representation of its current state.</span></span> <span data-ttu-id="682cb-159">Genererade mallen innehåller dock endast ett minimalt antal parametrar och inga variabler.</span><span class="sxs-lookup"><span data-stu-id="682cb-159">However, the generated template contains only a minimal number of parameters and no variables.</span></span> <span data-ttu-id="682cb-160">De flesta av värdena i mallen är hårdkodat.</span><span class="sxs-lookup"><span data-stu-id="682cb-160">Most of the values in the template are hard-coded.</span></span> <span data-ttu-id="682cb-161">Innan du distribuerar mallen genererade, kanske du vill konvertera flera värden till parametrar, så du kan anpassa distributionen för olika miljöer.</span><span class="sxs-lookup"><span data-stu-id="682cb-161">Before deploying the generated template, you may wish to convert more of the values into parameters so you can customize the deployment for different environments.</span></span>
  
    <span data-ttu-id="682cb-162">Om du vill exportera mallen för en resursgrupp till en lokal katalog, kör den `azure group export` som visas i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="682cb-162">To export the template for a resource group to a local directory, run the `azure group export` command as shown in the following example.</span></span> <span data-ttu-id="682cb-163">(I stället använda en lokal katalog som är lämpliga för din operativsystemmiljö.)</span><span class="sxs-lookup"><span data-stu-id="682cb-163">(Substitute a local directory appropriate for your operating system environment.)</span></span>
  
        azure group export testRG ~/azure/templates/
* <span data-ttu-id="682cb-164">**Hämta mall för en viss distribution** – det här är användbart när du vill visa den faktiska mall som användes för att distribuera resurser.</span><span class="sxs-lookup"><span data-stu-id="682cb-164">**Download the template for a particular deployment** -- This is helpful when you need to view the actual template that was used to deploy resources.</span></span> <span data-ttu-id="682cb-165">Mallen innehåller alla parametrar och variabler som definieras för den ursprungliga distributionen.</span><span class="sxs-lookup"><span data-stu-id="682cb-165">The template includes all parameters and variables defined for the original deployment.</span></span> <span data-ttu-id="682cb-166">Om någon i organisationen har gjort ändringar i resursgruppen utanför definition i mallen representerar inte den här mallen det aktuella tillståndet för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="682cb-166">However, if someone in your organization made changes to the resource group outside of the definition in the template, this template doesn't represent the current state of the resource group.</span></span>
  
    <span data-ttu-id="682cb-167">Om du vill hämta en mall för en viss distribution till en lokal katalog, kör den `azure group deployment template download` kommando.</span><span class="sxs-lookup"><span data-stu-id="682cb-167">To download the template used for a particular deployment to a local directory, run the `azure group deployment template download` command.</span></span> <span data-ttu-id="682cb-168">Exempel:</span><span class="sxs-lookup"><span data-stu-id="682cb-168">For example:</span></span>
  
        azure group deployment template download TestRG testRGDeploy ~/azure/templates/downloads/

> [!NOTE]
> <span data-ttu-id="682cb-169">Exportera mallen är en förhandsversion och inte alla typer av resurser stöd för närvarande inte exportera en mall.</span><span class="sxs-lookup"><span data-stu-id="682cb-169">Template export is in preview, and not all resource types currently support exporting a template.</span></span> <span data-ttu-id="682cb-170">När du försöker exportera en mall kan du se ett felmeddelande om att vissa resurser inte exporterades.</span><span class="sxs-lookup"><span data-stu-id="682cb-170">When attempting to export a template, you may see an error that states some resources were not exported.</span></span> <span data-ttu-id="682cb-171">Om det behövs, manuellt definiera resurserna i mallen när du har hämtat den.</span><span class="sxs-lookup"><span data-stu-id="682cb-171">If needed, manually define these resources in your template after downloading it.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="682cb-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="682cb-172">Next steps</span></span>
* <span data-ttu-id="682cb-173">För att få information om distributionsåtgärder och felsöka distribution med Azure CLI, se [visa distributionsåtgärder](resource-manager-deployment-operations.md).</span><span class="sxs-lookup"><span data-stu-id="682cb-173">To get details of deployment operations and troubleshoot deployment errors with the Azure CLI, see [View deployment operations](resource-manager-deployment-operations.md).</span></span>
* <span data-ttu-id="682cb-174">Om du vill använda CLI för att ställa in ett program eller skript för att komma åt resurser, se [använder Azure CLI för att skapa ett huvudnamn för tjänsten att komma åt resurser](resource-group-authenticate-service-principal-cli.md).</span><span class="sxs-lookup"><span data-stu-id="682cb-174">If you want to use the CLI to set up an application or script to access resources, see [Use Azure CLI to create a service principal to access resources](resource-group-authenticate-service-principal-cli.md).</span></span>
* <span data-ttu-id="682cb-175">Vägledning för hur företag kan använda resurshanteraren för att effektivt hantera prenumerationer finns i [Azure enterprise scaffold - förebyggande prenumerationsåtgärder](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="682cb-175">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

