---
title: aaaAuthor anpassad R-moduler i Azure Machine Learning | Microsoft Docs
description: "Snabbstart för att skapa anpassade R-moduler i Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 6cbc628a-7e60-42ce-9f90-20aaea7ba630
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 03/24/2017
ms.author: bradsev;ankarlof
ms.openlocfilehash: 8007c2abe20a4ab990f38b6d09bc4e6834ad2082
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="author-custom-r-modules-in-azure-machine-learning"></a><span data-ttu-id="23534-103">Skapa anpassade R-moduler i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="23534-103">Author custom R modules in Azure Machine Learning</span></span>
<span data-ttu-id="23534-104">Det här avsnittet beskrivs hur tooauthor och distribuera en anpassad R-modul i Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="23534-104">This topic describes how tooauthor and deploy a custom R module in Azure Machine Learning.</span></span> <span data-ttu-id="23534-105">Det förklarar vilka anpassade R-moduler är och vilka filer som används toodefine dem.</span><span class="sxs-lookup"><span data-stu-id="23534-105">It explains what custom R modules are and what files are used toodefine them.</span></span> <span data-ttu-id="23534-106">Den illustrerar hur tooconstruct hello filer som definierar en modul och hur tooregister hello-modul för distribution i en Machine Learning-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="23534-106">It illustrates how tooconstruct hello files that define a module and how tooregister hello module for deployment in a Machine Learning workspace.</span></span> <span data-ttu-id="23534-107">hello beskrivs element och attribut som används i hello definition av hello anpassad modul sedan i detalj.</span><span class="sxs-lookup"><span data-stu-id="23534-107">hello elements and attributes used in hello definition of hello custom module are then described in more detail.</span></span> <span data-ttu-id="23534-108">Hur diskuteras också toouse extra funktioner och filer och flera utdata.</span><span class="sxs-lookup"><span data-stu-id="23534-108">How toouse auxiliary functionality and files and multiple outputs is also discussed.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="what-is-a-custom-r-module"></a><span data-ttu-id="23534-109">Vad är en anpassad R-modul?</span><span class="sxs-lookup"><span data-stu-id="23534-109">What is a custom R module?</span></span>
<span data-ttu-id="23534-110">En **anpassad modul** är en användardefinierad modul som kan vara upp tooyour arbetsytan och köras som en del av en Azure Machine Learning-experiment.</span><span class="sxs-lookup"><span data-stu-id="23534-110">A **custom module** is a user-defined module that can be uploaded tooyour workspace and executed as part of an Azure Machine Learning experiment.</span></span> <span data-ttu-id="23534-111">En **anpassad R-modul** är en anpassad modul som kör en användardefinierad R-funktion.</span><span class="sxs-lookup"><span data-stu-id="23534-111">A **custom R module** is a custom module that executes a user-defined R function.</span></span> <span data-ttu-id="23534-112">**R** är ett programmeringsspråk för statistisk databehandling och bilder som ofta används av forskare statistiker och data för att implementera algoritmer.</span><span class="sxs-lookup"><span data-stu-id="23534-112">**R** is a programming language for statistical computing and graphics that is widely used by statisticians and data scientists for implementing algorithms.</span></span> <span data-ttu-id="23534-113">R är för närvarande hello enda språk som stöds i anpassade moduler, men stöd för ytterligare språk är schemalagd för framtida versioner.</span><span class="sxs-lookup"><span data-stu-id="23534-113">Currently, R is hello only language supported in custom modules, but support for additional languages is scheduled for future releases.</span></span>

<span data-ttu-id="23534-114">Anpassade moduler har **förstklassigt status** i Azure Machine Learning i hello meningen att de kan användas som en annan modul.</span><span class="sxs-lookup"><span data-stu-id="23534-114">Custom modules have **first-class status** in Azure Machine Learning in hello sense that they can be used just like any other module.</span></span> <span data-ttu-id="23534-115">De kan köras med andra moduler som ingår i publicerade experiment eller visualiseringar.</span><span class="sxs-lookup"><span data-stu-id="23534-115">They can be executed with other modules, included in published experiments or in visualizations.</span></span> <span data-ttu-id="23534-116">Du har kontroll över hello-algoritmen som implementeras av hello modul, hello inkommande och utgående portarna toobe används, hello modellering parametrar och andra olika runtime-funktioner.</span><span class="sxs-lookup"><span data-stu-id="23534-116">You have control over hello algorithm implemented by hello module, hello input and output ports toobe used, hello modeling parameters, and other various runtime behaviors.</span></span> <span data-ttu-id="23534-117">Ett experiment som innehåller anpassade moduler kan också publiceras till hello Cortana Intelligence Gallery lättare att dela.</span><span class="sxs-lookup"><span data-stu-id="23534-117">An experiment that contains custom modules can also be published into hello Cortana Intelligence Gallery for easy sharing.</span></span>

## <a name="files-in-a-custom-r-module"></a><span data-ttu-id="23534-118">Filer i en anpassad R-modul</span><span class="sxs-lookup"><span data-stu-id="23534-118">Files in a custom R module</span></span>
<span data-ttu-id="23534-119">En anpassad R-modul har definierats som en .zip-fil som innehåller minst två filer:</span><span class="sxs-lookup"><span data-stu-id="23534-119">A custom R module is defined by a .zip file that contains, at a minimum, two files:</span></span>

* <span data-ttu-id="23534-120">En **källfilen** som implementerar hello R-funktionen som exponeras av hello-modul</span><span class="sxs-lookup"><span data-stu-id="23534-120">A **source file** that implements hello R function exposed by hello module</span></span>
* <span data-ttu-id="23534-121">En **XML-definitionsfilen** som beskriver hello anpassad modul gränssnitt</span><span class="sxs-lookup"><span data-stu-id="23534-121">An **XML definition file** that describes hello custom module interface</span></span>

<span data-ttu-id="23534-122">Ytterligare extra filer kan också tas med i hello ZIP-fil som innehåller funktioner som kan nås från hello anpassad modul.</span><span class="sxs-lookup"><span data-stu-id="23534-122">Additional auxiliary files can also be included in hello .zip file that provides functionality that can be accessed from hello custom module.</span></span> <span data-ttu-id="23534-123">Det här alternativet diskuteras i hello **argument** tillhör hello referensavsnitt **element i XML-definitionsfilen för hello** följande hello quickstart exempel.</span><span class="sxs-lookup"><span data-stu-id="23534-123">This option is discussed in hello **Arguments** part of hello reference section **Elements in hello XML definition file** following hello quickstart example.</span></span>

