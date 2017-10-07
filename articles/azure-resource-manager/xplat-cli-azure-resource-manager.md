---
title: aaaManage resurser med hello Azure CLI | Microsoft Docs
description: "Använd hello Azure-kommandoradsgränssnittet (CLI) toomanage Azure resurser och grupper"
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
ms.openlocfilehash: 3df70e123d14d3bbf2648c71970bac1db4afc025
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-azure-cli-toomanage-azure-resources-and-resource-groups"></a><span data-ttu-id="1d87e-103">Använd hello Azure CLI toomanage Azure resurser och resursgrupper</span><span class="sxs-lookup"><span data-stu-id="1d87e-103">Use hello Azure CLI toomanage Azure resources and resource groups</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1d87e-104">Portal</span><span class="sxs-lookup"><span data-stu-id="1d87e-104">Portal</span></span>](resource-group-portal.md) 
> * [<span data-ttu-id="1d87e-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="1d87e-105">Azure CLI</span></span>](xplat-cli-azure-resource-manager.md)
> * [<span data-ttu-id="1d87e-106">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="1d87e-106">Azure PowerShell</span></span>](powershell-azure-resource-manager.md)
> * [<span data-ttu-id="1d87e-107">REST-API</span><span class="sxs-lookup"><span data-stu-id="1d87e-107">REST API</span></span>](resource-manager-rest-api.md)
> 
> 

<span data-ttu-id="1d87e-108">hello Azure-kommandoradsgränssnittet (Azure CLI) är en av flera verktyg du kan använda toodeploy och hantera resurser med Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="1d87e-108">hello Azure Command-Line Interface (Azure CLI) is one of several tools you can use toodeploy and manage resources with Resource Manager.</span></span> <span data-ttu-id="1d87e-109">Den här artikeln innehåller vanliga sätt toomanage Azure resurser och resursgrupper med hjälp av hello Azure CLI i Resource Manager-läge.</span><span class="sxs-lookup"><span data-stu-id="1d87e-109">This article introduces common ways toomanage Azure resources and resource groups by using hello Azure CLI in Resource Manager mode.</span></span> <span data-ttu-id="1d87e-110">Information om hur du använder hello CLI toodeploy resurser finns [distribuera resurser med Resource Manager-mallar och Azure CLI](resource-group-template-deploy-cli.md).</span><span class="sxs-lookup"><span data-stu-id="1d87e-110">For information about using hello CLI toodeploy resources, see [Deploy resources with Resource Manager templates and Azure CLI](resource-group-template-deploy-cli.md).</span></span> <span data-ttu-id="1d87e-111">Information om Azure-resurser och Resource Manager finns hello [översikt över Azure Resource Manager](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1d87e-111">For background about Azure resources and Resource Manager, visit hello [Azure Resource Manager Overview](resource-group-overview.md).</span></span>

> [!NOTE]
> <span data-ttu-id="1d87e-112">toomanage Azure resurser med hello Azure CLI, du behöver för[installera hello Azure CLI](../cli-install-nodejs.md), och [logga in tooAzure](../xplat-cli-connect.md) med hjälp av hello `azure login` kommando.</span><span class="sxs-lookup"><span data-stu-id="1d87e-112">toomanage Azure resources with hello Azure CLI, you need too[install hello Azure CLI](../cli-install-nodejs.md), and [log in tooAzure](../xplat-cli-connect.md) by using hello `azure login` command.</span></span> <span data-ttu-id="1d87e-113">Se till att hello CLI finns i Resource Manager-läget (kör `azure config mode arm`).</span><span class="sxs-lookup"><span data-stu-id="1d87e-113">Make sure hello CLI is in Resource Manager mode (run `azure config mode arm`).</span></span> <span data-ttu-id="1d87e-114">Om du har gjort detta kan är du klar toogo.</span><span class="sxs-lookup"><span data-stu-id="1d87e-114">If you've done these things, you're ready toogo.</span></span>
> 
> 

## <a name="get-resource-groups-and-resources"></a><span data-ttu-id="1d87e-115">Hämta resursgrupper och resurser</span><span class="sxs-lookup"><span data-stu-id="1d87e-115">Get resource groups and resources</span></span>
### <a name="resource-groups"></a><span data-ttu-id="1d87e-116">Resursgrupper</span><span class="sxs-lookup"><span data-stu-id="1d87e-116">Resource groups</span></span>
<span data-ttu-id="1d87e-117">tooget en lista över alla resursgrupper i din prenumeration och deras platser kör det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="1d87e-117">tooget a list of all resource groups in your subscription and their locations, run this command.</span></span>

    azure group list


### <a name="resources"></a><span data-ttu-id="1d87e-118">Resurser</span><span class="sxs-lookup"><span data-stu-id="1d87e-118">Resources</span></span>
 <span data-ttu-id="1d87e-119">toolist alla resurser i en grupp, till exempel en med namnet *testRG*, använda hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="1d87e-119">toolist all resources in a group, such as one with name *testRG*, use hello following command:</span></span>

    azure resource list testRG

<span data-ttu-id="1d87e-120">tooview en enskild resurs hello-gruppen, till exempel en virtuell dator med namnet *MyUbuntuVM*, använder du ett kommando som hello nedan:</span><span class="sxs-lookup"><span data-stu-id="1d87e-120">tooview an individual resource within hello group, such as a VM named *MyUbuntuVM*, use a command like hello following:</span></span>

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

<span data-ttu-id="1d87e-121">Meddelande hello **Microsoft.Compute/virtualMachines** parameter.</span><span class="sxs-lookup"><span data-stu-id="1d87e-121">Notice hello **Microsoft.Compute/virtualMachines** parameter.</span></span> <span data-ttu-id="1d87e-122">Den här parametern anger hello hello-resurs som du begär information.</span><span class="sxs-lookup"><span data-stu-id="1d87e-122">This parameter indicates hello type of hello resource you are requesting information on.</span></span>

> [!NOTE]
> <span data-ttu-id="1d87e-123">När du använder hello **azure-resurs** kommandon än hello **lista** kommandot måste du ange hello API-versionen av hello resursen med hello **-o** parameter.</span><span class="sxs-lookup"><span data-stu-id="1d87e-123">When using hello **azure resource** commands other than hello **list** command, you must specify hello API version of hello resource with hello **-o** parameter.</span></span> <span data-ttu-id="1d87e-124">Om du är osäker på om hello API-version, se hello mallfilen och hitta hello apiVersion fältet för hello resurs.</span><span class="sxs-lookup"><span data-stu-id="1d87e-124">If you're unsure about hello API version, consult hello template file and find hello apiVersion field for hello resource.</span></span> <span data-ttu-id="1d87e-125">Mer information om API-versioner i Resource Manager finns i [resursproviders och typer](resource-manager-supported-services.md).</span><span class="sxs-lookup"><span data-stu-id="1d87e-125">For more about API versions in Resource Manager, see [Resource providers and types](resource-manager-supported-services.md).</span></span>
> 
> 

<span data-ttu-id="1d87e-126">När du visar information på en resurs, är det ofta användbara toouse hello `--json` parameter.</span><span class="sxs-lookup"><span data-stu-id="1d87e-126">When viewing details on a resource, it is often useful toouse hello `--json` parameter.</span></span> <span data-ttu-id="1d87e-127">Denna parameter gör hello utdata mer lättläst eftersom vissa värden kapslade strukturer eller samlingar.</span><span class="sxs-lookup"><span data-stu-id="1d87e-127">This parameter makes hello output more readable, because some values are nested structures, or collections.</span></span> <span data-ttu-id="1d87e-128">hello det här exemplet returnerar hello resultaten av hello **visa** kommandot som JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="1d87e-128">hello following example demonstrates returning hello results of hello **show** command as a JSON document.</span></span>

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json

> [!NOTE]
> <span data-ttu-id="1d87e-129">Om du vill, spara hello JSON data toofile med hjälp av hello &gt; tooa för tecknet toodirect hello utdatafilen.</span><span class="sxs-lookup"><span data-stu-id="1d87e-129">If you want, save hello JSON data toofile by using hello &gt; character toodirect hello output tooa file.</span></span> <span data-ttu-id="1d87e-130">Exempel:</span><span class="sxs-lookup"><span data-stu-id="1d87e-130">For example:</span></span>
> 
> `azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json > myfile.json`
> 
> 

### <a name="tags"></a><span data-ttu-id="1d87e-131">Taggar</span><span class="sxs-lookup"><span data-stu-id="1d87e-131">Tags</span></span>
[!INCLUDE [resource-manager-tag-resources-cli](../../includes/resource-manager-tag-resources-cli.md)]

## <a name="manage-resources"></a><span data-ttu-id="1d87e-132">Hantera resurser</span><span class="sxs-lookup"><span data-stu-id="1d87e-132">Manage resources</span></span>
<span data-ttu-id="1d87e-133">tooadd en resurs, till exempel en storage-konto tooa resursgrupp, köra ett kommando som liknar:</span><span class="sxs-lookup"><span data-stu-id="1d87e-133">tooadd a resource such as a storage account tooa resource group, run a command similar to:</span></span>

    azure resource create testRG MyStorageAccount "Microsoft.Storage/storageAccounts" "westus" -o "2015-06-15" -p "{\"accountType\": \"Standard_LRS\"}"

<span data-ttu-id="1d87e-134">Dessutom toospecifying hello API-versionen av hello resurs med hello **-o** parametern, Använd hello **-p** parametern toopass en JSON-formaterad sträng med alla obligatoriska eller ytterligare egenskaper.</span><span class="sxs-lookup"><span data-stu-id="1d87e-134">In addition toospecifying hello API version of hello resource with hello **-o** parameter, use hello **-p** parameter toopass a JSON-formatted string with any required or additional properties.</span></span>

<span data-ttu-id="1d87e-135">toodelete en befintlig resurs, till exempel en virtuell datorresurs kommandot hello följande:</span><span class="sxs-lookup"><span data-stu-id="1d87e-135">toodelete an existing resource such as a virtual machine resource, use a command like hello following:</span></span>

    azure resource delete testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

<span data-ttu-id="1d87e-136">toomove befintliga resurser tooanother resursgrupp eller prenumeration, använda hello **azure-resurs flytta** kommando.</span><span class="sxs-lookup"><span data-stu-id="1d87e-136">toomove existing resources tooanother resource group or subscription, use hello **azure resource move** command.</span></span> <span data-ttu-id="1d87e-137">följande exempel visar hur hello toomove en Redis-Cache tooa ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="1d87e-137">hello following example shows how toomove a Redis Cache tooa new resource group.</span></span> <span data-ttu-id="1d87e-138">I hello **-i** parameter, ange en kommaavgränsad lista över hello resurs id toomove.</span><span class="sxs-lookup"><span data-stu-id="1d87e-138">In hello **-i** parameter, provide a comma-separated list of hello resource id's toomove.</span></span>

    azure resource move -i "/subscriptions/{guid}/resourceGroups/OldRG/providers/Microsoft.Cache/Redis/examplecache" -d "NewRG"

## <a name="control-access-tooresources"></a><span data-ttu-id="1d87e-139">Kontrollera åtkomst tooresources</span><span class="sxs-lookup"><span data-stu-id="1d87e-139">Control access tooresources</span></span>
<span data-ttu-id="1d87e-140">Du kan använda hello Azure CLI toocreate och hantera principer toocontrol åtkomst tooAzure resurser.</span><span class="sxs-lookup"><span data-stu-id="1d87e-140">You can use hello Azure CLI toocreate and manage policies toocontrol access tooAzure resources.</span></span> <span data-ttu-id="1d87e-141">Information om principdefinitioner och tilldela principer tooresources finns [använda princip toomanage resurser och styr åtkomsten](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="1d87e-141">For background about policy definitions and assigning policies tooresources, see [Use policy toomanage resources and control access](resource-manager-policy.md).</span></span>

<span data-ttu-id="1d87e-142">Till exempel definiera hello följa principen toodeny alla begäranden om platsen inte är västra USA eller norra centrala USA och spara den toohello princip definition filen policy.json:</span><span class="sxs-lookup"><span data-stu-id="1d87e-142">For example, define hello following policy toodeny all requests where location is not West US or North Central US, and save it toohello policy definition file policy.json:</span></span>

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

<span data-ttu-id="1d87e-143">Kör sedan hello **principdefinitionen skapa** kommando:</span><span class="sxs-lookup"><span data-stu-id="1d87e-143">Then run hello **policy definition create** command:</span></span>

    azure policy definition create MyPolicy -p c:\temp\policy.json

<span data-ttu-id="1d87e-144">Det här kommandot visar utdata liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="1d87e-144">This command shows output similar toohello following:</span></span>

    + <span data-ttu-id="1d87e-145">Skapar principdefinitionen MyPolicy data: Principnamn: MyPolicy data: PolicyDefinitionId: /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy</span><span class="sxs-lookup"><span data-stu-id="1d87e-145">Creating policy definition MyPolicy data:    PolicyName:             MyPolicy data:    PolicyDefinitionId:     /subscriptions/########-####-####-####-############/providers/Microsoft.Authorization/policyDefinitions/MyPolicy</span></span>

    <span data-ttu-id="1d87e-146">data: PolicyType: anpassade data: DisplayName: Odefinierad data: beskrivning: Odefinierad data: PolicyRule: fältet = plats, i = [westus northcentralus], gälla = neka</span><span class="sxs-lookup"><span data-stu-id="1d87e-146">data:    PolicyType:             Custom data:    DisplayName:            undefined data:    Description:            undefined data:    PolicyRule:             field=location, in=[westus, northcentralus], effect=deny</span></span>

 <span data-ttu-id="1d87e-147">tooassign en princip definitionsområdet hello du vill använda hello **PolicyDefinitionId** returnerades från föregående hello-kommando.</span><span class="sxs-lookup"><span data-stu-id="1d87e-147">tooassign a policy at hello scope you want, use hello **PolicyDefinitionId** returned from hello previous command.</span></span> <span data-ttu-id="1d87e-148">I följande exempel hello, detta scope hello prenumeration, men du kan definiera tooresource grupper eller enskilda resurser:</span><span class="sxs-lookup"><span data-stu-id="1d87e-148">In hello following example, this scope is hello subscription, but you can scope tooresource groups or individual resources:</span></span>

    <span data-ttu-id="1d87e-149">Azure principtilldelningens skapa MyPolicyAssignment -p /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy -s /subscriptions/###-###-###-###-### /</span><span class="sxs-lookup"><span data-stu-id="1d87e-149">azure policy assignment create MyPolicyAssignment -p /subscriptions/########-####-####-####-############/providers/Microsoft.Authorization/policyDefinitions/MyPolicy -s /subscriptions/########-####-####-####-############/</span></span>

<span data-ttu-id="1d87e-150">Du kan hämta, ändra eller ta bort principdefinitioner med hello **princip definition visa**, **definition principuppsättningen**, och **principdefinitionen ta bort** kommandon.</span><span class="sxs-lookup"><span data-stu-id="1d87e-150">You can get, change, or remove policy definitions by using hello **policy definition show**, **policy definition set**, and **policy definition delete** commands.</span></span>

<span data-ttu-id="1d87e-151">På samma sätt kan du kan hämta, ändra eller ta bort principtilldelningar med hello **princip tilldelningen visa**, **tilldelning principuppsättningen**, och **principtilldelning ta bort** kommandon .</span><span class="sxs-lookup"><span data-stu-id="1d87e-151">Similarly, you can get, change, or remove policy assignments by using hello **policy assignment show**, **policy assignment set**, and **policy assignment delete** commands.</span></span>

## <a name="export-a-resource-group-as-a-template"></a><span data-ttu-id="1d87e-152">Exportera en resursgrupp som en mall</span><span class="sxs-lookup"><span data-stu-id="1d87e-152">Export a resource group as a template</span></span>
<span data-ttu-id="1d87e-153">Du kan visa hello Resource Manager-mall för hello resursgrupp för en befintlig resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="1d87e-153">For an existing resource group, you can view hello Resource Manager template for hello resource group.</span></span> <span data-ttu-id="1d87e-154">Exporterar hello mallen har två fördelar:</span><span class="sxs-lookup"><span data-stu-id="1d87e-154">Exporting hello template offers two benefits:</span></span>

1. <span data-ttu-id="1d87e-155">Enkelt kan du automatisera framtida distributioner av hello lösning eftersom alla hello-infrastruktur är definierad i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="1d87e-155">You can easily automate future deployments of hello solution because all hello infrastructure is defined in hello template.</span></span>
2. <span data-ttu-id="1d87e-156">Du kan bekanta dig med mallens syntax genom att titta på hello JSON som representerar din lösning.</span><span class="sxs-lookup"><span data-stu-id="1d87e-156">You can become familiar with template syntax by looking at hello JSON that represents your solution.</span></span>

<span data-ttu-id="1d87e-157">Använder hello Azure CLI, kan du antingen exportera en mall som representerar hello aktuell status för din resursgrupp eller hämta hello-mallen som användes för en viss distribution.</span><span class="sxs-lookup"><span data-stu-id="1d87e-157">Using hello Azure CLI, you can either export a template that represents hello current state of your resource group, or download hello template that was used for a particular deployment.</span></span>

* <span data-ttu-id="1d87e-158">**Exportera hello mall för en resursgrupp** -det här är användbart när du har gjort ändringar tooa resursgrupp och måste tooretrieve hello JSON-representation av det aktuella tillståndet.</span><span class="sxs-lookup"><span data-stu-id="1d87e-158">**Export hello template for a resource group** - This is helpful when you made changes tooa resource group, and need tooretrieve hello JSON representation of its current state.</span></span> <span data-ttu-id="1d87e-159">Hello genererade mallen innehåller dock endast ett minimalt antal parametrar och inga variabler.</span><span class="sxs-lookup"><span data-stu-id="1d87e-159">However, hello generated template contains only a minimal number of parameters and no variables.</span></span> <span data-ttu-id="1d87e-160">De flesta hello värden i hello mallen är hårdkodat.</span><span class="sxs-lookup"><span data-stu-id="1d87e-160">Most of hello values in hello template are hard-coded.</span></span> <span data-ttu-id="1d87e-161">Innan du distribuerar hello genereras mallen du tooconvert mer hello värden till parametrar så att du kan anpassa hello distribution för olika miljöer.</span><span class="sxs-lookup"><span data-stu-id="1d87e-161">Before deploying hello generated template, you may wish tooconvert more of hello values into parameters so you can customize hello deployment for different environments.</span></span>
  
    <span data-ttu-id="1d87e-162">tooexport hello mall för en resurs grupp tooa lokal katalog, kör hello `azure group export` som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="1d87e-162">tooexport hello template for a resource group tooa local directory, run hello `azure group export` command as shown in hello following example.</span></span> <span data-ttu-id="1d87e-163">(I stället använda en lokal katalog som är lämpliga för din operativsystemmiljö.)</span><span class="sxs-lookup"><span data-stu-id="1d87e-163">(Substitute a local directory appropriate for your operating system environment.)</span></span>
  
        azure group export testRG ~/azure/templates/
* <span data-ttu-id="1d87e-164">**Hämta hello mall för en viss distribution** – det här är användbart när du behöver tooview hello faktiska mall som har använt toodeploy resurser.</span><span class="sxs-lookup"><span data-stu-id="1d87e-164">**Download hello template for a particular deployment** -- This is helpful when you need tooview hello actual template that was used toodeploy resources.</span></span> <span data-ttu-id="1d87e-165">hello mallen innehåller alla parametrar och variabler som definieras för hello ursprungliga distributionen.</span><span class="sxs-lookup"><span data-stu-id="1d87e-165">hello template includes all parameters and variables defined for hello original deployment.</span></span> <span data-ttu-id="1d87e-166">Om någon i din organisation har gjort ändringar toohello resursgruppen utanför hello definition i hello mallen representerar inte den här mallen hello hello resursgruppen aktuella tillstånd.</span><span class="sxs-lookup"><span data-stu-id="1d87e-166">However, if someone in your organization made changes toohello resource group outside of hello definition in hello template, this template doesn't represent hello current state of hello resource group.</span></span>
  
    <span data-ttu-id="1d87e-167">toodownload hello mall som används för en viss distribution tooa lokal katalog, kör hello `azure group deployment template download` kommando.</span><span class="sxs-lookup"><span data-stu-id="1d87e-167">toodownload hello template used for a particular deployment tooa local directory, run hello `azure group deployment template download` command.</span></span> <span data-ttu-id="1d87e-168">Exempel:</span><span class="sxs-lookup"><span data-stu-id="1d87e-168">For example:</span></span>
  
        azure group deployment template download TestRG testRGDeploy ~/azure/templates/downloads/

> [!NOTE]
> <span data-ttu-id="1d87e-169">Exportera mallen är en förhandsversion och inte alla typer av resurser stöd för närvarande inte exportera en mall.</span><span class="sxs-lookup"><span data-stu-id="1d87e-169">Template export is in preview, and not all resource types currently support exporting a template.</span></span> <span data-ttu-id="1d87e-170">Ett felmeddelande som anger att vissa resurser inte har exporterats kan visas vid försök tooexport en mall.</span><span class="sxs-lookup"><span data-stu-id="1d87e-170">When attempting tooexport a template, you may see an error that states some resources were not exported.</span></span> <span data-ttu-id="1d87e-171">Om det behövs, manuellt definiera resurserna i mallen när du har hämtat den.</span><span class="sxs-lookup"><span data-stu-id="1d87e-171">If needed, manually define these resources in your template after downloading it.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="1d87e-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1d87e-172">Next steps</span></span>
* <span data-ttu-id="1d87e-173">tooget information om distributionsåtgärder och felsöka distribution med hello Azure CLI, se [visa distributionsåtgärder](resource-manager-deployment-operations.md).</span><span class="sxs-lookup"><span data-stu-id="1d87e-173">tooget details of deployment operations and troubleshoot deployment errors with hello Azure CLI, see [View deployment operations](resource-manager-deployment-operations.md).</span></span>
* <span data-ttu-id="1d87e-174">Om du vill toouse hello CLI tooset upp ett program eller skript tooaccess resurser, se [Använd Azure CLI toocreate ett huvudnamn för tjänsten tooaccess resurser](resource-group-authenticate-service-principal-cli.md).</span><span class="sxs-lookup"><span data-stu-id="1d87e-174">If you want toouse hello CLI tooset up an application or script tooaccess resources, see [Use Azure CLI toocreate a service principal tooaccess resources](resource-group-authenticate-service-principal-cli.md).</span></span>
* <span data-ttu-id="1d87e-175">Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="1d87e-175">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

