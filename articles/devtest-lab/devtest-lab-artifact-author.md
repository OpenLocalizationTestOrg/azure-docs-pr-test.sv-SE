---
title: "aaaCreate anpassade artefakter för den virtuella datorn med DevTest Labs | Microsoft Docs"
description: "Lär dig hur tooauthor egna artefakter för hjälp med DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 32dcdc61-ec23-4a01-b731-78c029ea5316
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2017
ms.author: tarcher
ms.openlocfilehash: 2bd603bc1241ca6b669a3a276a677729514f0df2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-custom-artifacts-for-your-devtest-labs-vm"></a><span data-ttu-id="d03d1-103">Skapa anpassade artefakter för den virtuella datorn med DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="d03d1-103">Create custom artifacts for your DevTest Labs VM</span></span>
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/how-to-author-custom-artifacts/player]
> 
> 

## <a name="overview"></a><span data-ttu-id="d03d1-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="d03d1-104">Overview</span></span>
<span data-ttu-id="d03d1-105">**Artefakter** är används toodeploy och konfigurera ditt program när en virtuell dator har etablerats.</span><span class="sxs-lookup"><span data-stu-id="d03d1-105">**Artifacts** are used toodeploy and configure your application after a VM is provisioned.</span></span> <span data-ttu-id="d03d1-106">En artefakt består av en artefakt definitionsfilen och andra filer som lagras i en mapp i en git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="d03d1-106">An artifact consists of an artifact definition file and other script files that are stored in a folder in a git repository.</span></span> <span data-ttu-id="d03d1-107">Artefakt definitionsfiler består av JSON och uttryck som du kan använda toospecify vad du vill tooinstall på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="d03d1-107">Artifact definition files consist of JSON and expressions that you can use toospecify what you want tooinstall on a VM.</span></span> <span data-ttu-id="d03d1-108">Du kan till exempel definiera hello namnet på en artefakt, kommandot toorun och parametrar som är tillgängliga när hello kommandot körs.</span><span class="sxs-lookup"><span data-stu-id="d03d1-108">For example, you can define hello name of an artifact, command toorun, and parameters that are made available when hello command is run.</span></span> <span data-ttu-id="d03d1-109">Du kan se tooother skriptfiler i definitionsfilen för hello artefakt efter namn.</span><span class="sxs-lookup"><span data-stu-id="d03d1-109">You can refer tooother script files within hello artifact definition file by name.</span></span>

## <a name="artifact-definition-file-format"></a><span data-ttu-id="d03d1-110">Artefakt format för paketdefinitionsfiler</span><span class="sxs-lookup"><span data-stu-id="d03d1-110">Artifact definition file format</span></span>
<span data-ttu-id="d03d1-111">hello visar följande exempel hello avsnitt som utgör hello grundläggande struktur för en definitionsfil:</span><span class="sxs-lookup"><span data-stu-id="d03d1-111">hello following example shows hello sections that make up hello basic structure of a definition file:</span></span>

    {
      "$schema": "https://raw.githubusercontent.com/Azure/azure-devtestlab/master/schemas/2016-11-28/dtlArtifacts.json",
      "title": "",
      "description": "",
      "iconUri": "",
      "targetOsType": "",
      "parameters": {
        "<parameterName>": {
          "type": "",
          "displayName": "",
          "description": ""
        }
      },
      "runCommand": {
        "commandToExecute": ""
      }
    }

