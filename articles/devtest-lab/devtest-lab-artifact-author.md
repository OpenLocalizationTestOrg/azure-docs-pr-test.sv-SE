---
title: "Skapa anpassade artefakter för den virtuella datorn med DevTest Labs | Microsoft Docs"
description: "Lär dig hur du skapar egna artefakter för användning med DevTest Labs"
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
ms.openlocfilehash: 2412033daa1d97860dd9f380178622b1ddc590c0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-custom-artifacts-for-your-devtest-labs-vm"></a><span data-ttu-id="44be9-103">Skapa anpassade artefakter för den virtuella datorn med DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="44be9-103">Create custom artifacts for your DevTest Labs VM</span></span>
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/how-to-author-custom-artifacts/player]
> 
> 

## <a name="overview"></a><span data-ttu-id="44be9-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="44be9-104">Overview</span></span>
<span data-ttu-id="44be9-105">**Artefakter** används för att distribuera och konfigurera ditt program när en virtuell dator har etablerats.</span><span class="sxs-lookup"><span data-stu-id="44be9-105">**Artifacts** are used to deploy and configure your application after a VM is provisioned.</span></span> <span data-ttu-id="44be9-106">En artefakt består av en artefakt definitionsfilen och andra filer som lagras i en mapp i en git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="44be9-106">An artifact consists of an artifact definition file and other script files that are stored in a folder in a git repository.</span></span> <span data-ttu-id="44be9-107">Artefakt definitionsfiler består av JSON och uttryck som du kan använda för att ange vad du vill installera på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="44be9-107">Artifact definition files consist of JSON and expressions that you can use to specify what you want to install on a VM.</span></span> <span data-ttu-id="44be9-108">Du kan till exempel definiera namnet på en artefakt kommando som ska köras och parametrar som är tillgängliga när du kör kommandot.</span><span class="sxs-lookup"><span data-stu-id="44be9-108">For example, you can define the name of an artifact, command to run, and parameters that are made available when the command is run.</span></span> <span data-ttu-id="44be9-109">Du kan referera till andra filer i definitionsfilen artefakt efter namn.</span><span class="sxs-lookup"><span data-stu-id="44be9-109">You can refer to other script files within the artifact definition file by name.</span></span>

## <a name="artifact-definition-file-format"></a><span data-ttu-id="44be9-110">Artefakt format för paketdefinitionsfiler</span><span class="sxs-lookup"><span data-stu-id="44be9-110">Artifact definition file format</span></span>
<span data-ttu-id="44be9-111">I följande exempel visas de avsnitt som utgör den grundläggande strukturen i en definitionsfil:</span><span class="sxs-lookup"><span data-stu-id="44be9-111">The following example shows the sections that make up the basic structure of a definition file:</span></span>

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

