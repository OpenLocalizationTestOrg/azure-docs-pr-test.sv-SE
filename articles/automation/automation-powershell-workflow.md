---
title: "aaaLearning PowerShell-arbetsflöde för Azure Automation | Microsoft Docs"
description: "Den här artikeln är avsedd som en snabb lektionen för författare bekant med PowerShell toounderstand hello specifika skillnader mellan PowerShell och PowerShell-arbetsflöde och begrepp tillämpliga tooAutomation runbooks."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 84bf133e-5343-4e0e-8d6c-bb14304a70db
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/21/2017
ms.author: magoedte;bwren
ms.openlocfilehash: 362c504eb96d31b99a826b128e6a591beecaa084
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="learning-key-windows-powershell-workflow-concepts-for-automation-runbooks"></a><span data-ttu-id="dd9bd-103">Learning nyckelbegrepp i Windows PowerShell-arbetsflöde för Automation-runbooks</span><span class="sxs-lookup"><span data-stu-id="dd9bd-103">Learning key Windows PowerShell Workflow concepts for Automation runbooks</span></span> 
<span data-ttu-id="dd9bd-104">Azure Automation-Runbooks implementeras som Windows PowerShell-arbetsflöden.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-104">Runbooks in Azure Automation are implemented as Windows PowerShell Workflows.</span></span>  <span data-ttu-id="dd9bd-105">En Windows PowerShell-arbetsflöde är liknande tooa Windows PowerShell-skript men vissa viktiga skillnader som kan vara förvirrande tooa ny användare.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-105">A Windows PowerShell Workflow is similar tooa Windows PowerShell script but has some significant differences that can be confusing tooa new user.</span></span>  <span data-ttu-id="dd9bd-106">Den här artikeln är avsedd toohelp du skriver runbooks med PowerShell-arbetsflöde, rekommenderar vi att du skriver runbooks med PowerShell om du inte behöver kontrollpunkter.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-106">While this article is intended toohelp you write runbooks using PowerShell workflow, we recommend you write runbooks using PowerShell unless you need checkpoints.</span></span>  <span data-ttu-id="dd9bd-107">Det finns flera syntax skillnader vid redigering av runbooks med PowerShell-arbetsflöde och skillnaderna kräver lite mer arbete toowrite effektiva arbetsflöden.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-107">There are several syntax differences when authoring PowerShell Workflow runbooks and these differences require a bit more work toowrite effective workflows.</span></span>  

<span data-ttu-id="dd9bd-108">Ett arbetsflöde är en sekvens med programmerade, nätverksanslutna steg som utför tidskrävande uppgifter eller kräver hello samordning av flera steg i flera enheter eller hanterade noder.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-108">A workflow is a sequence of programmed, connected steps that perform long-running tasks or require hello coordination of multiple steps across multiple devices or managed nodes.</span></span> <span data-ttu-id="dd9bd-109">hello fördelarna med ett arbetsflöde jämfört med ett vanligt skript inkluderar hello möjlighet toosimultaneously utföra en åtgärd mot flera enheter och hello möjlighet tooautomatically återställning vid fel.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-109">hello benefits of a workflow over a normal script include hello ability toosimultaneously perform an action against multiple devices and hello ability tooautomatically recover from failures.</span></span> <span data-ttu-id="dd9bd-110">En Windows PowerShell-arbetsflöde är ett Windows PowerShell-skript som använder Windows Workflow Foundation.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-110">A Windows PowerShell Workflow is a Windows PowerShell script that uses Windows Workflow Foundation.</span></span> <span data-ttu-id="dd9bd-111">Även om hello arbetsflödet skrivs med Windows PowerShell-syntax och startas av Windows PowerShell, bearbetas men det av Windows Workflow Foundation.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-111">While hello workflow is written with Windows PowerShell syntax and launched by Windows PowerShell, it is processed by Windows Workflow Foundation.</span></span>