| <span data-ttu-id="d03d1-112">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="d03d1-112">Element name</span></span> | <span data-ttu-id="d03d1-113">Krävs?</span><span class="sxs-lookup"><span data-stu-id="d03d1-113">Required?</span></span> | <span data-ttu-id="d03d1-114">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="d03d1-114">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d03d1-115">$schema</span><span class="sxs-lookup"><span data-stu-id="d03d1-115">$schema</span></span> |<span data-ttu-id="d03d1-116">Nej</span><span class="sxs-lookup"><span data-stu-id="d03d1-116">No</span></span> |<span data-ttu-id="d03d1-117">Sökväg till hello JSON-schema som hjälper till att testa hello giltighet hello definitionsfilen.</span><span class="sxs-lookup"><span data-stu-id="d03d1-117">Location of hello JSON schema file that helps in testing hello validity of hello definition file.</span></span> |
| <span data-ttu-id="d03d1-118">Rubrik</span><span class="sxs-lookup"><span data-stu-id="d03d1-118">title</span></span> |<span data-ttu-id="d03d1-119">Ja</span><span class="sxs-lookup"><span data-stu-id="d03d1-119">Yes</span></span> |<span data-ttu-id="d03d1-120">Namnet på hello artefakt som visas i hello labbet.</span><span class="sxs-lookup"><span data-stu-id="d03d1-120">Name of hello artifact displayed in hello lab.</span></span> |
| <span data-ttu-id="d03d1-121">description</span><span class="sxs-lookup"><span data-stu-id="d03d1-121">description</span></span> |<span data-ttu-id="d03d1-122">Ja</span><span class="sxs-lookup"><span data-stu-id="d03d1-122">Yes</span></span> |<span data-ttu-id="d03d1-123">Beskrivning av hello artefakt som visas i hello labbet.</span><span class="sxs-lookup"><span data-stu-id="d03d1-123">Description of hello artifact displayed in hello lab.</span></span> |
| <span data-ttu-id="d03d1-124">iconUri</span><span class="sxs-lookup"><span data-stu-id="d03d1-124">iconUri</span></span> |<span data-ttu-id="d03d1-125">Nej</span><span class="sxs-lookup"><span data-stu-id="d03d1-125">No</span></span> |<span data-ttu-id="d03d1-126">URI för hello-ikonen som visas i hello labbet.</span><span class="sxs-lookup"><span data-stu-id="d03d1-126">Uri of hello icon displayed in hello lab.</span></span> |
| <span data-ttu-id="d03d1-127">targetOsType</span><span class="sxs-lookup"><span data-stu-id="d03d1-127">targetOsType</span></span> |<span data-ttu-id="d03d1-128">Ja</span><span class="sxs-lookup"><span data-stu-id="d03d1-128">Yes</span></span> |<span data-ttu-id="d03d1-129">Operativsystemet på hello VM där artefakt är installerad.</span><span class="sxs-lookup"><span data-stu-id="d03d1-129">Operating system of hello VM where artifact is installed.</span></span> <span data-ttu-id="d03d1-130">Alternativ som stöds är: Windows och Linux.</span><span class="sxs-lookup"><span data-stu-id="d03d1-130">Supported options are: Windows and Linux.</span></span> |
| <span data-ttu-id="d03d1-131">parameters</span><span class="sxs-lookup"><span data-stu-id="d03d1-131">parameters</span></span> |<span data-ttu-id="d03d1-132">Nej</span><span class="sxs-lookup"><span data-stu-id="d03d1-132">No</span></span> |<span data-ttu-id="d03d1-133">Värden som tillhandahålls när artefakt installationskommando körs på en dator.</span><span class="sxs-lookup"><span data-stu-id="d03d1-133">Values that are provided when artifact install command is run on a machine.</span></span> <span data-ttu-id="d03d1-134">På så sätt kan anpassa din artefakt.</span><span class="sxs-lookup"><span data-stu-id="d03d1-134">This helps in customizing your artifact.</span></span> |
| <span data-ttu-id="d03d1-135">Kör kommando</span><span class="sxs-lookup"><span data-stu-id="d03d1-135">runCommand</span></span> |<span data-ttu-id="d03d1-136">Ja</span><span class="sxs-lookup"><span data-stu-id="d03d1-136">Yes</span></span> |<span data-ttu-id="d03d1-137">Artefakt installationskommando som körs på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="d03d1-137">Artifact install command that is executed on a VM.</span></span> |

### <a name="artifact-parameters"></a><span data-ttu-id="d03d1-138">Artefakt parametrar</span><span class="sxs-lookup"><span data-stu-id="d03d1-138">Artifact parameters</span></span>
<span data-ttu-id="d03d1-139">I hello parametrar i definitionsfilen för hello kan ange du vilka värden som en användare kan ange när du installerar en artefakt.</span><span class="sxs-lookup"><span data-stu-id="d03d1-139">In hello parameters section of hello definition file, you specify which values a user can input when installing an artifact.</span></span> <span data-ttu-id="d03d1-140">Du kan se toothese värden i hello artefakt installationskommando.</span><span class="sxs-lookup"><span data-stu-id="d03d1-140">You can refer toothese values in hello artifact install command.</span></span>