| <span data-ttu-id="44be9-112">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="44be9-112">Element name</span></span> | <span data-ttu-id="44be9-113">Krävs?</span><span class="sxs-lookup"><span data-stu-id="44be9-113">Required?</span></span> | <span data-ttu-id="44be9-114">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="44be9-114">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="44be9-115">$schema</span><span class="sxs-lookup"><span data-stu-id="44be9-115">$schema</span></span> |<span data-ttu-id="44be9-116">Nej</span><span class="sxs-lookup"><span data-stu-id="44be9-116">No</span></span> |<span data-ttu-id="44be9-117">Plats för JSON-schemafilen som hjälper till att testa giltighet definitionsfilen.</span><span class="sxs-lookup"><span data-stu-id="44be9-117">Location of the JSON schema file that helps in testing the validity of the definition file.</span></span> |
| <span data-ttu-id="44be9-118">Rubrik</span><span class="sxs-lookup"><span data-stu-id="44be9-118">title</span></span> |<span data-ttu-id="44be9-119">Ja</span><span class="sxs-lookup"><span data-stu-id="44be9-119">Yes</span></span> |<span data-ttu-id="44be9-120">Namnet på den artefakt som visas i labbet.</span><span class="sxs-lookup"><span data-stu-id="44be9-120">Name of the artifact displayed in the lab.</span></span> |
| <span data-ttu-id="44be9-121">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="44be9-121">description</span></span> |<span data-ttu-id="44be9-122">Ja</span><span class="sxs-lookup"><span data-stu-id="44be9-122">Yes</span></span> |<span data-ttu-id="44be9-123">Beskrivning av artefakt som visas i labbet.</span><span class="sxs-lookup"><span data-stu-id="44be9-123">Description of the artifact displayed in the lab.</span></span> |
| <span data-ttu-id="44be9-124">iconUri</span><span class="sxs-lookup"><span data-stu-id="44be9-124">iconUri</span></span> |<span data-ttu-id="44be9-125">Nej</span><span class="sxs-lookup"><span data-stu-id="44be9-125">No</span></span> |<span data-ttu-id="44be9-126">URI för den ikon som visas i labbet.</span><span class="sxs-lookup"><span data-stu-id="44be9-126">Uri of the icon displayed in the lab.</span></span> |
| <span data-ttu-id="44be9-127">targetOsType</span><span class="sxs-lookup"><span data-stu-id="44be9-127">targetOsType</span></span> |<span data-ttu-id="44be9-128">Ja</span><span class="sxs-lookup"><span data-stu-id="44be9-128">Yes</span></span> |<span data-ttu-id="44be9-129">Operativsystemet på den virtuella datorn där artefakt är installerad.</span><span class="sxs-lookup"><span data-stu-id="44be9-129">Operating system of the VM where artifact is installed.</span></span> <span data-ttu-id="44be9-130">Alternativ som stöds är: Windows och Linux.</span><span class="sxs-lookup"><span data-stu-id="44be9-130">Supported options are: Windows and Linux.</span></span> |
| <span data-ttu-id="44be9-131">Parametrar</span><span class="sxs-lookup"><span data-stu-id="44be9-131">parameters</span></span> |<span data-ttu-id="44be9-132">Nej</span><span class="sxs-lookup"><span data-stu-id="44be9-132">No</span></span> |<span data-ttu-id="44be9-133">Värden som tillhandahålls när artefakt installationskommando körs på en dator.</span><span class="sxs-lookup"><span data-stu-id="44be9-133">Values that are provided when artifact install command is run on a machine.</span></span> <span data-ttu-id="44be9-134">På så sätt kan anpassa din artefakt.</span><span class="sxs-lookup"><span data-stu-id="44be9-134">This helps in customizing your artifact.</span></span> |
| <span data-ttu-id="44be9-135">Kör kommando</span><span class="sxs-lookup"><span data-stu-id="44be9-135">runCommand</span></span> |<span data-ttu-id="44be9-136">Ja</span><span class="sxs-lookup"><span data-stu-id="44be9-136">Yes</span></span> |<span data-ttu-id="44be9-137">Artefakt installationskommando som körs på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="44be9-137">Artifact install command that is executed on a VM.</span></span> |

### <a name="artifact-parameters"></a><span data-ttu-id="44be9-138">Artefakt parametrar</span><span class="sxs-lookup"><span data-stu-id="44be9-138">Artifact parameters</span></span>
<span data-ttu-id="44be9-139">I avsnittet parametrar i definitionsfilen anger du vilka värden som en användare kan ange när du installerar en artefakt.</span><span class="sxs-lookup"><span data-stu-id="44be9-139">In the parameters section of the definition file, you specify which values a user can input when installing an artifact.</span></span> <span data-ttu-id="44be9-140">Du kan referera till dessa värden i kommandot artefakt install.</span><span class="sxs-lookup"><span data-stu-id="44be9-140">You can refer to these values in the artifact install command.</span></span>

<span data-ttu-id="44be9-141">Du kan definiera parametrar med följande struktur:</span><span class="sxs-lookup"><span data-stu-id="44be9-141">You define parameters with the following structure:</span></span>

    "parameters": {
        "<parameterName>": {
          "type": "<type-of-parameter-value>",
          "displayName": "<display-name-of-parameter>",
          "description": "<description-of-parameter>"
        }
      }