<span data-ttu-id="dd9bd-112">Fullständig information om hello avsnitt i den här artikeln finns [komma igång med Windows PowerShell-arbetsflöde](http://technet.microsoft.com/library/jj134242.aspx).</span><span class="sxs-lookup"><span data-stu-id="dd9bd-112">For complete details on hello topics in this article, see [Getting Started with Windows PowerShell Workflow](http://technet.microsoft.com/library/jj134242.aspx).</span></span>

## <a name="basic-structure-of-a-workflow"></a><span data-ttu-id="dd9bd-113">Grundläggande struktur för ett arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="dd9bd-113">Basic structure of a workflow</span></span>
<span data-ttu-id="dd9bd-114">hello första steg tooconverting ett PowerShell-skript tooa PowerShell-arbetsflöde omsluta den med hello **arbetsflöde** nyckelord.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-114">hello first step tooconverting a PowerShell script tooa PowerShell workflow is enclosing it with hello **Workflow** keyword.</span></span>  <span data-ttu-id="dd9bd-115">Ett arbetsflöde som startar med hello **arbetsflöde** följt av hello brödtext hello skriptkoden omgiven av klammerparenteser.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-115">A workflow starts with hello **Workflow** keyword followed by hello body of hello script enclosed in braces.</span></span> <span data-ttu-id="dd9bd-116">hello namnet på arbetsflödet hello följer hello **arbetsflöde** nyckelordet enligt hello följande syntax:</span><span class="sxs-lookup"><span data-stu-id="dd9bd-116">hello name of hello workflow follows hello **Workflow** keyword as shown in hello following syntax:</span></span>

    Workflow Test-Workflow
    {
       <Commands>
    }

<span data-ttu-id="dd9bd-117">hello namnet på hello arbetsflödet måste matcha hello namnet i hello Automation-runbook.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-117">hello name of hello workflow must match hello name of hello Automation runbook.</span></span> <span data-ttu-id="dd9bd-118">Om hello runbook importeras sedan hello filename måste matcha hello Arbetsflödesnamn och måste sluta med *.ps1*.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-118">If hello runbook is being imported, then hello filename must match hello workflow name and must end in *.ps1*.</span></span>

<span data-ttu-id="dd9bd-119">tooadd parametrar toohello arbetsflöde, Använd hello **Param** nyckelordet samma sätt som tooa skript.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-119">tooadd parameters toohello workflow, use hello **Param** keyword just as you would tooa script.</span></span>

## <a name="code-changes"></a><span data-ttu-id="dd9bd-120">Kodändringar</span><span class="sxs-lookup"><span data-stu-id="dd9bd-120">Code changes</span></span>
<span data-ttu-id="dd9bd-121">PowerShell-arbetsflöde kod verkar nästan samma tooPowerShell skriptkod förutom några betydande förändringar.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-121">PowerShell workflow code looks almost identical tooPowerShell script code except for a few significant changes.</span></span>  <span data-ttu-id="dd9bd-122">hello följande avsnitt beskrivs de ändringar som du behöver toomake tooa PowerShell-skript för den toorun i ett arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-122">hello following sections describe changes that you need toomake tooa PowerShell script for it toorun in a workflow.</span></span>

### <a name="activities"></a><span data-ttu-id="dd9bd-123">Aktiviteter</span><span class="sxs-lookup"><span data-stu-id="dd9bd-123">Activities</span></span>
<span data-ttu-id="dd9bd-124">En aktivitet är en viss aktivitet i ett arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-124">An activity is a specific task in a workflow.</span></span> <span data-ttu-id="dd9bd-125">Precis som ett skript består av ett eller flera kommandon, består ett arbetsflöde av en eller flera aktiviteter som utförs i en sekvens.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-125">Just as a script is composed of one or more commands, a workflow is composed of one or more activities that are carried out in a sequence.</span></span> <span data-ttu-id="dd9bd-126">Windows PowerShell-arbetsflöde konverterar automatiskt många av hello tooactivities för Windows PowerShell-cmdlets när ett arbetsflöde körs.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-126">Windows PowerShell Workflow automatically converts many of hello Windows PowerShell cmdlets tooactivities when it runs a workflow.</span></span> <span data-ttu-id="dd9bd-127">När du anger en av dessa cmdletar i din runbook, körs hello motsvarande aktivitet i Windows Workflow Foundation.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-127">When you specify one of these cmdlets in your runbook, hello corresponding activity is run by Windows Workflow Foundation.</span></span> <span data-ttu-id="dd9bd-128">För dessa cmdlets utan en motsvarande aktivitet körs Windows PowerShell-arbetsflöde automatiskt hello cmdlet inom en [InlineScript](#inlinescript) aktivitet.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-128">For those cmdlets without a corresponding activity, Windows PowerShell Workflow automatically runs hello cmdlet within an [InlineScript](#inlinescript) activity.</span></span> <span data-ttu-id="dd9bd-129">Det finns en uppsättning cmdlets som är undantagna och inte kan användas i ett arbetsflöde om du uttryckligen lägga till dem i ett InlineScript-block.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-129">There is a set of cmdlets that are excluded and cannot be used in a workflow unless you explicitly include them in an InlineScript block.</span></span> <span data-ttu-id="dd9bd-130">Mer information om dessa koncept finns [med hjälp av aktiviteter i Skriptarbetsflöden](http://technet.microsoft.com/library/jj574194.aspx).</span><span class="sxs-lookup"><span data-stu-id="dd9bd-130">For further details on these concepts, see [Using Activities in Script Workflows](http://technet.microsoft.com/library/jj574194.aspx).</span></span>

<span data-ttu-id="dd9bd-131">Arbetsflödesaktiviteter delar en uppsättning gemensamma parametrar tooconfigure driften.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-131">Workflow activities share a set of common parameters tooconfigure their operation.</span></span> <span data-ttu-id="dd9bd-132">Mer information om gemensamma arbetsflödesparametrar hello finns [about_WorkflowCommonParameters](http://technet.microsoft.com/library/jj129719.aspx).</span><span class="sxs-lookup"><span data-stu-id="dd9bd-132">For details about hello workflow common parameters, see [about_WorkflowCommonParameters](http://technet.microsoft.com/library/jj129719.aspx).</span></span>

### <a name="positional-parameters"></a><span data-ttu-id="dd9bd-133">Namngivna parametrar</span><span class="sxs-lookup"><span data-stu-id="dd9bd-133">Positional parameters</span></span>
<span data-ttu-id="dd9bd-134">Du kan använda namngivna parametrar med aktiviteter och cmdlets i ett arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-134">You can't use positional parameters with activities and cmdlets in a workflow.</span></span>  <span data-ttu-id="dd9bd-135">Det innebär är att du måste använda parameternamn.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-135">All this means is that you must use parameter names.</span></span>

<span data-ttu-id="dd9bd-136">Anta till exempel att hello efter koden som hämtar alla tjänster som körs.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-136">For example, consider hello following code that gets all running services.</span></span>

     Get-Service | Where-Object {$_.Status -eq "Running"}

<span data-ttu-id="dd9bd-137">Om du försöker toorun samma kod i ett arbetsflöde, meddelande ett som ”parametern set inte kan lösas med hello angetts namngivna parametrar”.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-137">If you try toorun this same code in a workflow, you receive a message like "Parameter set cannot be resolved using hello specified named parameters."</span></span>  <span data-ttu-id="dd9bd-138">toocorrect, ange hello parameternamn som hello följande.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-138">toocorrect this, provide hello parameter name as in hello following.</span></span>

    Workflow Get-RunningServices
    {
        Get-Service | Where-Object -FilterScript {$_.Status -eq "Running"}
    }

### <a name="deserialized-objects"></a><span data-ttu-id="dd9bd-139">Avserialiserat objekt</span><span class="sxs-lookup"><span data-stu-id="dd9bd-139">Deserialized objects</span></span>
<span data-ttu-id="dd9bd-140">Objekt i arbetsflöden är avserialiseras.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-140">Objects in workflows are deserialized.</span></span>  <span data-ttu-id="dd9bd-141">Det innebär att deras egenskaper finns kvar, men inte deras metoder.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-141">This means that their properties are still available, but not their methods.</span></span>  <span data-ttu-id="dd9bd-142">Tänk dig följande PowerShell-koden som stoppar en tjänst med hello stoppmetoden av hello webbtjänstobjektet hello.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-142">For example, consider hello following PowerShell code that stops a service using hello Stop method of hello Service object.</span></span>

    $Service = Get-Service -Name MyService
    $Service.Stop()

<span data-ttu-id="dd9bd-143">Om du försöker toorun detta i ett arbetsflöde, får du ett felmeddelande som säger ”metodanropet inte stöds i en Windows PowerShell-arbetsflöde”.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-143">If you try toorun this in a workflow, you receive an error saying "Method invocation is not supported in a Windows PowerShell Workflow."</span></span>  

<span data-ttu-id="dd9bd-144">Ett alternativ är toowrap dessa två rader med kod i en [InlineScript](#inlinescript) blockera i vilket fall $Service skulle vara en serviceobjektet inom hello-block.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-144">One option is toowrap these two lines of code in an [InlineScript](#inlinescript) block in which case $Service would be a service object within hello block.</span></span>

    Workflow Stop-Service
    {
        InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
        }
    }

<span data-ttu-id="dd9bd-145">Ett annat alternativ är toouse en annan cmdlet som utför hello samma funktioner som hello-metoden, om det inte finns.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-145">Another option is toouse another cmdlet that performs hello same functionality as hello method, if one is available.</span></span>  <span data-ttu-id="dd9bd-146">I vårt exempel hello Stop-Service cmdlet ger hello samma funktioner som hello stoppmetoden, och du kan använda följande hello för ett arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-146">In our sample, hello Stop-Service cmdlet provides hello same functionality as hello Stop method, and you could use hello following for a workflow.</span></span>

    Workflow Stop-MyService
    {
        $Service = Get-Service -Name MyService
        Stop-Service -Name $Service.Name
    }


## <a name="inlinescript"></a><span data-ttu-id="dd9bd-147">InlineScript</span><span class="sxs-lookup"><span data-stu-id="dd9bd-147">InlineScript</span></span>
<span data-ttu-id="dd9bd-148">Hej **InlineScript** aktivitet är användbart när du behöver toorun ett eller flera kommandon som traditionella PowerShell-skript i stället för PowerShell-arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-148">hello **InlineScript** activity is useful when you need toorun one or more commands as traditional PowerShell script instead of PowerShell workflow.</span></span>  <span data-ttu-id="dd9bd-149">Medan kommandon i ett arbetsflöde skickas tooWindows Workflow Foundation för bearbetning, bearbetas kommandona i ett InlineScript-block av Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-149">While commands in a workflow are sent tooWindows Workflow Foundation for processing, commands in an InlineScript block are processed by Windows PowerShell.</span></span>

<span data-ttu-id="dd9bd-150">InlineScript använder hello följande syntax som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-150">InlineScript uses hello following syntax shown below.</span></span>

    InlineScript
    {
      <Script Block>
    } <Common Parameters>

<span data-ttu-id="dd9bd-151">Du kan returnera utdata från en InlineScript genom att tilldela hello utdata tooa variabeln.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-151">You can return output from an InlineScript by assigning hello output tooa variable.</span></span> <span data-ttu-id="dd9bd-152">hello följande exempel stoppar en tjänst och matar ut hello tjänstnamn.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-152">hello following example stops a service and then outputs hello service name.</span></span>

    Workflow Stop-MyService
    {
        $Output = InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


<span data-ttu-id="dd9bd-153">Du kan skicka värden i ett InlineScript-block, men du måste använda **$Using** omfångsmodifieraren.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-153">You can pass values into an InlineScript block, but you must use **$Using** scope modifier.</span></span>  <span data-ttu-id="dd9bd-154">hello följande exempel är identiska toohello föregående exempel förutom att hello tjänstnamn tillhandahålls av en variabel.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-154">hello following example is identical toohello previous example except that hello service name is provided by a variable.</span></span>

    Workflow Stop-MyService
    {
        $ServiceName = "MyService"

        $Output = InlineScript {
            $Service = Get-Service -Name $Using:ServiceName
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


<span data-ttu-id="dd9bd-155">InlineScript-aktiviteter kan vara avgörande i vissa arbetsflöden, stöder inte arbetsflöde för konstruktionerna och bör endast användas när det behövs för hello följande orsaker:</span><span class="sxs-lookup"><span data-stu-id="dd9bd-155">While InlineScript activities may be critical in certain workflows, they do not support workflow constructs and should only be used when necessary for hello following reasons:</span></span>

* <span data-ttu-id="dd9bd-156">Du kan inte använda [kontrollpunkter](#checkpoints) i ett InlineScript-block.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-156">You cannot use [checkpoints](#checkpoints) inside an InlineScript block.</span></span> <span data-ttu-id="dd9bd-157">Om ett fel inträffar inom hello block, måste det köras från hello början av hello-block.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-157">If a failure occurs within hello block, it must be resumed from hello beginning of hello block.</span></span>
* <span data-ttu-id="dd9bd-158">Du kan inte använda [parallell körning](#parallel-processing) inuti en InlineScriptBlock.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-158">You cannot use [parallel execution](#parallel-processing) inside an InlineScriptBlock.</span></span>
* <span data-ttu-id="dd9bd-159">InlineScript påverkar skalbarhet hello arbetsflödet eftersom den innehåller hello Windows PowerShell-session för hello hello InlineScript-blockets hela längd.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-159">InlineScript affects scalability of hello workflow since it holds hello Windows PowerShell session for hello entire length of hello InlineScript block.</span></span>

<span data-ttu-id="dd9bd-160">Läs mer om hur du använder InlineScript [kör du Windows PowerShell-kommandon i ett arbetsflöde](http://technet.microsoft.com/library/jj574197.aspx) och [about_InlineScript](http://technet.microsoft.com/library/jj649082.aspx).</span><span class="sxs-lookup"><span data-stu-id="dd9bd-160">For more information on using InlineScript, see [Running Windows PowerShell Commands in a Workflow](http://technet.microsoft.com/library/jj574197.aspx) and [about_InlineScript](http://technet.microsoft.com/library/jj649082.aspx).</span></span>

## <a name="parallel-processing"></a><span data-ttu-id="dd9bd-161">Parallell bearbetning</span><span class="sxs-lookup"><span data-stu-id="dd9bd-161">Parallel processing</span></span>
<span data-ttu-id="dd9bd-162">En fördel med Windows PowerShell-arbetsflöden är hello möjlighet tooperform en uppsättning kommandon parallellt i stället för sekventiellt som i ett vanligt skript.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-162">One advantage of Windows PowerShell Workflows is hello ability tooperform a set of commands in parallel instead of sequentially as with a typical script.</span></span>

<span data-ttu-id="dd9bd-163">Du kan använda hello **parallella** nyckelordet toocreate ett skriptblock med flera kommandon som körs samtidigt.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-163">You can use hello **Parallel** keyword toocreate a script block with multiple commands that run concurrently.</span></span> <span data-ttu-id="dd9bd-164">Här används hello följande syntax som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-164">This uses hello following syntax shown below.</span></span> <span data-ttu-id="dd9bd-165">I det här fallet Activity1 och Activity2 börjar vid hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-165">In this case, Activity1 and Activity2 starts at hello same time.</span></span> <span data-ttu-id="dd9bd-166">Activity3 startas endast efter att både Activity1 och Activity2 har slutförts.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-166">Activity3 starts only after both Activity1 and Activity2 have completed.</span></span>

    Parallel
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>


<span data-ttu-id="dd9bd-167">Anta till exempel att hello följande PowerShell-kommandon som kopierar flera filer tooa nätverk mål.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-167">For example, consider hello following PowerShell commands that copy multiple files tooa network destination.</span></span>  <span data-ttu-id="dd9bd-168">Dessa kommandon körs sekventiellt så att en fil måste avslutas kopierats innan hello nästa gång.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-168">These commands are run sequentially so that one file must finish copying before hello next is started.</span></span>     

    Copy-Item -Path C:\LocalPath\File1.txt -Destination \\NetworkPath\File1.txt
    Copy-Item -Path C:\LocalPath\File2.txt -Destination \\NetworkPath\File2.txt
    Copy-Item -Path C:\LocalPath\File3.txt -Destination \\NetworkPath\File3.txt

<span data-ttu-id="dd9bd-169">hello följande arbetsflöde kör dessa samma kommandon parallellt så att alla börja kopiera på hello samma tid.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-169">hello following workflow runs these same commands in parallel so that they all start copying at hello same time.</span></span>  <span data-ttu-id="dd9bd-170">Endast när de är alla visas kopieras slutförande hälsningsmeddelande.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-170">Only after they are all copied is hello completion message displayed.</span></span>

    Workflow Copy-Files
    {
        Parallel
        {
            Copy-Item -Path "C:\LocalPath\File1.txt" -Destination "\\NetworkPath"
            Copy-Item -Path "C:\LocalPath\File2.txt" -Destination "\\NetworkPath"
            Copy-Item -Path "C:\LocalPath\File3.txt" -Destination "\\NetworkPath"
        }

        Write-Output "Files copied."
    }


<span data-ttu-id="dd9bd-171">Du kan använda hello **ForEach-Parallel** konstruera tooprocess kommandon för varje objekt i en samling samtidigt.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-171">You can use hello **ForEach -Parallel** construct tooprocess commands for each item in a collection concurrently.</span></span> <span data-ttu-id="dd9bd-172">hello objekten i hello samlingen bearbetas parallellt medan hello kommandona i hello-skriptblocket körs sekventiellt.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-172">hello items in hello collection are processed in parallel while hello commands in hello script block run sequentially.</span></span> <span data-ttu-id="dd9bd-173">Här används hello följande syntax som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-173">This uses hello following syntax shown below.</span></span> <span data-ttu-id="dd9bd-174">I det här fallet Activity1 startar enligt hello samma tid för alla objekt i hello samling.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-174">In this case, Activity1 starts at hello same time for all items in hello collection.</span></span> <span data-ttu-id="dd9bd-175">För varje objekt startar Activity2 efter att Activity1 har slutförts.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-175">For each item, Activity2 starts after Activity1 is complete.</span></span> <span data-ttu-id="dd9bd-176">Activity3 startas endast efter att både Activity1 och Activity2 har slutförts för alla objekt.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-176">Activity3 starts only after both Activity1 and Activity2 have completed for all items.</span></span>

    ForEach -Parallel ($<item> in $<collection>)
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>

<span data-ttu-id="dd9bd-177">följande exempel hello är liknande toohello-föregående exempel filkopiering parallellt.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-177">hello following example is similar toohello previous example copying files in parallel.</span></span>  <span data-ttu-id="dd9bd-178">I det här fallet visas ett meddelande för varje fil när den kopierar.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-178">In this case, a message is displayed for each file after it copies.</span></span>  <span data-ttu-id="dd9bd-179">Endast när de är alla visas kopierats slutliga slutförande hälsningsmeddelande.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-179">Only after they are all completely copied is hello final completion message displayed.</span></span>

    Workflow Copy-Files
    {
        $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

        ForEach -Parallel ($File in $Files)
        {
            Copy-Item -Path $File -Destination \\NetworkPath
            Write-Output "$File copied."
        }

        Write-Output "All files copied."
    }

> [!NOTE]
> <span data-ttu-id="dd9bd-180">Vi rekommenderar inte underordnade runbooks som körs parallellt eftersom det har visats toogive osäkra resultat.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-180">We do not recommend running child runbooks in parallel since this has been shown toogive unreliable results.</span></span>  <span data-ttu-id="dd9bd-181">hello utdata från hello underordnad runbook som ibland visas inte och inställningar i en underordnad runbook kan påverka hello andra parallella underordnade runbooks</span><span class="sxs-lookup"><span data-stu-id="dd9bd-181">hello output from hello child runbook sometimes does not show up, and settings in one child runbook can affect hello other parallel child runbooks</span></span>
>

## <a name="checkpoints"></a><span data-ttu-id="dd9bd-182">Kontrollpunkter</span><span class="sxs-lookup"><span data-stu-id="dd9bd-182">Checkpoints</span></span>
<span data-ttu-id="dd9bd-183">En *kontrollpunkt* är en ögonblicksbild av hello aktuell status för hello arbetsflöde som inkluderar hello aktuella värdet för variabler och eventuella utdata genererade toothat punkt.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-183">A *checkpoint* is a snapshot of hello current state of hello workflow that includes hello current value for variables and any output generated toothat point.</span></span> <span data-ttu-id="dd9bd-184">Om ett arbetsflöde slutar i fel eller har pausats, sedan startar hello nästa gång den körs den från den senaste kontrollpunkten i stället för hello början av hello worfklow.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-184">If a workflow ends in error or is suspended, then hello next time it is run it will start from its last checkpoint instead of hello start of hello worfklow.</span></span>  <span data-ttu-id="dd9bd-185">Du kan ange en kontrollpunkt i ett arbetsflöde med hello **Checkpoint-Workflow** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-185">You can set a checkpoint in a workflow with hello **Checkpoint-Workflow** activity.</span></span>

<span data-ttu-id="dd9bd-186">I följande exempelkod hello, inträffar ett undantag efter Activity2 orsakar hello arbetsflöde tooend.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-186">In hello following sample code, an exception occurs after Activity2 causing hello workflow tooend.</span></span> <span data-ttu-id="dd9bd-187">När hello arbetsflödet körs igen, startar det genom att köra Activity2 eftersom den bara när hello senast lagrade kontrollpunkten.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-187">When hello workflow is run again, it starts by running Activity2 since this was just after hello last checkpoint set.</span></span>

    <Activity1>
    Checkpoint-Workflow
    <Activity2>
    <Exception>
    <Activity3>

<span data-ttu-id="dd9bd-188">Du bör lagra kontrollpunkter i ett arbetsflöde efter aktiviteter som kan vara felbenägna tooexception och får inte vara upprepas om hello arbetsflödet återupptas.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-188">You should set checkpoints in a workflow after activities that may be prone tooexception and should not be repeated if hello workflow is resumed.</span></span> <span data-ttu-id="dd9bd-189">Arbetsflödet kan till exempel skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-189">For example, your workflow may create a virtual machine.</span></span> <span data-ttu-id="dd9bd-190">Du kan ange en kontrollpunkt både före och efter hello kommandon toocreate hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-190">You could set a checkpoint both before and after hello commands toocreate hello virtual machine.</span></span> <span data-ttu-id="dd9bd-191">Om hello misslyckas, skulle sedan hello kommandon upprepas om hello arbetsflödet startas igen.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-191">If hello creation fails, then hello commands would be repeated if hello workflow is started again.</span></span> <span data-ttu-id="dd9bd-192">Om hello worfklow misslyckas när hello skapandet lyckas sedan skapas hello virtuella datorn inte igen när hello arbetsflödet återupptas.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-192">If hello worfklow fails after hello creation succeeds, then hello virtual machine will not be created again when hello workflow is resumed.</span></span>

<span data-ttu-id="dd9bd-193">hello följande exempel kopierar flera filer tooa nätverksplats och anger en kontrollpunkt efter varje fil.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-193">hello following example copies multiple files tooa network location and sets a checkpoint after each file.</span></span>  <span data-ttu-id="dd9bd-194">Om hello nätverksplats tappas bort, slutar hello arbetsflödet i fel.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-194">If hello network location is lost, then hello workflow ends in error.</span></span>  <span data-ttu-id="dd9bd-195">När den startas igen fortsätter den på hello senaste kontrollpunkten, vilket innebär att endast hello-filer som redan har kopierats hoppas över.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-195">When it is started again, it will resume at hello last checkpoint meaning that only hello files that have already been copied are skipped.</span></span>

    Workflow Copy-Files
    {
        $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

        ForEach ($File in $Files)
        {
            Copy-Item -Path $File -Destination \\NetworkPath
            Write-Output "$File copied."
            Checkpoint-Workflow
        }

        Write-Output "All files copied."
    }

<span data-ttu-id="dd9bd-196">Eftersom användarnamn autentiseringsuppgifter inte sparas när du anropar hello [Suspend-Workflow](https://technet.microsoft.com/library/jj733586.aspx) aktivitet eller efter hello senaste kontrollpunkten, du behöver tooset hello autentiseringsuppgifter toonull och sedan hämta dem igen från hello tillgången store efter  **Pausa arbetsflödet** eller kontrollpunkt anropades.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-196">Because username credentials are not persisted after you call hello [Suspend-Workflow](https://technet.microsoft.com/library/jj733586.aspx) activity or after hello last checkpoint, you need tooset hello credentials toonull and then retrieve them again from hello asset store after **Suspend-Workflow** or checkpoint is called.</span></span>  <span data-ttu-id="dd9bd-197">I annat fall visas följande felmeddelande hello: *går inte att återuppta arbetsflödesjobbet hello, antingen eftersom beständiga data inte kunde spara helt eller spara beständiga data har skadats. Du måste starta om hello arbetsflöde.*</span><span class="sxs-lookup"><span data-stu-id="dd9bd-197">Otherwise, you may receive hello following error message: *hello workflow job cannot be resumed, either because persistence data could not be saved completely, or saved persistence data has been corrupted. You must restart hello workflow.*</span></span>

<span data-ttu-id="dd9bd-198">hello följa samma kod visar hur toohandle detta i dina runbooks med PowerShell-arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-198">hello following same code demonstrates how toohandle this in your PowerShell Workflow runbooks.</span></span>

    workflow CreateTestVms
    {
       $Cred = Get-AzureAutomationCredential -Name "MyCredential"
       $null = Add-AzureRmAccount -Credential $Cred

       $VmsToCreate = Get-AzureAutomationVariable -Name "VmsToCreate"

       foreach ($VmName in $VmsToCreate)
         {
          # Do work first toocreate hello VM (code not shown)

          # Now add hello VM
          New-AzureRmVm -VM $Vm -Location "WestUs" -ResourceGroupName "ResourceGroup01"

          # Checkpoint so that VM creation is not repeated if workflow suspends
          $Cred = $null
          Checkpoint-Workflow
          $Cred = Get-AzureAutomationCredential -Name "MyCredential"
          $null = Add-AzureRmAccount -Credential $Cred
         }
     }


<span data-ttu-id="dd9bd-199">Detta är inte nödvändigt om du autentiserar med hjälp av en Kör som-konto konfigureras med en tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="dd9bd-199">This is not required if you are authenticating using a Run As account configured with a service principal.</span></span>  

<span data-ttu-id="dd9bd-200">Mer information om kontrollpunkter finns [att lägga till kontrollpunkter tooa arbetsflöde för skript](http://technet.microsoft.com/library/jj574114.aspx).</span><span class="sxs-lookup"><span data-stu-id="dd9bd-200">For more information about checkpoints, see [Adding Checkpoints tooa Script Workflow](http://technet.microsoft.com/library/jj574114.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="dd9bd-201">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dd9bd-201">Next steps</span></span>
* <span data-ttu-id="dd9bd-202">tooget igång med PowerShell arbetsflöde runbooks finns [min första PowerShell-arbetsflödesrunbook](automation-first-runbook-textual.md)</span><span class="sxs-lookup"><span data-stu-id="dd9bd-202">tooget started with PowerShell workflow runbooks, see [My first PowerShell workflow runbook](automation-first-runbook-textual.md)</span></span>