<span data-ttu-id="d03d1-141">Du kan definiera parametrar med hello följande struktur:</span><span class="sxs-lookup"><span data-stu-id="d03d1-141">You define parameters with hello following structure:</span></span>

    "parameters": {
        "<parameterName>": {
          "type": "<type-of-parameter-value>",
          "displayName": "<display-name-of-parameter>",
          "description": "<description-of-parameter>"
        }
      }

| <span data-ttu-id="d03d1-142">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="d03d1-142">Element name</span></span> | <span data-ttu-id="d03d1-143">Krävs?</span><span class="sxs-lookup"><span data-stu-id="d03d1-143">Required?</span></span> | <span data-ttu-id="d03d1-144">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="d03d1-144">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d03d1-145">typ</span><span class="sxs-lookup"><span data-stu-id="d03d1-145">type</span></span> |<span data-ttu-id="d03d1-146">Ja</span><span class="sxs-lookup"><span data-stu-id="d03d1-146">Yes</span></span> |<span data-ttu-id="d03d1-147">Typ av parametervärdet.</span><span class="sxs-lookup"><span data-stu-id="d03d1-147">Type of parameter value.</span></span> <span data-ttu-id="d03d1-148">Se följande lista för hello tillåtna typer hello:</span><span class="sxs-lookup"><span data-stu-id="d03d1-148">See hello following list for hello allowed types:</span></span> |
| <span data-ttu-id="d03d1-149">Visningsnamn</span><span class="sxs-lookup"><span data-stu-id="d03d1-149">displayName</span></span> |<span data-ttu-id="d03d1-150">Ja</span><span class="sxs-lookup"><span data-stu-id="d03d1-150">Yes</span></span> |<span data-ttu-id="d03d1-151">Namnet på hello parameter visas tooa användaren i hello labbet.</span><span class="sxs-lookup"><span data-stu-id="d03d1-151">Name of hello parameter that is displayed tooa user in hello lab.</span></span> | |
| <span data-ttu-id="d03d1-152">description</span><span class="sxs-lookup"><span data-stu-id="d03d1-152">description</span></span> |<span data-ttu-id="d03d1-153">Ja</span><span class="sxs-lookup"><span data-stu-id="d03d1-153">Yes</span></span> |<span data-ttu-id="d03d1-154">Beskrivning av hello-parameter som visas i hello labbet.</span><span class="sxs-lookup"><span data-stu-id="d03d1-154">Description of hello parameter that is displayed in hello lab.</span></span> |

<span data-ttu-id="d03d1-155">hello tillåtna typer är:</span><span class="sxs-lookup"><span data-stu-id="d03d1-155">hello allowed types are:</span></span>

* <span data-ttu-id="d03d1-156">strängen – en giltig JSON-sträng</span><span class="sxs-lookup"><span data-stu-id="d03d1-156">string – any valid JSON string</span></span>
* <span data-ttu-id="d03d1-157">int – alla giltiga JSON-heltal</span><span class="sxs-lookup"><span data-stu-id="d03d1-157">int – any valid JSON integer</span></span>
* <span data-ttu-id="d03d1-158">bool – alla giltiga JSON-booleska</span><span class="sxs-lookup"><span data-stu-id="d03d1-158">bool – any valid JSON Boolean</span></span>
* <span data-ttu-id="d03d1-159">matrisen – en giltig JSON-matris</span><span class="sxs-lookup"><span data-stu-id="d03d1-159">array – any valid JSON array</span></span>