| <span data-ttu-id="44be9-142">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="44be9-142">Element name</span></span> | <span data-ttu-id="44be9-143">Krävs?</span><span class="sxs-lookup"><span data-stu-id="44be9-143">Required?</span></span> | <span data-ttu-id="44be9-144">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="44be9-144">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="44be9-145">typ</span><span class="sxs-lookup"><span data-stu-id="44be9-145">type</span></span> |<span data-ttu-id="44be9-146">Ja</span><span class="sxs-lookup"><span data-stu-id="44be9-146">Yes</span></span> |<span data-ttu-id="44be9-147">Typ av parametervärdet.</span><span class="sxs-lookup"><span data-stu-id="44be9-147">Type of parameter value.</span></span> <span data-ttu-id="44be9-148">Se följande lista för tillåtna typer:</span><span class="sxs-lookup"><span data-stu-id="44be9-148">See the following list for the allowed types:</span></span> |
| <span data-ttu-id="44be9-149">Visningsnamn</span><span class="sxs-lookup"><span data-stu-id="44be9-149">displayName</span></span> |<span data-ttu-id="44be9-150">Ja</span><span class="sxs-lookup"><span data-stu-id="44be9-150">Yes</span></span> |<span data-ttu-id="44be9-151">Namnet på den parameter som visas för en användare i labbet.</span><span class="sxs-lookup"><span data-stu-id="44be9-151">Name of the parameter that is displayed to a user in the lab.</span></span> | |
| <span data-ttu-id="44be9-152">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="44be9-152">description</span></span> |<span data-ttu-id="44be9-153">Ja</span><span class="sxs-lookup"><span data-stu-id="44be9-153">Yes</span></span> |<span data-ttu-id="44be9-154">Beskrivning av den parameter som visas i labbet.</span><span class="sxs-lookup"><span data-stu-id="44be9-154">Description of the parameter that is displayed in the lab.</span></span> |

<span data-ttu-id="44be9-155">De tillåtna typerna är:</span><span class="sxs-lookup"><span data-stu-id="44be9-155">The allowed types are:</span></span>

* <span data-ttu-id="44be9-156">strängen – en giltig JSON-sträng</span><span class="sxs-lookup"><span data-stu-id="44be9-156">string – any valid JSON string</span></span>
* <span data-ttu-id="44be9-157">int – alla giltiga JSON-heltal</span><span class="sxs-lookup"><span data-stu-id="44be9-157">int – any valid JSON integer</span></span>
* <span data-ttu-id="44be9-158">bool – alla giltiga JSON-booleska</span><span class="sxs-lookup"><span data-stu-id="44be9-158">bool – any valid JSON Boolean</span></span>
* <span data-ttu-id="44be9-159">matrisen – en giltig JSON-matris</span><span class="sxs-lookup"><span data-stu-id="44be9-159">array – any valid JSON array</span></span>