## <a name="quickstart-example-define-package-and-register-a-custom-r-module"></a><span data-ttu-id="23534-124">Snabbstart exempel: definiera, paketera och registrera en anpassad R-modul</span><span class="sxs-lookup"><span data-stu-id="23534-124">Quickstart example: define, package, and register a custom R module</span></span>
<span data-ttu-id="23534-125">Det här exemplet illustrerar hur tooconstruct hello filer som krävs av en anpassad R-modul, paketera dem till en zip-filen och sedan registrera hello-modul i Machine Learning-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="23534-125">This example illustrates how tooconstruct hello files required by a custom R module, package them into a zip file, and then register hello module in your Machine Learning workspace.</span></span> <span data-ttu-id="23534-126">hello exempel zip-paketet och exempel filer kan hämtas från [hämta CustomAddRows.zip filen](http://go.microsoft.com/fwlink/?LinkID=524916&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="23534-126">hello example zip package and sample files can be downloaded from [Download CustomAddRows.zip file](http://go.microsoft.com/fwlink/?LinkID=524916&clcid=0x409).</span></span>

## <a name="hello-source-file"></a><span data-ttu-id="23534-127">hello källfilen</span><span class="sxs-lookup"><span data-stu-id="23534-127">hello source file</span></span>
<span data-ttu-id="23534-128">Överväg hello exempel på en **anpassad Lägg till rader** modul som ändrar hello standardimplementering av hello **lägga till rader** modulen används tooconcatenate rader (observationer) från två datamängder (dataramar).</span><span class="sxs-lookup"><span data-stu-id="23534-128">Consider hello example of a **Custom Add Rows** module that modifies hello standard implementation of hello **Add Rows** module used tooconcatenate rows (observations) from two datasets (data frames).</span></span> <span data-ttu-id="23534-129">hello standard **lägga till rader** modulen lägger till hello rader hello andra inkommande dataset toohello End hello första inkommande datauppsättningen med hello `rbind` algoritmen.</span><span class="sxs-lookup"><span data-stu-id="23534-129">hello standard **Add Rows** module appends hello rows of hello second input dataset toohello end of hello first input dataset using hello `rbind` algorithm.</span></span> <span data-ttu-id="23534-130">hello anpassade `CustomAddRows` funktionen accepterar två datauppsättningar på samma sätt, men även accepterar en parameter med booleska växlingsutrymme som en ytterligare indata.</span><span class="sxs-lookup"><span data-stu-id="23534-130">hello customized `CustomAddRows` function similarly accepts two datasets, but also accepts a Boolean swap parameter as an additional input.</span></span> <span data-ttu-id="23534-131">Om hello växlingen parameter har angetts för**FALSKT**, den returnerar hello samma datamängd som hello standardimplementeringen.</span><span class="sxs-lookup"><span data-stu-id="23534-131">If hello swap parameter is set too**FALSE**, it returns hello same data set as hello standard implementation.</span></span> <span data-ttu-id="23534-132">Men om hello växlingen parametern **SANT**, hello funktionen lägger till rader i första inkommande dataset toohello slutet av hello andra dataset i stället.</span><span class="sxs-lookup"><span data-stu-id="23534-132">But if hello swap parameter is **TRUE**, hello function appends rows of first input dataset toohello end of hello second dataset instead.</span></span> <span data-ttu-id="23534-133">Hej CustomAddRows.R-fil som innehåller hello implementering av hello R `CustomAddRows` funktion som exponeras av hello **anpassad Lägg till rader** modulen har hello följande R-koden.</span><span class="sxs-lookup"><span data-stu-id="23534-133">hello CustomAddRows.R file that contains hello implementation of hello R `CustomAddRows` function exposed by hello **Custom Add Rows** module has hello following R code.</span></span>

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) 
    {
        if (swap)
        {
            return (rbind(dataset2, dataset1));
        }
        else
        {
            return (rbind(dataset1, dataset2));
        } 
    } 

### <a name="hello-xml-definition-file"></a><span data-ttu-id="23534-134">hello XML-definitionsfilen</span><span class="sxs-lookup"><span data-stu-id="23534-134">hello XML definition file</span></span>
<span data-ttu-id="23534-135">tooexpose detta `CustomAddRows` fungera som en XML-definitionsfilen en Azure Machine Learning-modul måste skapas toospecify hur hello **anpassad Lägg till rader** modulen ska ser ut och fungerar.</span><span class="sxs-lookup"><span data-stu-id="23534-135">tooexpose this `CustomAddRows` function as an Azure Machine Learning module, an XML definition file must be created toospecify how hello **Custom Add Rows** module should look and behave.</span></span> 

    <!-- Defined a module using an R Script -->
    <Module name="Custom Add Rows">
        <Owner>Microsoft Corporation</Owner>
        <Description>Appends one dataset tooanother. Dataset 2 is concatenated tooDataset 1 when Swap is FALSE, and vice versa when Swap is TRUE.</Description>

    <!-- Specify hello base language, script file and R function toouse for this module. -->        
        <Language name="R" 
         sourceFile="CustomAddRows.R" 
         entryPoint="CustomAddRows" />  

    <!-- Define module input and output ports -->
    <!-- Note: hello values of hello id attributes in hello Input and Arg elements must match hello parameter names in hello R Function CustomAddRows defined in CustomAddRows.R. -->
        <Ports>
            <Input id="dataset1" name="Dataset 1" type="DataTable">
                <Description>First input dataset</Description>
            </Input>
            <Input id="dataset2" name="Dataset 2" type="DataTable">
                <Description>Second input dataset</Description>
            </Input>
            <Output id="dataset" name="Dataset" type="DataTable">
                <Description>hello combined dataset</Description>
            </Output>
        </Ports>

    <!-- Define module parameters -->
        <Arguments>
            <Arg id="swap" name="Swap" type="bool" >
                <Description>Swap input datasets.</Description>
            </Arg>
        </Arguments>
    </Module>