## <a name="artifact-expressions-and-functions"></a><span data-ttu-id="d03d1-160">Artefakt uttryck och funktioner</span><span class="sxs-lookup"><span data-stu-id="d03d1-160">Artifact expressions and functions</span></span>
<span data-ttu-id="d03d1-161">Du kan använda uttryck och funktioner tooconstruct hello artefakt installationskommando.</span><span class="sxs-lookup"><span data-stu-id="d03d1-161">You can use expression and functions tooconstruct hello artifact install command.</span></span>
<span data-ttu-id="d03d1-162">Uttryck är omges av hakparenteser ([och]) och utvärderas när hello artefakt installeras.</span><span class="sxs-lookup"><span data-stu-id="d03d1-162">Expressions are enclosed with brackets ([ and ]), and are evaluated when hello artifact is installed.</span></span> <span data-ttu-id="d03d1-163">Uttryck kan finnas var som helst i ett strängvärde i JSON och returnerar alltid en annan JSON-värde.</span><span class="sxs-lookup"><span data-stu-id="d03d1-163">Expressions can appear anywhere in a JSON string value and always return another JSON value.</span></span> <span data-ttu-id="d03d1-164">Om du behöver toouse en teckensträng som börjar med en hakparentes [, måste du använda två hakparenteser [[.</span><span class="sxs-lookup"><span data-stu-id="d03d1-164">If you need toouse a literal string that starts with a bracket [, you must use two brackets [[.</span></span>
<span data-ttu-id="d03d1-165">Normalt använder du uttryck med funktioner tooconstruct ett värde.</span><span class="sxs-lookup"><span data-stu-id="d03d1-165">Typically, you use expressions with functions tooconstruct a value.</span></span> <span data-ttu-id="d03d1-166">Precis som i JavaScript är-funktionsanrop som formaterade som functionName(arg1,arg2,arg3).</span><span class="sxs-lookup"><span data-stu-id="d03d1-166">Just like in JavaScript, function calls are formatted as functionName(arg1,arg2,arg3).</span></span>

<span data-ttu-id="d03d1-167">hello visas nedan vanliga funktioner:</span><span class="sxs-lookup"><span data-stu-id="d03d1-167">hello following list shows common functions:</span></span>

* <span data-ttu-id="d03d1-168">parameters(parameterName) - returnerar ett parametervärde som tillhandahålls vid hello artefakt kommandot körs.</span><span class="sxs-lookup"><span data-stu-id="d03d1-168">parameters(parameterName) - Returns a parameter value that is provided when hello artifact command is run.</span></span>
* <span data-ttu-id="d03d1-169">sammanfoga (arg1, arg2, arg3,...) - kombinerar flera strängvärden.</span><span class="sxs-lookup"><span data-stu-id="d03d1-169">concat(arg1,arg2,arg3, …..) -     Combines multiple string values.</span></span> <span data-ttu-id="d03d1-170">Den här funktionen kan ta valfritt antal argument.</span><span class="sxs-lookup"><span data-stu-id="d03d1-170">This function can take any number of arguments.</span></span>

<span data-ttu-id="d03d1-171">följande exempel visar hur hello toouse uttryck och funktioner tooconstruct ett värde:</span><span class="sxs-lookup"><span data-stu-id="d03d1-171">hello following example shows how toouse expression and functions tooconstruct a value:</span></span>

    runCommand": {
         "commandToExecute": "[concat('powershell.exe -ExecutionPolicy bypass \"& ./startChocolatey.ps1'
    , ' -RawPackagesList ', parameters('packages')
    , ' -Username ', parameters('installUsername')
    , ' -Password ', parameters('installPassword'))]"
    }

## <a name="create-a-custom-artifact"></a><span data-ttu-id="d03d1-172">Skapa en anpassad artefakt</span><span class="sxs-lookup"><span data-stu-id="d03d1-172">Create a custom artifact</span></span>
<span data-ttu-id="d03d1-173">Skapa din egen artefakt genom att följa dessa steg:</span><span class="sxs-lookup"><span data-stu-id="d03d1-173">Create your custom artifact by following these steps:</span></span>

1. <span data-ttu-id="d03d1-174">Installera en JSON-redigerare - du behöver en JSON-redigerare toowork med artefakt definitionsfilerna.</span><span class="sxs-lookup"><span data-stu-id="d03d1-174">Install a JSON editor - You need a JSON editor toowork with artifact definition files.</span></span> <span data-ttu-id="d03d1-175">Vi rekommenderar att du använder [Visual Studio Code](https://code.visualstudio.com/), som är tillgänglig för Windows, Linux och OS X.</span><span class="sxs-lookup"><span data-stu-id="d03d1-175">We recommend using [Visual Studio Code](https://code.visualstudio.com/), which is available for Windows, Linux and OS X.</span></span>
2. <span data-ttu-id="d03d1-176">Hämta en exempel-artifactfile.json - checka ut hello artefakter som skapats av Azure DevTest Labs-teamet på vår [GitHub-lagringsplatsen](https://github.com/Azure/azure-devtestlab), där vi har skapat ett omfattande Meddelandebibliotek artefakter som hjälper dig skapa dina egna artefakter.</span><span class="sxs-lookup"><span data-stu-id="d03d1-176">Get a sample artifactfile.json - Check out hello artifacts created by Azure DevTest Labs team at our [GitHub repository](https://github.com/Azure/azure-devtestlab), where we have created a rich library of artifacts that help you create your own artifacts.</span></span> <span data-ttu-id="d03d1-177">Hämta en artefakt definitionsfilen och göra ändringar tooit toocreate egna artefakter.</span><span class="sxs-lookup"><span data-stu-id="d03d1-177">Download an artifact definition file and make changes tooit toocreate your own artifacts.</span></span>
3. <span data-ttu-id="d03d1-178">Genom att utnyttja IntelliSense - utnyttjar IntelliSense toosee giltiga element som kan använda tooconstruct en artefakt definitionsfilen.</span><span class="sxs-lookup"><span data-stu-id="d03d1-178">Make use of IntelliSense - Leverage IntelliSense toosee valid elements that can be used tooconstruct an artifact definition file.</span></span> <span data-ttu-id="d03d1-179">Du kan också se hello olika alternativ för värden i ett element.</span><span class="sxs-lookup"><span data-stu-id="d03d1-179">You can also see hello different options for values of an element.</span></span> <span data-ttu-id="d03d1-180">Till exempel IntelliSense visar du hello två val av Windows eller Linux när du redigerar hello **targetOsType** element.</span><span class="sxs-lookup"><span data-stu-id="d03d1-180">For example, IntelliSense show you hello two choices of Windows or Linux when editing hello **targetOsType** element.</span></span>
4. <span data-ttu-id="d03d1-181">Store hello artefakt i en [git-lagringsplats](devtest-lab-add-artifact-repo.md).</span><span class="sxs-lookup"><span data-stu-id="d03d1-181">Store hello artifact in a [git repository](devtest-lab-add-artifact-repo.md).</span></span>
   
   1. <span data-ttu-id="d03d1-182">Skapa en separat katalog för varje artefakt där hello katalognamnet är hello samma som hello artefakt namn.</span><span class="sxs-lookup"><span data-stu-id="d03d1-182">Create a separate directory for each artifact where hello directory name is hello same as hello artifact name.</span></span>
   2. <span data-ttu-id="d03d1-183">Lagra hello artefakt-definitionsfil (artifactfile.json) i hello-katalogen som du skapade.</span><span class="sxs-lookup"><span data-stu-id="d03d1-183">Store hello artifact definition file (artifactfile.json) in hello directory you created.</span></span>
   3. <span data-ttu-id="d03d1-184">Store hello skripten från hello artefakt installationskommando.</span><span class="sxs-lookup"><span data-stu-id="d03d1-184">Store hello scripts that are referenced from hello artifact install command.</span></span>
      
      <span data-ttu-id="d03d1-185">Här är ett exempel på hur en artefakt-mappen kan se ut:</span><span class="sxs-lookup"><span data-stu-id="d03d1-185">Here is an example of how an artifact folder might look:</span></span>
      
      ![Exempel för artefakt git repo](./media/devtest-lab-artifact-author/git-repo.png)
5. <span data-ttu-id="d03d1-187">Lägga till hello artefakter databasen toohello lab: finns toohello artikel [lägga till en Git-lagringsplats för artefakter och mallar](devtest-lab-add-artifact-repo.md).</span><span class="sxs-lookup"><span data-stu-id="d03d1-187">Add hello artifacts repository toohello lab - Refer toohello article, [Add a Git repository for artifacts and templates](devtest-lab-add-artifact-repo.md).</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-articles"></a><span data-ttu-id="d03d1-188">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="d03d1-188">Related articles</span></span>
* [<span data-ttu-id="d03d1-189">Hur toodiagnose artefakt fel i DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="d03d1-189">How toodiagnose artifact failures in DevTest Labs</span></span>](devtest-lab-troubleshoot-artifact-failure.md)
* [<span data-ttu-id="d03d1-190">Ansluta till en VM-tooexisting AD-domän med hjälp av en resource manager-mall i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="d03d1-190">Join a VM tooexisting AD Domain using a resource manager template in Azure DevTest Labs</span></span>](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

## <a name="next-steps"></a><span data-ttu-id="d03d1-191">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d03d1-191">Next steps</span></span>
* <span data-ttu-id="d03d1-192">Lär dig hur för[lägga till ett Git artefakt databasen tooa labb](devtest-lab-add-artifact-repo.md).</span><span class="sxs-lookup"><span data-stu-id="d03d1-192">Learn how too[add a Git artifact repository tooa lab](devtest-lab-add-artifact-repo.md).</span></span>