## <a name="artifact-expressions-and-functions"></a><span data-ttu-id="44be9-160">Artefakt uttryck och funktioner</span><span class="sxs-lookup"><span data-stu-id="44be9-160">Artifact expressions and functions</span></span>
<span data-ttu-id="44be9-161">Du kan använda uttryck och funktioner för att konstruera artefakten installationskommando.</span><span class="sxs-lookup"><span data-stu-id="44be9-161">You can use expression and functions to construct the artifact install command.</span></span>
<span data-ttu-id="44be9-162">Uttryck är omges av hakparenteser ([och]) och utvärderas när artefakten installeras.</span><span class="sxs-lookup"><span data-stu-id="44be9-162">Expressions are enclosed with brackets ([ and ]), and are evaluated when the artifact is installed.</span></span> <span data-ttu-id="44be9-163">Uttryck kan finnas var som helst i ett strängvärde i JSON och returnerar alltid en annan JSON-värde.</span><span class="sxs-lookup"><span data-stu-id="44be9-163">Expressions can appear anywhere in a JSON string value and always return another JSON value.</span></span> <span data-ttu-id="44be9-164">Om du behöver använda en teckensträng som börjar med en hakparentes [, måste du använda två hakparenteser [[.</span><span class="sxs-lookup"><span data-stu-id="44be9-164">If you need to use a literal string that starts with a bracket [, you must use two brackets [[.</span></span>
<span data-ttu-id="44be9-165">Normalt använder du uttryck med funktioner för att konstruera ett värde.</span><span class="sxs-lookup"><span data-stu-id="44be9-165">Typically, you use expressions with functions to construct a value.</span></span> <span data-ttu-id="44be9-166">Precis som i JavaScript är-funktionsanrop som formaterade som functionName(arg1,arg2,arg3).</span><span class="sxs-lookup"><span data-stu-id="44be9-166">Just like in JavaScript, function calls are formatted as functionName(arg1,arg2,arg3).</span></span>

<span data-ttu-id="44be9-167">I följande lista visas vanliga funktioner:</span><span class="sxs-lookup"><span data-stu-id="44be9-167">The following list shows common functions:</span></span>

* <span data-ttu-id="44be9-168">parameters(parameterName) - returnerar ett parametervärde som tillhandahålls vid artefakt kommandot körs.</span><span class="sxs-lookup"><span data-stu-id="44be9-168">parameters(parameterName) - Returns a parameter value that is provided when the artifact command is run.</span></span>
* <span data-ttu-id="44be9-169">sammanfoga (arg1, arg2, arg3,...) - kombinerar flera strängvärden.</span><span class="sxs-lookup"><span data-stu-id="44be9-169">concat(arg1,arg2,arg3, …..) -     Combines multiple string values.</span></span> <span data-ttu-id="44be9-170">Den här funktionen kan ta valfritt antal argument.</span><span class="sxs-lookup"><span data-stu-id="44be9-170">This function can take any number of arguments.</span></span>

<span data-ttu-id="44be9-171">I följande exempel visas hur du använder uttryck och funktioner för att konstruera ett värde:</span><span class="sxs-lookup"><span data-stu-id="44be9-171">The following example shows how to use expression and functions to construct a value:</span></span>

    runCommand": {
         "commandToExecute": "[concat('powershell.exe -ExecutionPolicy bypass \"& ./startChocolatey.ps1'
    , ' -RawPackagesList ', parameters('packages')
    , ' -Username ', parameters('installUsername')
    , ' -Password ', parameters('installPassword'))]"
    }

## <a name="create-a-custom-artifact"></a><span data-ttu-id="44be9-172">Skapa en anpassad artefakt</span><span class="sxs-lookup"><span data-stu-id="44be9-172">Create a custom artifact</span></span>
<span data-ttu-id="44be9-173">Skapa din egen artefakt genom att följa dessa steg:</span><span class="sxs-lookup"><span data-stu-id="44be9-173">Create your custom artifact by following these steps:</span></span>

1. <span data-ttu-id="44be9-174">Installera en JSON-redigerare, behöver du en JSON-redigerare för att arbeta med artefakt definitionsfiler.</span><span class="sxs-lookup"><span data-stu-id="44be9-174">Install a JSON editor - You need a JSON editor to work with artifact definition files.</span></span> <span data-ttu-id="44be9-175">Vi rekommenderar att du använder [Visual Studio Code](https://code.visualstudio.com/), som är tillgänglig för Windows, Linux och OS X.</span><span class="sxs-lookup"><span data-stu-id="44be9-175">We recommend using [Visual Studio Code](https://code.visualstudio.com/), which is available for Windows, Linux and OS X.</span></span>
2. <span data-ttu-id="44be9-176">Hämta en exempel-artifactfile.json - utcheckning artefakter som skapats av Azure DevTest Labs-teamet på vår [GitHub-lagringsplatsen](https://github.com/Azure/azure-devtestlab), där vi har skapat ett omfattande Meddelandebibliotek artefakter som hjälper dig skapa dina egna artefakter.</span><span class="sxs-lookup"><span data-stu-id="44be9-176">Get a sample artifactfile.json - Check out the artifacts created by Azure DevTest Labs team at our [GitHub repository](https://github.com/Azure/azure-devtestlab), where we have created a rich library of artifacts that help you create your own artifacts.</span></span> <span data-ttu-id="44be9-177">Hämta en artefakt definitionsfilen och göra ändringar i den för att skapa egna artefakter.</span><span class="sxs-lookup"><span data-stu-id="44be9-177">Download an artifact definition file and make changes to it to create your own artifacts.</span></span>
3. <span data-ttu-id="44be9-178">Genom att utnyttja IntelliSense - utnyttjar IntelliSense Se giltiga element som kan användas för att konstruera en artefakt definitionsfilen.</span><span class="sxs-lookup"><span data-stu-id="44be9-178">Make use of IntelliSense - Leverage IntelliSense to see valid elements that can be used to construct an artifact definition file.</span></span> <span data-ttu-id="44be9-179">Du kan också se de olika alternativen för värden i ett element.</span><span class="sxs-lookup"><span data-stu-id="44be9-179">You can also see the different options for values of an element.</span></span> <span data-ttu-id="44be9-180">Till exempel IntelliSense visar du två val av Windows eller Linux när du redigerar den **targetOsType** element.</span><span class="sxs-lookup"><span data-stu-id="44be9-180">For example, IntelliSense show you the two choices of Windows or Linux when editing the **targetOsType** element.</span></span>
4. <span data-ttu-id="44be9-181">Lagra artefakt i en [git-lagringsplats](devtest-lab-add-artifact-repo.md).</span><span class="sxs-lookup"><span data-stu-id="44be9-181">Store the artifact in a [git repository](devtest-lab-add-artifact-repo.md).</span></span>
   
   1. <span data-ttu-id="44be9-182">Skapa en separat katalog för varje artefakt där katalognamnet är detsamma som namnet artefakt.</span><span class="sxs-lookup"><span data-stu-id="44be9-182">Create a separate directory for each artifact where the directory name is the same as the artifact name.</span></span>
   2. <span data-ttu-id="44be9-183">Lagra artefakt-definitionsfil (artifactfile.json) i den katalog som du skapade.</span><span class="sxs-lookup"><span data-stu-id="44be9-183">Store the artifact definition file (artifactfile.json) in the directory you created.</span></span>
   3. <span data-ttu-id="44be9-184">Lagra de skript som refereras till från installationskommando artefakt.</span><span class="sxs-lookup"><span data-stu-id="44be9-184">Store the scripts that are referenced from the artifact install command.</span></span>
      
      <span data-ttu-id="44be9-185">Här är ett exempel på hur en artefakt-mappen kan se ut:</span><span class="sxs-lookup"><span data-stu-id="44be9-185">Here is an example of how an artifact folder might look:</span></span>
      
      ![Exempel för artefakt git repo](./media/devtest-lab-artifact-author/git-repo.png)
5. <span data-ttu-id="44be9-187">Lägg till artefakter databasen till labbet - finns i artikel [lägga till en Git-lagringsplats för artefakter och mallar](devtest-lab-add-artifact-repo.md).</span><span class="sxs-lookup"><span data-stu-id="44be9-187">Add the artifacts repository to the lab - Refer to the article, [Add a Git repository for artifacts and templates](devtest-lab-add-artifact-repo.md).</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-articles"></a><span data-ttu-id="44be9-188">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="44be9-188">Related articles</span></span>
* [<span data-ttu-id="44be9-189">Felsökning av artefakt fel i DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="44be9-189">How to diagnose artifact failures in DevTest Labs</span></span>](devtest-lab-troubleshoot-artifact-failure.md)
* [<span data-ttu-id="44be9-190">Anslut en virtuell dator till befintliga AD-domän med hjälp av en resource manager-mall i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="44be9-190">Join a VM to existing AD Domain using a resource manager template in Azure DevTest Labs</span></span>](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

## <a name="next-steps"></a><span data-ttu-id="44be9-191">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="44be9-191">Next steps</span></span>
* <span data-ttu-id="44be9-192">Lär dig hur du [lägga till en Git-artefaktcentrallagret i ett labb](devtest-lab-add-artifact-repo.md).</span><span class="sxs-lookup"><span data-stu-id="44be9-192">Learn how to [add a Git artifact repository to a lab](devtest-lab-add-artifact-repo.md).</span></span>