<span data-ttu-id="23534-136">Det är kritiska toonote som hello värdet för hello **id** attribut för hello **indata** och **%d{arg/** element i XML-filen för hello måste matcha hello funktionen parameternamn för hello R kod i hello CustomAddRows.R filen exakt: (*dataset1*, *dataset2*, och *växlingen* i hello exemplet).</span><span class="sxs-lookup"><span data-stu-id="23534-136">It is critical toonote that hello value of hello **id** attributes of hello **Input** and **Arg** elements in hello XML file must match hello function parameter names of hello R code in hello CustomAddRows.R file EXACTLY: (*dataset1*, *dataset2*, and *swap* in hello example).</span></span> <span data-ttu-id="23534-137">På liknande sätt hello värdet för hello **entryPoint** attribut för hello **språk** elementet måste matcha hello namnet på hello funktion i hello R-skriptet exakt: (*CustomAddRows* i hello exemplet).</span><span class="sxs-lookup"><span data-stu-id="23534-137">Similarly, hello value of hello **entryPoint** attribute of hello **Language** element must match hello name of hello function in hello R script EXACTLY: (*CustomAddRows* in hello example).</span></span> 

<span data-ttu-id="23534-138">Däremot hello **id** attribut för hello **utdata** element överensstämmer inte tooany variabler i hello R-skriptet.</span><span class="sxs-lookup"><span data-stu-id="23534-138">In contrast, hello **id** attribute for hello **Output** element does not correspond tooany variables in hello R script.</span></span> <span data-ttu-id="23534-139">När flera utdata krävs bara returnera en lista från hello R-funktionen med resultat som placeras *i hello samma ordning* som **utdata** element deklareras i hello XML-fil.</span><span class="sxs-lookup"><span data-stu-id="23534-139">When more than one output is required, simply return a list from hello R function with results placed *in hello same order* as **Outputs** elements are declared in hello XML file.</span></span>

### <a name="package-and-register-hello-module"></a><span data-ttu-id="23534-140">Paket- och registrera hello-modul</span><span class="sxs-lookup"><span data-stu-id="23534-140">Package and register hello module</span></span>
<span data-ttu-id="23534-141">Spara dessa två filer som *CustomAddRows.R* och *CustomAddRows.xml* och sedan zip hello två filer tillsammans i en *CustomAddRows.zip* fil.</span><span class="sxs-lookup"><span data-stu-id="23534-141">Save these two files as *CustomAddRows.R* and *CustomAddRows.xml* and then zip hello two files together into a *CustomAddRows.zip* file.</span></span>

<span data-ttu-id="23534-142">tooregister dem i Machine Learning-arbetsytan, gå tooyour arbetsytan i hello Machine Learning Studio klickar du på hello **+ ny** knappen hello längst ned och välj **modul -> från ZIP-PAKETET** tooupload hello nya **anpassad Lägg till rader** modul.</span><span class="sxs-lookup"><span data-stu-id="23534-142">tooregister them in your Machine Learning workspace, go tooyour workspace in hello Machine Learning Studio, click hello **+NEW** button on hello bottom and choose **MODULE -> FROM ZIP PACKAGE** tooupload hello new **Custom Add Rows** module.</span></span>

![Ladda upp Zip](./media/machine-learning-custom-r-modules/upload-from-zip-package.png)

<span data-ttu-id="23534-144">Hej **anpassad Lägg till rader** modulen är nu redo toobe nås av Machine Learning-experiment.</span><span class="sxs-lookup"><span data-stu-id="23534-144">hello **Custom Add Rows** module is now ready toobe accessed by your Machine Learning experiments.</span></span>

## <a name="elements-in-hello-xml-definition-file"></a><span data-ttu-id="23534-145">Element i XML-definitionsfilen för hello</span><span class="sxs-lookup"><span data-stu-id="23534-145">Elements in hello XML definition file</span></span>
### <a name="module-elements"></a><span data-ttu-id="23534-146">Modulen element</span><span class="sxs-lookup"><span data-stu-id="23534-146">Module elements</span></span>
<span data-ttu-id="23534-147">Hej **modulen** elementet har använt toodefine en anpassad modul i hello XML-fil.</span><span class="sxs-lookup"><span data-stu-id="23534-147">hello **Module** element is used toodefine a custom module in hello XML file.</span></span> <span data-ttu-id="23534-148">Flera moduler kan definieras i en XML-fil med flera **modulen** element.</span><span class="sxs-lookup"><span data-stu-id="23534-148">Multiple modules can be defined in one XML file using multiple **module** elements.</span></span> <span data-ttu-id="23534-149">Varje modul på arbetsytan måste ha ett unikt namn.</span><span class="sxs-lookup"><span data-stu-id="23534-149">Each module in your workspace must have a unique name.</span></span> <span data-ttu-id="23534-150">Registrera en anpassad modul med samma namn som en befintlig anpassad modul hello och den ersätter hello befintlig modul med hello ny.</span><span class="sxs-lookup"><span data-stu-id="23534-150">Register a custom module with hello same name as an existing custom module and it replaces hello existing module with hello new one.</span></span> <span data-ttu-id="23534-151">Anpassade moduler kan dock vara registrerade med samma namn som en befintlig Azure Machine Learning-modul hello.</span><span class="sxs-lookup"><span data-stu-id="23534-151">Custom modules can, however, be registered with hello same name as an existing Azure Machine Learning module.</span></span> <span data-ttu-id="23534-152">Om så är fallet bör de visas i hello **anpassad** hello modulpaletten kategori.</span><span class="sxs-lookup"><span data-stu-id="23534-152">If so, they appear in hello **Custom** category of hello module palette.</span></span>

    <Module name="Custom Add Rows" isDeterministic="false"> 
        <Owner>Microsoft Corporation</Owner>
        <Description>Appends one dataset tooanother...</Description>/> 


<span data-ttu-id="23534-153">Inom hello **modulen** element, du kan ange två ytterligare valfritt element:</span><span class="sxs-lookup"><span data-stu-id="23534-153">Within hello **Module** element, you can specify two additional optional elements:</span></span>

* <span data-ttu-id="23534-154">en **ägare** element som är inbäddad i hello-modul</span><span class="sxs-lookup"><span data-stu-id="23534-154">an **Owner** element that is embedded into hello module</span></span>  
* <span data-ttu-id="23534-155">en **beskrivning** element som innehåller text som visas i snabbt hjälp för hello modulen och när du hovrar över hello-modul i hello Machine Learning-Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="23534-155">a **Description** element that contains text that is displayed in quick help for hello module and when you hover over hello module in hello Machine Learning UI.</span></span>

<span data-ttu-id="23534-156">Regler för tecken-gränser i hello modulen element:</span><span class="sxs-lookup"><span data-stu-id="23534-156">Rules for characters limits in hello Module elements:</span></span>

* <span data-ttu-id="23534-157">Hej värdet för hello **namn** attribut i hello **modulen** element får inte överstiga 64 tecken.</span><span class="sxs-lookup"><span data-stu-id="23534-157">hello value of hello **name** attribute in hello **Module** element must not exceed 64 characters in length.</span></span> 
* <span data-ttu-id="23534-158">Hej innehållet i hello **beskrivning** element får inte överstiga 128 tecken.</span><span class="sxs-lookup"><span data-stu-id="23534-158">hello content of hello **Description** element must not exceed 128 characters in length.</span></span>
* <span data-ttu-id="23534-159">Hej innehållet i hello **ägare** element får inte överstiga 32 tecken.</span><span class="sxs-lookup"><span data-stu-id="23534-159">hello content of hello **Owner** element must not exceed 32 characters in length.</span></span>

<span data-ttu-id="23534-160">En modul resultaten kan vara deterministisk eller nondeterministic.* * som standard, alla moduler anses toobe deterministisk.</span><span class="sxs-lookup"><span data-stu-id="23534-160">A module's results can be deterministic or nondeterministic.** By default, all modules are considered toobe deterministic.</span></span> <span data-ttu-id="23534-161">Det vill säga ska med en oföränderlig uppsättning indataparametrar och data hello modulen returnera hello samma resultat eacRAND eller en functionh körningen.</span><span class="sxs-lookup"><span data-stu-id="23534-161">That is, given an unchanging set of input parameters and data, hello module should return hello same results eacRAND or a functionh time it is run.</span></span> <span data-ttu-id="23534-162">Det här beteendet, Azure Machine Learning Studio Kör endast moduler som har markerats som deterministisk om en parameter eller hello indata har ändrats.</span><span class="sxs-lookup"><span data-stu-id="23534-162">Given this behavior, Azure Machine Learning Studio only reruns modules marked as deterministic if a parameter or hello input data has changed.</span></span> <span data-ttu-id="23534-163">Returnerar hello cachelagrade resultaten ger också mycket snabbare körning av experiment.</span><span class="sxs-lookup"><span data-stu-id="23534-163">Returning hello cached results also provides much faster execution of experiments.</span></span>

<span data-ttu-id="23534-164">Det finns funktioner som är icke-deterministisk, till exempel SLUMP eller en funktion som returnerar hello aktuellt datum och tid.</span><span class="sxs-lookup"><span data-stu-id="23534-164">There are functions that are nondeterministic, such as RAND or a function that returns hello current date or time.</span></span> <span data-ttu-id="23534-165">Om din modulen använder en icke-deterministisk funktion, kan du ange att hello-modulen är icke-deterministiska genom att ange hello valfria **isDeterministic** attribut för**FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="23534-165">If your module uses a nondeterministic function, you can specify that hello module is non-deterministic by setting hello optional **isDeterministic** attribute too**FALSE**.</span></span> <span data-ttu-id="23534-166">Detta garanterar att hello-modulen körs när hello experiment körs, även om hello modulindata och parametrar inte har ändrats.</span><span class="sxs-lookup"><span data-stu-id="23534-166">This insures that hello module is rerun whenever hello experiment is run, even if hello module input and parameters have not changed.</span></span> 

### <a name="language-definition"></a><span data-ttu-id="23534-167">Definition av språk</span><span class="sxs-lookup"><span data-stu-id="23534-167">Language Definition</span></span>
<span data-ttu-id="23534-168">Hej **språk** -elementet i XML-definitionsfilen är används toospecify hello anpassad modul språk.</span><span class="sxs-lookup"><span data-stu-id="23534-168">hello **Language** element in your XML definition file is used toospecify hello custom module language.</span></span> <span data-ttu-id="23534-169">R är för närvarande hello stöds bara språk.</span><span class="sxs-lookup"><span data-stu-id="23534-169">Currently, R is hello only supported language.</span></span> <span data-ttu-id="23534-170">Hej värdet för hello **sourceFile** attributet måste vara hello namn på hello R-fil som innehåller hello funktionen toocall när hello-modulen körs.</span><span class="sxs-lookup"><span data-stu-id="23534-170">hello value of hello **sourceFile** attribute must be hello name of hello R file that contains hello function toocall when hello module is run.</span></span> <span data-ttu-id="23534-171">Den här filen måste vara en del av hello zip-paketet.</span><span class="sxs-lookup"><span data-stu-id="23534-171">This file must be part of hello zip package.</span></span> <span data-ttu-id="23534-172">Hej värdet för hello **entryPoint** attributet är hello namnet på hello funktion som anropas och måste matcha en giltig funktion som definierats med i hello källfil.</span><span class="sxs-lookup"><span data-stu-id="23534-172">hello value of hello **entryPoint** attribute is hello name of hello function being called and must match a valid function defined with in hello source file.</span></span>

    <Language name="R" sourceFile="CustomAddRows.R" entryPoint="CustomAddRows" />


### <a name="ports"></a><span data-ttu-id="23534-173">Portar</span><span class="sxs-lookup"><span data-stu-id="23534-173">Ports</span></span>
<span data-ttu-id="23534-174">hello inkommande och utgående portar för en anpassad modul har angetts i underordnade element för hello **portar** avsnitt i hello XML-definitionsfilen.</span><span class="sxs-lookup"><span data-stu-id="23534-174">hello input and output ports for a custom module are specified in child elements of hello **Ports** section of hello XML definition file.</span></span> <span data-ttu-id="23534-175">hello ordningen på elementen anger hello layout erfarna (UX) av användare.</span><span class="sxs-lookup"><span data-stu-id="23534-175">hello order of these elements determines hello layout experienced (UX) by users.</span></span> <span data-ttu-id="23534-176">hello första underordnade **inkommande** eller **utdata** som anges i hello **portar** element i XML-filen för hello blir hello vänstra indataporten i hello Machine Learning UX.</span><span class="sxs-lookup"><span data-stu-id="23534-176">hello first child **input** or **output** listed in hello **Ports** element of hello XML file becomes hello left-most input port in hello Machine Learning UX.</span></span>
<span data-ttu-id="23534-177">Varje indata och utdataport kan ha en valfri **beskrivning** underordnat element som anger hello text som visas när du hovrar hello markören över hello port i hello Machine Learning-Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="23534-177">Each input and output port may have an optional **Description** child element that specifies hello text shown when you hover hello mouse cursor over hello port in hello Machine Learning UI.</span></span>

<span data-ttu-id="23534-178">**Portarna regler**:</span><span class="sxs-lookup"><span data-stu-id="23534-178">**Ports Rules**:</span></span>

* <span data-ttu-id="23534-179">Maximalt antal **inkommande och utgående portarna** är 8 för varje.</span><span class="sxs-lookup"><span data-stu-id="23534-179">Maximum number of **input and output ports** is 8 for each.</span></span>

### <a name="input-elements"></a><span data-ttu-id="23534-180">Inkommande element</span><span class="sxs-lookup"><span data-stu-id="23534-180">Input elements</span></span>
<span data-ttu-id="23534-181">Portar kan du arbetsytan och toopass data tooyour R-funktionen.</span><span class="sxs-lookup"><span data-stu-id="23534-181">Input ports allow you toopass data tooyour R function and workspace.</span></span> <span data-ttu-id="23534-182">Hej **datatyper** som stöds för inkommande portar är följande:</span><span class="sxs-lookup"><span data-stu-id="23534-182">hello **data types** that are supported for input ports are as follows:</span></span> 

<span data-ttu-id="23534-183">**DataTable:** tooyour R funktionen skickas i den här typen som en data.frame.</span><span class="sxs-lookup"><span data-stu-id="23534-183">**DataTable:** This type is passed tooyour R function as a data.frame.</span></span> <span data-ttu-id="23534-184">I själva verket några typer (till exempel CSV-filer eller ARFF-filer) som stöds av Machine Learning och som är kompatibla med **DataTable** är konverterade tooa data.frame automatiskt.</span><span class="sxs-lookup"><span data-stu-id="23534-184">In fact, any types (for example, CSV files or ARFF files) that are supported by Machine Learning and that are compatible with **DataTable** are converted tooa data.frame automatically.</span></span> 

        <Input id="dataset1" name="Input 1" type="DataTable" isOptional="false">
            <Description>Input Dataset 1</Description>
           </Input>

<span data-ttu-id="23534-185">Hej **id** attribut som är associerade med varje **DataTable** indataport måste ha ett unikt värde och värdet måste matcha motsvarande namngiven parameter i R-funktionen.</span><span class="sxs-lookup"><span data-stu-id="23534-185">hello **id** attribute associated with each **DataTable** input port must have a unique value and this value must match its corresponding named parameter in your R function.</span></span>
<span data-ttu-id="23534-186">Valfria **DataTable** portar som inte angavs som indata i ett experiment har hello värdet **NULL** skickade toohello R-funktionen och valfria zip portar ignoreras om hello-indata inte är anslutet.</span><span class="sxs-lookup"><span data-stu-id="23534-186">Optional **DataTable** ports that are not passed as input in an experiment have hello value **NULL** passed toohello R function and optional zip ports are ignored if hello input is not connected.</span></span> <span data-ttu-id="23534-187">Hej **isOptional** attributet är valfri för båda hello **DataTable** och **Zip** datatyper och är *FALSKT* som standard.</span><span class="sxs-lookup"><span data-stu-id="23534-187">hello **isOptional** attribute is optional for both hello **DataTable** and **Zip** types and is *false* by default.</span></span>

<span data-ttu-id="23534-188">**ZIP:** anpassade moduler kan acceptera en zip-fil som indata.</span><span class="sxs-lookup"><span data-stu-id="23534-188">**Zip:** Custom modules can accept a zip file as input.</span></span> <span data-ttu-id="23534-189">Den här indata packat till hello R arbetskatalog för din funktion</span><span class="sxs-lookup"><span data-stu-id="23534-189">This input is unpacked into hello R working directory of your function</span></span>

        <Input id="zippedData" name="Zip Input" type="Zip" IsOptional="false">
            <Description>Zip files toobe extracted toohello R working directory.</Description>
           </Input>

<span data-ttu-id="23534-190">För anpassade R-moduler saknar hello-id för en Zip-port toomatch eventuella parametrar för hello R-funktionen.</span><span class="sxs-lookup"><span data-stu-id="23534-190">For custom R modules, hello id for a Zip port does not have toomatch any parameters of hello R function.</span></span> <span data-ttu-id="23534-191">Det beror på att hello zip-filen är automatiskt extraherade toohello R arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="23534-191">This is because hello zip file is automatically extracted toohello R working directory.</span></span>

<span data-ttu-id="23534-192">**Regler för inkommande:**</span><span class="sxs-lookup"><span data-stu-id="23534-192">**Input Rules:**</span></span>

* <span data-ttu-id="23534-193">Hej värdet för hello **id** attribut för hello **indata** elementet måste vara ett giltigt R variabelnamn.</span><span class="sxs-lookup"><span data-stu-id="23534-193">hello value of hello **id** attribute of hello **Input** element must be a valid R variable name.</span></span>
* <span data-ttu-id="23534-194">Hej värdet för hello **id** attribut för hello **indata** element får inte vara längre än 64 tecken.</span><span class="sxs-lookup"><span data-stu-id="23534-194">hello value of hello **id** attribute of hello **Input** element must not be longer than 64 characters.</span></span>
* <span data-ttu-id="23534-195">Hej värdet för hello **namn** attribut för hello **indata** element får inte vara längre än 64 tecken.</span><span class="sxs-lookup"><span data-stu-id="23534-195">hello value of hello **name** attribute of hello **Input** element must not be longer than 64 characters.</span></span>
* <span data-ttu-id="23534-196">Hej innehållet i hello **beskrivning** element får inte vara längre än 128 tecken</span><span class="sxs-lookup"><span data-stu-id="23534-196">hello content of hello **Description** element must not be longer than 128 characters</span></span>
* <span data-ttu-id="23534-197">Hej värdet för hello **typen** attribut för hello **indata** elementet måste vara *Zip* eller *DataTable*.</span><span class="sxs-lookup"><span data-stu-id="23534-197">hello value of hello **type** attribute of hello **Input** element must be *Zip* or *DataTable*.</span></span>
* <span data-ttu-id="23534-198">hello värdet för hello **isOptional** attribut för hello **indata** elementet krävs inte (och är *FALSKT* som standard om inget värde anges), men om detta anges måste det vara *SANT* eller *FALSKT*.</span><span class="sxs-lookup"><span data-stu-id="23534-198">hello value of hello **isOptional** attribute of hello **Input** element is not required (and is *false* by default when not specified); but if it is specified, it must be *true* or *false*.</span></span>

### <a name="output-elements"></a><span data-ttu-id="23534-199">Utdata element</span><span class="sxs-lookup"><span data-stu-id="23534-199">Output elements</span></span>
<span data-ttu-id="23534-200">**Standard utgående portar:** portar är mappade toohello returvärden från din R-funktionen, som sedan kan användas av efterföljande moduler.</span><span class="sxs-lookup"><span data-stu-id="23534-200">**Standard output ports:** Output ports are mapped toohello return values from your R function, which can then be used by subsequent modules.</span></span> <span data-ttu-id="23534-201">*DataTable* är hello endast standardutdata porttyp som stöds för närvarande.</span><span class="sxs-lookup"><span data-stu-id="23534-201">*DataTable* is hello only standard output port type supported currently.</span></span> <span data-ttu-id="23534-202">(Stöd för *inlärning* och *transformerar* är kommande.) En *DataTable* utdata har definierats som:</span><span class="sxs-lookup"><span data-stu-id="23534-202">(Support for *Learners* and *Transforms* is forthcoming.) A *DataTable* output is defined as:</span></span>

    <Output id="dataset" name="Dataset" type="DataTable">
        <Description>Combined dataset</Description>
    </Output>

<span data-ttu-id="23534-203">Utdata i anpassade R-moduler hello värdet för hello **id** inte har toocorrespond med något i hello R-skriptet, men det måste vara unikt.</span><span class="sxs-lookup"><span data-stu-id="23534-203">For outputs in custom R modules, hello value of hello **id** attribute does not have toocorrespond with anything in hello R script, but it must be unique.</span></span> <span data-ttu-id="23534-204">För en enskild modulen utdata hello returvärde från hello R-funktionen måste vara en *data.frame*.</span><span class="sxs-lookup"><span data-stu-id="23534-204">For a single module output, hello return value from hello R function must be a *data.frame*.</span></span> <span data-ttu-id="23534-205">I ordning toooutput fler än ett objekt av-datatypen stöds, hello lämpliga portar måste toobe som angetts i XML-definitionsfilen för hello och hello-objekten måste toobe returneras som en lista.</span><span class="sxs-lookup"><span data-stu-id="23534-205">In order toooutput more than one object of a supported data type, hello appropriate output ports need toobe specified in hello XML definition file and hello objects need toobe returned as a list.</span></span> <span data-ttu-id="23534-206">hello utdata objekt tilldelas toooutput portar från vänster tooright, reflektion hello ordning där hello objekten placeras i hello returnerade lista.</span><span class="sxs-lookup"><span data-stu-id="23534-206">hello output objects are assigned toooutput ports from left tooright, reflecting hello order in which hello objects are placed in hello returned list.</span></span>

<span data-ttu-id="23534-207">Om du vill toomodify hello exempelvis **anpassad Lägg till rader** modulen toooutput hello ursprungliga två datamängder *dataset1* och *dataset2*, dessutom toohello nya ansluten DataSet, *dataset*, (i en ordning, från vänster tooright som: *dataset*, *dataset1*, *dataset2*), definiera hello utgående portar i hello CustomAddRows.xml fil på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="23534-207">For example, if you want toomodify hello **Custom Add Rows** module toooutput hello original two datasets, *dataset1* and *dataset2*, in addition toohello new joined dataset, *dataset*, (in an order, from left tooright, as: *dataset*, *dataset1*, *dataset2*), then define hello output ports in hello CustomAddRows.xml file as follows:</span></span>

    <Ports> 
        <Output id="dataset" name="Dataset Out" type="DataTable"> 
            <Description>New Dataset</Description> 
        </Output> 
        <Output id="dataset1_out" name="Dataset 1 Out" type="DataTable"> 
            <Description>First Dataset</Description> 
        </Output> 
        <Output id="dataset2_out" name="Dataset 2 Out" type="DataTable"> 
            <Description>Second Dataset</Description> 
        </Output> 
        <Input id="dataset1" name="Dataset 1" type="DataTable"> 
            <Description>First Input Table</Description>
        </Input> 
        <Input id="dataset2" name="Dataset 2" type="DataTable"> 
            <Description>Second Input Table</Description> 
        </Input> 
    </Ports> 


<span data-ttu-id="23534-208">Och returnerar hello lista över objekt i en lista i hello i 'CustomAddRows.R' i rätt ordning:</span><span class="sxs-lookup"><span data-stu-id="23534-208">And return hello list of objects in a list in hello correct order in ‘CustomAddRows.R’:</span></span>

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) { 
        if (swap) { dataset <- rbind(dataset2, dataset1)) } 
        else { dataset <- rbind(dataset1, dataset2)) 
        } 
    return (list(dataset, dataset1, dataset2)) 
    } 

<span data-ttu-id="23534-209">**Visualiseringen utdata:** du kan också ange en utdataporten av typen *visualiseringen*, som visar hello utdata från hello R grafik enheten och konsolen utdata.</span><span class="sxs-lookup"><span data-stu-id="23534-209">**Visualization output:** You can also specify an output port of type *Visualization*, which displays hello output from hello R graphics device and console output.</span></span> <span data-ttu-id="23534-210">Den här porten är inte en del av hello R funktionen utdata och inte störa hello ordning hello andra utgående porttyper.</span><span class="sxs-lookup"><span data-stu-id="23534-210">This port is not part of hello R function output and does not interfere with hello order of hello other output port types.</span></span> <span data-ttu-id="23534-211">tooadd en visualiseringen port toohello anpassade moduler, lägga till en **utdata** element med värdet *visualiseringen* för dess **typen** attribut:</span><span class="sxs-lookup"><span data-stu-id="23534-211">tooadd a visualization port toohello custom modules, add an **Output** element with a value of *Visualization* for its **type** attribute:</span></span>

    <Output id="deviceOutput" name="View Port" type="Visualization">
      <Description>View hello R console graphics device output.</Description>
    </Output>

<span data-ttu-id="23534-212">**Utgående regler:**</span><span class="sxs-lookup"><span data-stu-id="23534-212">**Output Rules:**</span></span>

* <span data-ttu-id="23534-213">Hej värdet för hello **id** attribut för hello **utdata** elementet måste vara ett giltigt R variabelnamn.</span><span class="sxs-lookup"><span data-stu-id="23534-213">hello value of hello **id** attribute of hello **Output** element must be a valid R variable name.</span></span>
* <span data-ttu-id="23534-214">Hej värdet för hello **id** attribut för hello **utdata** element får inte vara längre än 32 tecken.</span><span class="sxs-lookup"><span data-stu-id="23534-214">hello value of hello **id** attribute of hello **Output** element must not be longer than 32 characters.</span></span>
* <span data-ttu-id="23534-215">Hej värdet för hello **namn** attribut för hello **utdata** element får inte vara längre än 64 tecken.</span><span class="sxs-lookup"><span data-stu-id="23534-215">hello value of hello **name** attribute of hello **Output** element must not be longer than 64 characters.</span></span>
* <span data-ttu-id="23534-216">Hej värdet för hello **typen** attribut för hello **utdata** elementet måste vara *visualiseringen*.</span><span class="sxs-lookup"><span data-stu-id="23534-216">hello value of hello **type** attribute of hello **Output** element must be *Visualization*.</span></span>

### <a name="arguments"></a><span data-ttu-id="23534-217">Argument</span><span class="sxs-lookup"><span data-stu-id="23534-217">Arguments</span></span>
<span data-ttu-id="23534-218">Ytterligare information kan skickas toohello R-funktionen via modulparametrar som definieras i hello **argument** element.</span><span class="sxs-lookup"><span data-stu-id="23534-218">Additional data can be passed toohello R function via module parameters which are defined in hello **Arguments** element.</span></span> <span data-ttu-id="23534-219">Dessa parametrar visas i hello längst till höger egenskapsrutan för hello Machine Learning Användargränssnittet när hello modul väljs.</span><span class="sxs-lookup"><span data-stu-id="23534-219">These parameters appear in hello rightmost properties pane of hello Machine Learning UI when hello module is selected.</span></span> <span data-ttu-id="23534-220">Argument kan vara någon av hello stöds eller så kan du skapa en anpassad uppräkning vid behov.</span><span class="sxs-lookup"><span data-stu-id="23534-220">Arguments can be any of hello supported types or you can create a custom enumeration when needed.</span></span> <span data-ttu-id="23534-221">Liknande toohello **portar** element, **argument** element kan ha en valfri **beskrivning** element som anger hello text som visas när du hovrar hello mus över hello parameternamnet.</span><span class="sxs-lookup"><span data-stu-id="23534-221">Similar toohello **Ports** elements, **Arguments** elements can have an optional **Description** element that specifies hello text that appears when you hover hello mouse over hello parameter name.</span></span>
<span data-ttu-id="23534-222">Valfria egenskaper för en modul som standardvärde, minValue och maxValue kan läggas till tooany argumentet som attribut tooa **egenskaper** element.</span><span class="sxs-lookup"><span data-stu-id="23534-222">Optional properties for a module, such as defaultValue, minValue, and maxValue can be added tooany argument as attributes tooa **Properties** element.</span></span> <span data-ttu-id="23534-223">Giltiga egenskaperna för hello **egenskaper** element beroende hello Argumenttypen och beskrivs med hello stöds Argumenttyperna i hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="23534-223">Valid properties for hello **Properties** element depend on hello argument type and are described with hello supported argument types in hello next section.</span></span> <span data-ttu-id="23534-224">Argument med hello **isOptional** egenskapsuppsättning för**”true”** kräver inte hello användaren tooenter ett värde.</span><span class="sxs-lookup"><span data-stu-id="23534-224">Arguments with hello **isOptional** property set too**"true"** do not require hello user tooenter a value.</span></span> <span data-ttu-id="23534-225">Om ett värde inte tillhandahålls toohello argumentet, skickas toohello funktionens startadress inte i hello argumentet.</span><span class="sxs-lookup"><span data-stu-id="23534-225">If a value is not provided toohello argument, then hello argument is not passed toohello entry point function.</span></span> <span data-ttu-id="23534-226">Hello funktionens startadress-argument som är valfritt måste toobe explicit hanteras av hello funktion, t.ex. tilldelade standardvärdet NULL i hello post punkt funktionsdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="23534-226">Arguments of hello entry point function that are optional need toobe explicitly handled by hello function, e.g. assigned a default value of NULL in hello entry point function definition.</span></span> <span data-ttu-id="23534-227">Ett valfritt argument kommer bara att verkställa hello andra argumentet villkor, d.v.s. min eller max, om ett värde som tillhandahålls av hello användare.</span><span class="sxs-lookup"><span data-stu-id="23534-227">An optional argument will only enforce hello other argument constraints, i.e. min or max, if a value is provided by hello user.</span></span>
<span data-ttu-id="23534-228">Det är viktigt att var och en av hello parametrar har unika ID-värden som är kopplade till dem som in- och utdataenheter.</span><span class="sxs-lookup"><span data-stu-id="23534-228">As with inputs and outputs, it is critical that each of hello parameters have unique id values associated with them.</span></span> <span data-ttu-id="23534-229">I vår Snabbstartsguide exempel hello associerade id-parametern har *växlingen*.</span><span class="sxs-lookup"><span data-stu-id="23534-229">In our quick start example hello associated id/parameter was *swap*.</span></span>

### <a name="arg-element"></a><span data-ttu-id="23534-230">%D{arg/ element</span><span class="sxs-lookup"><span data-stu-id="23534-230">Arg element</span></span>
<span data-ttu-id="23534-231">En modul-parameter har definierats med hello **%d{arg/** underordnat element för hello **argument** avsnitt i hello XML-definitionsfilen.</span><span class="sxs-lookup"><span data-stu-id="23534-231">A module parameter is defined using hello **Arg** child element of hello **Arguments** section of hello XML definition file.</span></span> <span data-ttu-id="23534-232">Precis som med hello underordnade element i hello **portar** avsnittet, hello sorteringen av parametrar i hello **argument** avsnittet definierar hello layout påträffades i hello UX.</span><span class="sxs-lookup"><span data-stu-id="23534-232">As with hello child elements in hello **Ports** section, hello ordering of parameters in hello **Arguments** section defines hello layout encountered in hello UX.</span></span> <span data-ttu-id="23534-233">hello parametrar visas, uppifrån nedåt i hello Användargränssnittet i samma ordning i vilken de definieras hello i hello XML-fil.</span><span class="sxs-lookup"><span data-stu-id="23534-233">hello parameters appear from top down in hello UI in hello same order in which they are defined in hello XML file.</span></span> <span data-ttu-id="23534-234">hello-typer som stöds av Machine Learning för parametrar i den här listan.</span><span class="sxs-lookup"><span data-stu-id="23534-234">hello types supported by Machine Learning for parameters are listed here.</span></span> 

<span data-ttu-id="23534-235">**int** – ett heltal (32-bitars) typparameter.</span><span class="sxs-lookup"><span data-stu-id="23534-235">**int** – an Integer (32-bit) type parameter.</span></span>

    <Arg id="intValue1" name="Int Param" type="int">
        <Properties min="0" max="100" default="0" />
        <Description>Integer Parameter</Description>
    </Arg>


* <span data-ttu-id="23534-236">*Valfria egenskaper*: **min**, **max**, **standard** och **isOptional**</span><span class="sxs-lookup"><span data-stu-id="23534-236">*Optional Properties*: **min**, **max**, **default** and **isOptional**</span></span>

<span data-ttu-id="23534-237">**dubbla** – en parameter av typen double.</span><span class="sxs-lookup"><span data-stu-id="23534-237">**double** – a double type parameter.</span></span>

    <Arg id="doubleValue1" name="Double Param" type="double">
        <Properties min="0.000" max="0.999" default="0.3" />
        <Description>Double Parameter</Description>
    </Arg>


* <span data-ttu-id="23534-238">*Valfria egenskaper*: **min**, **max**, **standard** och **isOptional**</span><span class="sxs-lookup"><span data-stu-id="23534-238">*Optional Properties*: **min**, **max**, **default** and **isOptional**</span></span>

<span data-ttu-id="23534-239">**bool** – en boolesk parameter som representeras av en kryssruta i UX.</span><span class="sxs-lookup"><span data-stu-id="23534-239">**bool** – a Boolean parameter that is represented by a check-box in UX.</span></span>

    <Arg id="boolValue1" name="Boolean Param" type="bool">
        <Properties default="true" />
        <Description>Boolean Parameter</Description>
    </Arg>



* <span data-ttu-id="23534-240">*Valfria egenskaper*: **standard** -falskt om inte angetts</span><span class="sxs-lookup"><span data-stu-id="23534-240">*Optional Properties*: **default** - false if not set</span></span>

<span data-ttu-id="23534-241">**strängen**: en vanlig sträng</span><span class="sxs-lookup"><span data-stu-id="23534-241">**string**: a standard string</span></span>

    <Arg id="stringValue1" name="My string Param" type="string">
        <Properties isOptional="true" />
        <Description>String Parameter 1</Description>
    </Arg>    

* <span data-ttu-id="23534-242">*Valfria egenskaper*: **standard** och **isOptional**</span><span class="sxs-lookup"><span data-stu-id="23534-242">*Optional Properties*: **default** and **isOptional**</span></span>

<span data-ttu-id="23534-243">**ColumnPicker**: en parameter för val av kolumn.</span><span class="sxs-lookup"><span data-stu-id="23534-243">**ColumnPicker**: a column selection parameter.</span></span> <span data-ttu-id="23534-244">Den här typen visas i hello UX med väljaren för en kolumn.</span><span class="sxs-lookup"><span data-stu-id="23534-244">This type renders in hello UX as a column chooser.</span></span> <span data-ttu-id="23534-245">Hej **egenskapen** element är används här toospecify hello-id för hello port från vilken kolumner väljs, där hello-porttyp måste vara *DataTable*.</span><span class="sxs-lookup"><span data-stu-id="23534-245">hello **Property** element is used here toospecify hello id of hello port from which columns are selected, where hello target port type must be *DataTable*.</span></span> <span data-ttu-id="23534-246">hello resultatet av hello kolumnmarkeringen skickas toohello R-funktionen som en lista med strängar som innehåller hello valt kolumnnamn.</span><span class="sxs-lookup"><span data-stu-id="23534-246">hello result of hello column selection is passed toohello R function as a list of strings containing hello selected column names.</span></span> 

        <Arg id="colset" name="Column set" type="ColumnPicker">      
          <Properties portId="datasetIn1" allowedTypes="Numeric" default="NumericAll"/>
          <Description>Column set</Description>
        </Arg>


* <span data-ttu-id="23534-247">*Obligatoriska egenskaper*: **portId** -matchar hello-id för ett indata-element med typen *DataTable*.</span><span class="sxs-lookup"><span data-stu-id="23534-247">*Required Properties*: **portId** - matches hello id of an Input element with type *DataTable*.</span></span>
* <span data-ttu-id="23534-248">*Valfria egenskaper*:</span><span class="sxs-lookup"><span data-stu-id="23534-248">*Optional Properties*:</span></span>
  
  * <span data-ttu-id="23534-249">**allowedTypes** -filter hello kolumnen typer från vilken du kan välja.</span><span class="sxs-lookup"><span data-stu-id="23534-249">**allowedTypes** - Filters hello column types from which you can pick.</span></span> <span data-ttu-id="23534-250">Giltiga värden är:</span><span class="sxs-lookup"><span data-stu-id="23534-250">Valid values include:</span></span> 
    
    * <span data-ttu-id="23534-251">numeriskt</span><span class="sxs-lookup"><span data-stu-id="23534-251">Numeric</span></span>
    * <span data-ttu-id="23534-252">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="23534-252">Boolean</span></span>
    * <span data-ttu-id="23534-253">Kategoriska</span><span class="sxs-lookup"><span data-stu-id="23534-253">Categorical</span></span>
    * <span data-ttu-id="23534-254">Sträng</span><span class="sxs-lookup"><span data-stu-id="23534-254">String</span></span>
    * <span data-ttu-id="23534-255">Etikett</span><span class="sxs-lookup"><span data-stu-id="23534-255">Label</span></span>
    * <span data-ttu-id="23534-256">Funktion</span><span class="sxs-lookup"><span data-stu-id="23534-256">Feature</span></span>
    * <span data-ttu-id="23534-257">Poäng</span><span class="sxs-lookup"><span data-stu-id="23534-257">Score</span></span>
    * <span data-ttu-id="23534-258">Alla</span><span class="sxs-lookup"><span data-stu-id="23534-258">All</span></span>
  * <span data-ttu-id="23534-259">**standard** -giltig standardvalen för hello kolumnen Väljaren inkluderar:</span><span class="sxs-lookup"><span data-stu-id="23534-259">**default** - Valid default selections for hello column picker include:</span></span> 
    
    * <span data-ttu-id="23534-260">Ingen</span><span class="sxs-lookup"><span data-stu-id="23534-260">None</span></span>
    * <span data-ttu-id="23534-261">NumericFeature</span><span class="sxs-lookup"><span data-stu-id="23534-261">NumericFeature</span></span>
    * <span data-ttu-id="23534-262">NumericLabel</span><span class="sxs-lookup"><span data-stu-id="23534-262">NumericLabel</span></span>
    * <span data-ttu-id="23534-263">NumericScore</span><span class="sxs-lookup"><span data-stu-id="23534-263">NumericScore</span></span>
    * <span data-ttu-id="23534-264">NumericAll</span><span class="sxs-lookup"><span data-stu-id="23534-264">NumericAll</span></span>
    * <span data-ttu-id="23534-265">BooleanFeature</span><span class="sxs-lookup"><span data-stu-id="23534-265">BooleanFeature</span></span>
    * <span data-ttu-id="23534-266">BooleanLabel</span><span class="sxs-lookup"><span data-stu-id="23534-266">BooleanLabel</span></span>
    * <span data-ttu-id="23534-267">BooleanScore</span><span class="sxs-lookup"><span data-stu-id="23534-267">BooleanScore</span></span>
    * <span data-ttu-id="23534-268">BooleanAll</span><span class="sxs-lookup"><span data-stu-id="23534-268">BooleanAll</span></span>
    * <span data-ttu-id="23534-269">CategoricalFeature</span><span class="sxs-lookup"><span data-stu-id="23534-269">CategoricalFeature</span></span>
    * <span data-ttu-id="23534-270">CategoricalLabel</span><span class="sxs-lookup"><span data-stu-id="23534-270">CategoricalLabel</span></span>
    * <span data-ttu-id="23534-271">CategoricalScore</span><span class="sxs-lookup"><span data-stu-id="23534-271">CategoricalScore</span></span>
    * <span data-ttu-id="23534-272">CategoricalAll</span><span class="sxs-lookup"><span data-stu-id="23534-272">CategoricalAll</span></span>
    * <span data-ttu-id="23534-273">StringFeature</span><span class="sxs-lookup"><span data-stu-id="23534-273">StringFeature</span></span>
    * <span data-ttu-id="23534-274">StringLabel</span><span class="sxs-lookup"><span data-stu-id="23534-274">StringLabel</span></span>
    * <span data-ttu-id="23534-275">StringScore</span><span class="sxs-lookup"><span data-stu-id="23534-275">StringScore</span></span>
    * <span data-ttu-id="23534-276">StringAll</span><span class="sxs-lookup"><span data-stu-id="23534-276">StringAll</span></span>
    * <span data-ttu-id="23534-277">AllLabel</span><span class="sxs-lookup"><span data-stu-id="23534-277">AllLabel</span></span>
    * <span data-ttu-id="23534-278">AllFeature</span><span class="sxs-lookup"><span data-stu-id="23534-278">AllFeature</span></span>
    * <span data-ttu-id="23534-279">AllScore</span><span class="sxs-lookup"><span data-stu-id="23534-279">AllScore</span></span>
    * <span data-ttu-id="23534-280">Alla</span><span class="sxs-lookup"><span data-stu-id="23534-280">All</span></span>

<span data-ttu-id="23534-281">**Listrutan**: ett användardefinierat uppräknade () listrutan.</span><span class="sxs-lookup"><span data-stu-id="23534-281">**DropDown**: a user-specified enumerated (dropdown) list.</span></span> <span data-ttu-id="23534-282">hello nedrullningsbara objekt har angetts i hello **egenskaper** element med ett **objektet** element.</span><span class="sxs-lookup"><span data-stu-id="23534-282">hello dropdown items are specified within hello **Properties** element using an **Item** element.</span></span> <span data-ttu-id="23534-283">Hej **id** för varje **objektet** måste vara unika och en giltig R-variabel.</span><span class="sxs-lookup"><span data-stu-id="23534-283">hello **id** for each **Item** must be unique and a valid R variable.</span></span> <span data-ttu-id="23534-284">Hej värdet för hello **namn** av en **objektet** fungerar både som hello text som visas och hello-värde som har överförts toohello R-funktionen.</span><span class="sxs-lookup"><span data-stu-id="23534-284">hello value of hello **name** of an **Item** serves as both hello text that you see and hello value that is passed toohello R function.</span></span>

    <Arg id="color" name="Color" type="DropDown">
      <Properties default="red">
        <Item id="red" name="Red Value"/>
        <Item id="green" name="Green Value"/>
        <Item id="blue" name="Blue Value"/>
      </Properties>
      <Description>Select a color.</Description>
    </Arg>    

* <span data-ttu-id="23534-285">*Valfria egenskaper*:</span><span class="sxs-lookup"><span data-stu-id="23534-285">*Optional Properties*:</span></span>
  * <span data-ttu-id="23534-286">**standard** - hello värde hello standardegenskapen måste överensstämma med ett id-värde från en hello **objektet** element.</span><span class="sxs-lookup"><span data-stu-id="23534-286">**default** - hello value for hello default property must correspond with an id value from one of hello **Item** elements.</span></span>

### <a name="auxiliary-files"></a><span data-ttu-id="23534-287">Extra filer</span><span class="sxs-lookup"><span data-stu-id="23534-287">Auxiliary Files</span></span>
<span data-ttu-id="23534-288">Alla filer som placeras i en anpassad modul ZIP-filen är pågående toobe tillgängliga för användning under körning.</span><span class="sxs-lookup"><span data-stu-id="23534-288">Any file that is placed in your custom module ZIP file is going toobe available for use during execution time.</span></span> <span data-ttu-id="23534-289">Alla finns katalogstrukturer bevaras.</span><span class="sxs-lookup"><span data-stu-id="23534-289">Any directory structures present are preserved.</span></span> <span data-ttu-id="23534-290">Det innebär att filen ursprung fungerar hello samma lokalt och i Azure Machine Learning-körning.</span><span class="sxs-lookup"><span data-stu-id="23534-290">This means that file sourcing works hello same locally and in Azure Machine Learning execution.</span></span> 

> [!NOTE]
> <span data-ttu-id="23534-291">Observera att alla filer är extraherade too'src' directory så att alla sökvägar som ska ha ' src /-prefix.</span><span class="sxs-lookup"><span data-stu-id="23534-291">Notice that all files are extracted too‘src’ directory so all paths should have ‘src/’ prefix.</span></span>
> 
> 

<span data-ttu-id="23534-292">Anta exempelvis att du vill tooremove några rader med NAs från hello datamängd och även ta bort alla dubbla rader innan mata ut den till CustomAddRows och du redan har skrivit ett R-funktion som gör detta i en fil RemoveDupNARows.R:</span><span class="sxs-lookup"><span data-stu-id="23534-292">For example, say you want tooremove any rows with NAs from hello dataset, and also remove any duplicate rows, before outputting it into CustomAddRows, and you’ve already written an R function that does that in a file RemoveDupNARows.R:</span></span>

    RemoveDupNARows <- function(dataFrame) {
        #Remove Duplicate Rows:
        dataFrame <- unique(dataFrame)
        #Remove Rows with NAs:
        finalDataFrame <- dataFrame[complete.cases(dataFrame),]
        return(finalDataFrame)
    }
<span data-ttu-id="23534-293">Du kan källfil hello extra RemoveDupNARows.R i hello CustomAddRows funktionen:</span><span class="sxs-lookup"><span data-stu-id="23534-293">You can source hello auxiliary file RemoveDupNARows.R in hello CustomAddRows function:</span></span>

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) {
        source("src/RemoveDupNARows.R")
            if (swap) { 
                dataset <- rbind(dataset2, dataset1))
             } else { 
                  dataset <- rbind(dataset1, dataset2)) 
             } 
        dataset <- removeDupNARows(dataset)
        return (dataset)
    }

<span data-ttu-id="23534-294">Därefter ladda upp en zip-fil som innehåller 'CustomAddRows.R', 'CustomAddRows.xml' och 'RemoveDupNARows.R' som en anpassad R-modul.</span><span class="sxs-lookup"><span data-stu-id="23534-294">Next, upload a zip file containing ‘CustomAddRows.R’, ‘CustomAddRows.xml’, and ‘RemoveDupNARows.R’ as a custom R module.</span></span>

## <a name="execution-environment"></a><span data-ttu-id="23534-295">Körningsmiljö</span><span class="sxs-lookup"><span data-stu-id="23534-295">Execution Environment</span></span>
<span data-ttu-id="23534-296">hello körningsmiljö för hello R-skriptet hello använder samma version av R som hello **köra R-skriptet** modul och kan använda hello samma paket som standard.</span><span class="sxs-lookup"><span data-stu-id="23534-296">hello execution environment for hello R script uses hello same version of R as hello **Execute R Script** module and can use hello same default packages.</span></span> <span data-ttu-id="23534-297">Du kan också lägga till ytterligare R-paket tooyour anpassad modul genom att inkludera dem i hello anpassad modul zip-paketet.</span><span class="sxs-lookup"><span data-stu-id="23534-297">You can also add additional R packages tooyour custom module by including them in hello custom module zip package.</span></span> <span data-ttu-id="23534-298">Bara läsa in dem i din R-skriptet som i din egen miljö för R.</span><span class="sxs-lookup"><span data-stu-id="23534-298">Just load them in your R script as you would in your own R environment.</span></span> 

<span data-ttu-id="23534-299">**Begränsningar av hello körningsmiljö** omfattar:</span><span class="sxs-lookup"><span data-stu-id="23534-299">**Limitations of hello execution environment** include:</span></span>

* <span data-ttu-id="23534-300">Icke-beständiga filsystem: filer som skrivits när hello anpassad modul körs är inte beständiga över flera körningar av hello samma modul.</span><span class="sxs-lookup"><span data-stu-id="23534-300">Non-persistent file system: Files written when hello custom module is run are not persisted across multiple runs of hello same module.</span></span>
* <span data-ttu-id="23534-301">Ingen nätverksåtkomst</span><span class="sxs-lookup"><span data-stu-id="23534-301">No network access</span></span>

