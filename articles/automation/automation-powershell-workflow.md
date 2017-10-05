---
title: "Learning PowerShell-arbetsflöde för Azure Automation | Microsoft Docs"
description: "Den här artikeln är avsedd som en snabb lektionen för författare som är bekanta med PowerShell att förstå de specifika skillnaderna mellan PowerShell och PowerShell-arbetsflöde och begrepp som gäller för Automation-runbooks."
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
ms.openlocfilehash: 4de812c7f863e42a6ed10c2312d61b8377e06431
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="learning-key-windows-powershell-workflow-concepts-for-automation-runbooks"></a><span data-ttu-id="7c216-103">Learning nyckelbegrepp i Windows PowerShell-arbetsflöde för Automation-runbooks</span><span class="sxs-lookup"><span data-stu-id="7c216-103">Learning key Windows PowerShell Workflow concepts for Automation runbooks</span></span> 
<span data-ttu-id="7c216-104">Azure Automation-Runbooks implementeras som Windows PowerShell-arbetsflöden.</span><span class="sxs-lookup"><span data-stu-id="7c216-104">Runbooks in Azure Automation are implemented as Windows PowerShell Workflows.</span></span>  <span data-ttu-id="7c216-105">En Windows PowerShell-arbetsflöde är liknar en Windows PowerShell-skript men vissa viktiga skillnader som kan vara förvirrande för en ny användare.</span><span class="sxs-lookup"><span data-stu-id="7c216-105">A Windows PowerShell Workflow is similar to a Windows PowerShell script but has some significant differences that can be confusing to a new user.</span></span>  <span data-ttu-id="7c216-106">När den här artikeln är avsedd att hjälpa dig att skriva runbooks med PowerShell-arbetsflöde, rekommenderar vi att du skriver runbooks med PowerShell om du inte behöver kontrollpunkter.</span><span class="sxs-lookup"><span data-stu-id="7c216-106">While this article is intended to help you write runbooks using PowerShell workflow, we recommend you write runbooks using PowerShell unless you need checkpoints.</span></span>  <span data-ttu-id="7c216-107">Det finns flera syntax skillnader vid redigering av runbooks med PowerShell-arbetsflöde och skillnaderna kräver lite mer arbete att skriva effektiva arbetsflöden.</span><span class="sxs-lookup"><span data-stu-id="7c216-107">There are several syntax differences when authoring PowerShell Workflow runbooks and these differences require a bit more work to write effective workflows.</span></span>  

<span data-ttu-id="7c216-108">Ett arbetsflöde är en sekvens med programmerade, nätverksanslutna steg som utför tidskrävande uppgifter eller kräver samordning av flera steg i flera enheter eller hanterade noder.</span><span class="sxs-lookup"><span data-stu-id="7c216-108">A workflow is a sequence of programmed, connected steps that perform long-running tasks or require the coordination of multiple steps across multiple devices or managed nodes.</span></span> <span data-ttu-id="7c216-109">Fördelarna med ett arbetsflöde jämfört med ett vanligt skript inkluderar möjligheten att utföra en åtgärd mot flera enheter samtidigt och möjligheten att automatisk återställning vid fel.</span><span class="sxs-lookup"><span data-stu-id="7c216-109">The benefits of a workflow over a normal script include the ability to simultaneously perform an action against multiple devices and the ability to automatically recover from failures.</span></span> <span data-ttu-id="7c216-110">En Windows PowerShell-arbetsflöde är ett Windows PowerShell-skript som använder Windows Workflow Foundation.</span><span class="sxs-lookup"><span data-stu-id="7c216-110">A Windows PowerShell Workflow is a Windows PowerShell script that uses Windows Workflow Foundation.</span></span> <span data-ttu-id="7c216-111">När arbetsflödet skrivs med Windows PowerShell-syntax och startas av Windows PowerShell, bearbetas men det av Windows Workflow Foundation.</span><span class="sxs-lookup"><span data-stu-id="7c216-111">While the workflow is written with Windows PowerShell syntax and launched by Windows PowerShell, it is processed by Windows Workflow Foundation.</span></span>

<span data-ttu-id="7c216-112">Mer information om ämnen i den här artikeln finns [komma igång med Windows PowerShell-arbetsflöde](http://technet.microsoft.com/library/jj134242.aspx).</span><span class="sxs-lookup"><span data-stu-id="7c216-112">For complete details on the topics in this article, see [Getting Started with Windows PowerShell Workflow](http://technet.microsoft.com/library/jj134242.aspx).</span></span>

## <a name="basic-structure-of-a-workflow"></a><span data-ttu-id="7c216-113">Grundläggande struktur för ett arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="7c216-113">Basic structure of a workflow</span></span>
<span data-ttu-id="7c216-114">Det första steget för att konvertera ett PowerShell-skript till ett PowerShell-arbetsflöde är omsluta den med den **arbetsflöde** nyckelord.</span><span class="sxs-lookup"><span data-stu-id="7c216-114">The first step to converting a PowerShell script to a PowerShell workflow is enclosing it with the **Workflow** keyword.</span></span>  <span data-ttu-id="7c216-115">Ett arbetsflöde som startar med den **arbetsflöde** följt av skriptkoden omgiven av klammerparenteser.</span><span class="sxs-lookup"><span data-stu-id="7c216-115">A workflow starts with the **Workflow** keyword followed by the body of the script enclosed in braces.</span></span> <span data-ttu-id="7c216-116">Namnet på arbetsflödet följer den **arbetsflöde** nyckelord som visas i följande syntax:</span><span class="sxs-lookup"><span data-stu-id="7c216-116">The name of the workflow follows the **Workflow** keyword as shown in the following syntax:</span></span>

    Workflow Test-Workflow
    {
       <Commands>
    }

<span data-ttu-id="7c216-117">Namnet på arbetsflödet måste matcha namnet i Automation-runbook.</span><span class="sxs-lookup"><span data-stu-id="7c216-117">The name of the workflow must match the name of the Automation runbook.</span></span> <span data-ttu-id="7c216-118">Om runbook importeras sedan filnamnet måste matcha namnet på arbetsflödet och måste sluta med *.ps1*.</span><span class="sxs-lookup"><span data-stu-id="7c216-118">If the runbook is being imported, then the filename must match the workflow name and must end in *.ps1*.</span></span>

<span data-ttu-id="7c216-119">Om du vill lägga till parametrar i arbetsflödet, Använd den **Param** nyckelordet precis som du skulle för ett skript.</span><span class="sxs-lookup"><span data-stu-id="7c216-119">To add parameters to the workflow, use the **Param** keyword just as you would to a script.</span></span>

## <a name="code-changes"></a><span data-ttu-id="7c216-120">Kodändringar</span><span class="sxs-lookup"><span data-stu-id="7c216-120">Code changes</span></span>
<span data-ttu-id="7c216-121">PowerShell-arbetsflöde kod verkar nästan identisk med PowerShell skriptkod förutom några betydande förändringar.</span><span class="sxs-lookup"><span data-stu-id="7c216-121">PowerShell workflow code looks almost identical to PowerShell script code except for a few significant changes.</span></span>  <span data-ttu-id="7c216-122">I följande avsnitt beskrivs ändringar som du behöver göra i ett PowerShell-skript för att köra i ett arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="7c216-122">The following sections describe changes that you need to make to a PowerShell script for it to run in a workflow.</span></span>

### <a name="activities"></a><span data-ttu-id="7c216-123">Aktiviteter</span><span class="sxs-lookup"><span data-stu-id="7c216-123">Activities</span></span>
<span data-ttu-id="7c216-124">En aktivitet är en viss aktivitet i ett arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="7c216-124">An activity is a specific task in a workflow.</span></span> <span data-ttu-id="7c216-125">Precis som ett skript består av ett eller flera kommandon, består ett arbetsflöde av en eller flera aktiviteter som utförs i en sekvens.</span><span class="sxs-lookup"><span data-stu-id="7c216-125">Just as a script is composed of one or more commands, a workflow is composed of one or more activities that are carried out in a sequence.</span></span> <span data-ttu-id="7c216-126">Windows PowerShell-arbetsflöde konverterar automatiskt många av Windows PowerShell-cmdlets för aktiviteter när ett arbetsflöde körs.</span><span class="sxs-lookup"><span data-stu-id="7c216-126">Windows PowerShell Workflow automatically converts many of the Windows PowerShell cmdlets to activities when it runs a workflow.</span></span> <span data-ttu-id="7c216-127">När du anger en av dessa cmdletar i din runbook, körs motsvarande aktiviteten i Windows Workflow Foundation.</span><span class="sxs-lookup"><span data-stu-id="7c216-127">When you specify one of these cmdlets in your runbook, the corresponding activity is run by Windows Workflow Foundation.</span></span> <span data-ttu-id="7c216-128">För dessa cmdlets utan en motsvarande aktivitet körs Windows PowerShell-arbetsflöde automatiskt cmdleten i en [InlineScript](#inlinescript) aktivitet.</span><span class="sxs-lookup"><span data-stu-id="7c216-128">For those cmdlets without a corresponding activity, Windows PowerShell Workflow automatically runs the cmdlet within an [InlineScript](#inlinescript) activity.</span></span> <span data-ttu-id="7c216-129">Det finns en uppsättning cmdlets som är undantagna och inte kan användas i ett arbetsflöde om du uttryckligen lägga till dem i ett InlineScript-block.</span><span class="sxs-lookup"><span data-stu-id="7c216-129">There is a set of cmdlets that are excluded and cannot be used in a workflow unless you explicitly include them in an InlineScript block.</span></span> <span data-ttu-id="7c216-130">Mer information om dessa koncept finns [med hjälp av aktiviteter i Skriptarbetsflöden](http://technet.microsoft.com/library/jj574194.aspx).</span><span class="sxs-lookup"><span data-stu-id="7c216-130">For further details on these concepts, see [Using Activities in Script Workflows](http://technet.microsoft.com/library/jj574194.aspx).</span></span>

<span data-ttu-id="7c216-131">Arbetsflödesaktiviteter delar en uppsättning gemensamma parametrar för att konfigurera driften.</span><span class="sxs-lookup"><span data-stu-id="7c216-131">Workflow activities share a set of common parameters to configure their operation.</span></span> <span data-ttu-id="7c216-132">Mer information om arbetsflödets gemensamma parametrar finns [about_WorkflowCommonParameters](http://technet.microsoft.com/library/jj129719.aspx).</span><span class="sxs-lookup"><span data-stu-id="7c216-132">For details about the workflow common parameters, see [about_WorkflowCommonParameters](http://technet.microsoft.com/library/jj129719.aspx).</span></span>

### <a name="positional-parameters"></a><span data-ttu-id="7c216-133">Namngivna parametrar</span><span class="sxs-lookup"><span data-stu-id="7c216-133">Positional parameters</span></span>
<span data-ttu-id="7c216-134">Du kan använda namngivna parametrar med aktiviteter och cmdlets i ett arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="7c216-134">You can't use positional parameters with activities and cmdlets in a workflow.</span></span>  <span data-ttu-id="7c216-135">Det innebär är att du måste använda parameternamn.</span><span class="sxs-lookup"><span data-stu-id="7c216-135">All this means is that you must use parameter names.</span></span>

<span data-ttu-id="7c216-136">Tänk dig följande kod som hämtar alla tjänster som körs.</span><span class="sxs-lookup"><span data-stu-id="7c216-136">For example, consider the following code that gets all running services.</span></span>

     Get-Service | Where-Object {$_.Status -eq "Running"}

<span data-ttu-id="7c216-137">Om du försöker köra samma kod i ett arbetsflöde, får du ett meddelande som ”parameteruppsättningen inte kan matchas med de tillhandahållna namngivna parametrarna”.</span><span class="sxs-lookup"><span data-stu-id="7c216-137">If you try to run this same code in a workflow, you receive a message like "Parameter set cannot be resolved using the specified named parameters."</span></span>  <span data-ttu-id="7c216-138">Åtgärda detta genom att ange parameternamnet enligt följande.</span><span class="sxs-lookup"><span data-stu-id="7c216-138">To correct this, provide the parameter name as in the following.</span></span>

    Workflow Get-RunningServices
    {
        Get-Service | Where-Object -FilterScript {$_.Status -eq "Running"}
    }

### <a name="deserialized-objects"></a><span data-ttu-id="7c216-139">Avserialiserat objekt</span><span class="sxs-lookup"><span data-stu-id="7c216-139">Deserialized objects</span></span>
<span data-ttu-id="7c216-140">Objekt i arbetsflöden är avserialiseras.</span><span class="sxs-lookup"><span data-stu-id="7c216-140">Objects in workflows are deserialized.</span></span>  <span data-ttu-id="7c216-141">Det innebär att deras egenskaper finns kvar, men inte deras metoder.</span><span class="sxs-lookup"><span data-stu-id="7c216-141">This means that their properties are still available, but not their methods.</span></span>  <span data-ttu-id="7c216-142">Tänk dig följande PowerShell-kod som stoppar en tjänst med metoden stopp för Service-objektet.</span><span class="sxs-lookup"><span data-stu-id="7c216-142">For example, consider the following PowerShell code that stops a service using the Stop method of the Service object.</span></span>

    $Service = Get-Service -Name MyService
    $Service.Stop()

<span data-ttu-id="7c216-143">Om du försöker köra det i ett arbetsflöde, får du ett felmeddelande som säger ”metodanropet inte stöds i en Windows PowerShell-arbetsflöde”.</span><span class="sxs-lookup"><span data-stu-id="7c216-143">If you try to run this in a workflow, you receive an error saying "Method invocation is not supported in a Windows PowerShell Workflow."</span></span>  

<span data-ttu-id="7c216-144">Ett alternativ är att omsluta dessa två rader med kod i en [InlineScript](#inlinescript) blockera i vilket fall $Service skulle vara en serviceobjektet i blocket.</span><span class="sxs-lookup"><span data-stu-id="7c216-144">One option is to wrap these two lines of code in an [InlineScript](#inlinescript) block in which case $Service would be a service object within the block.</span></span>

    Workflow Stop-Service
    {
        InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
        }
    }

<span data-ttu-id="7c216-145">Ett annat alternativ är att använda en annan cmdlet som utför samma funktion som metod, om det inte finns.</span><span class="sxs-lookup"><span data-stu-id="7c216-145">Another option is to use another cmdlet that performs the same functionality as the method, if one is available.</span></span>  <span data-ttu-id="7c216-146">I vårt exempel cmdlet Stop-Service fungerar på samma sätt som metod för att stoppa och du kan använda följande för ett arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="7c216-146">In our sample, the Stop-Service cmdlet provides the same functionality as the Stop method, and you could use the following for a workflow.</span></span>

    Workflow Stop-MyService
    {
        $Service = Get-Service -Name MyService
        Stop-Service -Name $Service.Name
    }


## <a name="inlinescript"></a><span data-ttu-id="7c216-147">InlineScript</span><span class="sxs-lookup"><span data-stu-id="7c216-147">InlineScript</span></span>
<span data-ttu-id="7c216-148">Den **InlineScript** aktivitet är användbart när du behöver köra ett eller flera kommandon som traditionella PowerShell-skript i stället för PowerShell-arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="7c216-148">The **InlineScript** activity is useful when you need to run one or more commands as traditional PowerShell script instead of PowerShell workflow.</span></span>  <span data-ttu-id="7c216-149">Medan kommandon i ett arbetsflöde skickas till Windows Workflow Foundation för bearbetning, bearbetas kommandona i ett InlineScript-block av Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7c216-149">While commands in a workflow are sent to Windows Workflow Foundation for processing, commands in an InlineScript block are processed by Windows PowerShell.</span></span>

<span data-ttu-id="7c216-150">InlineScript använder du följande syntax som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="7c216-150">InlineScript uses the following syntax shown below.</span></span>

    InlineScript
    {
      <Script Block>
    } <Common Parameters>

<span data-ttu-id="7c216-151">Du kan returnera utdata från en InlineScript genom att tilldela en variabel utdata.</span><span class="sxs-lookup"><span data-stu-id="7c216-151">You can return output from an InlineScript by assigning the output to a variable.</span></span> <span data-ttu-id="7c216-152">I följande exempel stoppar en tjänst och matar ut tjänstnamnet.</span><span class="sxs-lookup"><span data-stu-id="7c216-152">The following example stops a service and then outputs the service name.</span></span>

    Workflow Stop-MyService
    {
        $Output = InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


<span data-ttu-id="7c216-153">Du kan skicka värden i ett InlineScript-block, men du måste använda **$Using** omfångsmodifieraren.</span><span class="sxs-lookup"><span data-stu-id="7c216-153">You can pass values into an InlineScript block, but you must use **$Using** scope modifier.</span></span>  <span data-ttu-id="7c216-154">I följande exempel är identiskt med föregående exempel förutom att namnet på tjänsten som tillhandahålls av en variabel.</span><span class="sxs-lookup"><span data-stu-id="7c216-154">The following example is identical to the previous example except that the service name is provided by a variable.</span></span>

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


<span data-ttu-id="7c216-155">InlineScript-aktiviteter kan vara avgörande i vissa arbetsflöden, stöder inte arbetsflöde för konstruktionerna och bör endast användas när det är nödvändigt av följande skäl:</span><span class="sxs-lookup"><span data-stu-id="7c216-155">While InlineScript activities may be critical in certain workflows, they do not support workflow constructs and should only be used when necessary for the following reasons:</span></span>

* <span data-ttu-id="7c216-156">Du kan inte använda [kontrollpunkter](#checkpoints) i ett InlineScript-block.</span><span class="sxs-lookup"><span data-stu-id="7c216-156">You cannot use [checkpoints](#checkpoints) inside an InlineScript block.</span></span> <span data-ttu-id="7c216-157">Om ett fel uppstår i kodblocket, måste det köras från början av blocket.</span><span class="sxs-lookup"><span data-stu-id="7c216-157">If a failure occurs within the block, it must be resumed from the beginning of the block.</span></span>
* <span data-ttu-id="7c216-158">Du kan inte använda [parallell körning](#parallel-processing) inuti en InlineScriptBlock.</span><span class="sxs-lookup"><span data-stu-id="7c216-158">You cannot use [parallel execution](#parallel-processing) inside an InlineScriptBlock.</span></span>
* <span data-ttu-id="7c216-159">InlineScript påverkar skalbarhet av arbetsflödet eftersom den innehåller Windows PowerShell-sessionen för InlineScript-blockets hela längd.</span><span class="sxs-lookup"><span data-stu-id="7c216-159">InlineScript affects scalability of the workflow since it holds the Windows PowerShell session for the entire length of the InlineScript block.</span></span>

<span data-ttu-id="7c216-160">Läs mer om hur du använder InlineScript [kör du Windows PowerShell-kommandon i ett arbetsflöde](http://technet.microsoft.com/library/jj574197.aspx) och [about_InlineScript](http://technet.microsoft.com/library/jj649082.aspx).</span><span class="sxs-lookup"><span data-stu-id="7c216-160">For more information on using InlineScript, see [Running Windows PowerShell Commands in a Workflow](http://technet.microsoft.com/library/jj574197.aspx) and [about_InlineScript](http://technet.microsoft.com/library/jj649082.aspx).</span></span>

## <a name="parallel-processing"></a><span data-ttu-id="7c216-161">Parallell bearbetning</span><span class="sxs-lookup"><span data-stu-id="7c216-161">Parallel processing</span></span>
<span data-ttu-id="7c216-162">En fördel med Windows PowerShell-arbetsflöden är möjligheten att utföra en uppsättning kommandon parallellt i stället för sekventiellt som i ett vanligt skript.</span><span class="sxs-lookup"><span data-stu-id="7c216-162">One advantage of Windows PowerShell Workflows is the ability to perform a set of commands in parallel instead of sequentially as with a typical script.</span></span>

<span data-ttu-id="7c216-163">Du kan använda den **parallella** nyckelord för att skapa ett skriptblock med flera kommandon som körs samtidigt.</span><span class="sxs-lookup"><span data-stu-id="7c216-163">You can use the **Parallel** keyword to create a script block with multiple commands that run concurrently.</span></span> <span data-ttu-id="7c216-164">Detta använder du följande syntax som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="7c216-164">This uses the following syntax shown below.</span></span> <span data-ttu-id="7c216-165">I det här fallet startar Activity1 och Activity2 på samma gång.</span><span class="sxs-lookup"><span data-stu-id="7c216-165">In this case, Activity1 and Activity2 starts at the same time.</span></span> <span data-ttu-id="7c216-166">Activity3 startas endast efter att både Activity1 och Activity2 har slutförts.</span><span class="sxs-lookup"><span data-stu-id="7c216-166">Activity3 starts only after both Activity1 and Activity2 have completed.</span></span>

    Parallel
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>


<span data-ttu-id="7c216-167">Tänk dig följande PowerShell-kommandon som kopiera flera filer till en plats i nätverket.</span><span class="sxs-lookup"><span data-stu-id="7c216-167">For example, consider the following PowerShell commands that copy multiple files to a network destination.</span></span>  <span data-ttu-id="7c216-168">Dessa kommandon körs sekventiellt så att en fil måste avslutas innan nästa startas har kopierats.</span><span class="sxs-lookup"><span data-stu-id="7c216-168">These commands are run sequentially so that one file must finish copying before the next is started.</span></span>     

    Copy-Item -Path C:\LocalPath\File1.txt -Destination \\NetworkPath\File1.txt
    Copy-Item -Path C:\LocalPath\File2.txt -Destination \\NetworkPath\File2.txt
    Copy-Item -Path C:\LocalPath\File3.txt -Destination \\NetworkPath\File3.txt

<span data-ttu-id="7c216-169">Följande arbetsflöde kör samma kommandon parallellt så att alla börja kopiera på samma gång.</span><span class="sxs-lookup"><span data-stu-id="7c216-169">The following workflow runs these same commands in parallel so that they all start copying at the same time.</span></span>  <span data-ttu-id="7c216-170">Endast när de är alla visas kopieras slutförande meddelandet.</span><span class="sxs-lookup"><span data-stu-id="7c216-170">Only after they are all copied is the completion message displayed.</span></span>

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


<span data-ttu-id="7c216-171">Du kan använda den **ForEach-Parallel** konstruktion kan kommandon bearbetas för varje objekt i en samling samtidigt.</span><span class="sxs-lookup"><span data-stu-id="7c216-171">You can use the **ForEach -Parallel** construct to process commands for each item in a collection concurrently.</span></span> <span data-ttu-id="7c216-172">Objekt i samlingen bearbetas parallellt medan kommandona i skriptblocket körs sekventiellt.</span><span class="sxs-lookup"><span data-stu-id="7c216-172">The items in the collection are processed in parallel while the commands in the script block run sequentially.</span></span> <span data-ttu-id="7c216-173">Detta använder du följande syntax som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="7c216-173">This uses the following syntax shown below.</span></span> <span data-ttu-id="7c216-174">I det här fallet startar Activity1 samtidigt för alla objekt i samlingen.</span><span class="sxs-lookup"><span data-stu-id="7c216-174">In this case, Activity1 starts at the same time for all items in the collection.</span></span> <span data-ttu-id="7c216-175">För varje objekt startar Activity2 efter att Activity1 har slutförts.</span><span class="sxs-lookup"><span data-stu-id="7c216-175">For each item, Activity2 starts after Activity1 is complete.</span></span> <span data-ttu-id="7c216-176">Activity3 startas endast efter att både Activity1 och Activity2 har slutförts för alla objekt.</span><span class="sxs-lookup"><span data-stu-id="7c216-176">Activity3 starts only after both Activity1 and Activity2 have completed for all items.</span></span>

    ForEach -Parallel ($<item> in $<collection>)
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>

<span data-ttu-id="7c216-177">I följande exempel liknar föregående exempel filkopiering parallellt.</span><span class="sxs-lookup"><span data-stu-id="7c216-177">The following example is similar to the previous example copying files in parallel.</span></span>  <span data-ttu-id="7c216-178">I det här fallet visas ett meddelande för varje fil när den kopierar.</span><span class="sxs-lookup"><span data-stu-id="7c216-178">In this case, a message is displayed for each file after it copies.</span></span>  <span data-ttu-id="7c216-179">Endast när de är alla visas kopierats slutliga slutförande meddelandet.</span><span class="sxs-lookup"><span data-stu-id="7c216-179">Only after they are all completely copied is the final completion message displayed.</span></span>

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
> <span data-ttu-id="7c216-180">Vi rekommenderar inte underordnade runbooks som körs parallellt eftersom det har visat sig ge osäkra resultat.</span><span class="sxs-lookup"><span data-stu-id="7c216-180">We do not recommend running child runbooks in parallel since this has been shown to give unreliable results.</span></span>  <span data-ttu-id="7c216-181">Utdata från den underordnade runbooken ibland visas inte och inställningar i en underordnad runbook kan påverka andra parallella underordnade runbooks</span><span class="sxs-lookup"><span data-stu-id="7c216-181">The output from the child runbook sometimes does not show up, and settings in one child runbook can affect the other parallel child runbooks</span></span>
>

## <a name="checkpoints"></a><span data-ttu-id="7c216-182">Kontrollpunkter</span><span class="sxs-lookup"><span data-stu-id="7c216-182">Checkpoints</span></span>
<span data-ttu-id="7c216-183">En *kontrollpunkt* är en ögonblicksbild av det aktuella tillståndet för arbetsflödet som innehåller det aktuella värdet för variabler och all utdata som genererats till den punkten.</span><span class="sxs-lookup"><span data-stu-id="7c216-183">A *checkpoint* is a snapshot of the current state of the workflow that includes the current value for variables and any output generated to that point.</span></span> <span data-ttu-id="7c216-184">Om ett arbetsflöde slutar i fel eller har pausats, sedan startar nästa gång den körs den från den senaste kontrollpunkten i stället för i början av worfklow.</span><span class="sxs-lookup"><span data-stu-id="7c216-184">If a workflow ends in error or is suspended, then the next time it is run it will start from its last checkpoint instead of the start of the worfklow.</span></span>  <span data-ttu-id="7c216-185">Du kan ange en kontrollpunkt i ett arbetsflöde med den **Checkpoint-Workflow** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="7c216-185">You can set a checkpoint in a workflow with the **Checkpoint-Workflow** activity.</span></span>

<span data-ttu-id="7c216-186">I följande exempelkod ett undantag som inträffar efter Activity2 orsakar arbetsflödet ska sluta.</span><span class="sxs-lookup"><span data-stu-id="7c216-186">In the following sample code, an exception occurs after Activity2 causing the workflow to end.</span></span> <span data-ttu-id="7c216-187">När arbetsflödet körs igen, startar det genom att köra Activity2 eftersom den bara när den senast lagrade kontrollpunkten.</span><span class="sxs-lookup"><span data-stu-id="7c216-187">When the workflow is run again, it starts by running Activity2 since this was just after the last checkpoint set.</span></span>

    <Activity1>
    Checkpoint-Workflow
    <Activity2>
    <Exception>
    <Activity3>

<span data-ttu-id="7c216-188">Du bör lagra kontrollpunkter i ett arbetsflöde efter aktiviteter som kan vara utsatt för undantag och inte ska upprepas om arbetsflödet återupptas.</span><span class="sxs-lookup"><span data-stu-id="7c216-188">You should set checkpoints in a workflow after activities that may be prone to exception and should not be repeated if the workflow is resumed.</span></span> <span data-ttu-id="7c216-189">Arbetsflödet kan till exempel skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="7c216-189">For example, your workflow may create a virtual machine.</span></span> <span data-ttu-id="7c216-190">Du kan ange en kontrollpunkt både före och efter kommandona för att skapa den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="7c216-190">You could set a checkpoint both before and after the commands to create the virtual machine.</span></span> <span data-ttu-id="7c216-191">Om misslyckas, skulle sedan kommandona upprepas om arbetsflödet startas igen.</span><span class="sxs-lookup"><span data-stu-id="7c216-191">If the creation fails, then the commands would be repeated if the workflow is started again.</span></span> <span data-ttu-id="7c216-192">Om worfklow misslyckas efter skapandet lyckas, sedan skapas den virtuella datorn inte igen när arbetsflödet återupptas.</span><span class="sxs-lookup"><span data-stu-id="7c216-192">If the worfklow fails after the creation succeeds, then the virtual machine will not be created again when the workflow is resumed.</span></span>

<span data-ttu-id="7c216-193">I följande exempel kopierar flera filer till en nätverksplats och anger en kontrollpunkt efter varje fil.</span><span class="sxs-lookup"><span data-stu-id="7c216-193">The following example copies multiple files to a network location and sets a checkpoint after each file.</span></span>  <span data-ttu-id="7c216-194">Om nätverksplatsen tappas bort, slutar arbetsflödet i fel.</span><span class="sxs-lookup"><span data-stu-id="7c216-194">If the network location is lost, then the workflow ends in error.</span></span>  <span data-ttu-id="7c216-195">När den startas igen fortsätter den med den senaste kontrollpunkten, vilket innebär att endast de filer som redan har kopierats hoppas över.</span><span class="sxs-lookup"><span data-stu-id="7c216-195">When it is started again, it will resume at the last checkpoint meaning that only the files that have already been copied are skipped.</span></span>

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

<span data-ttu-id="7c216-196">Eftersom användarnamn autentiseringsuppgifter inte sparas när du anropar den [Suspend-Workflow](https://technet.microsoft.com/library/jj733586.aspx) aktivitet eller efter den senaste kontrollpunkten, måste du ange autentiseringsuppgifterna som null och sedan hämta dem igen från arkivet tillgången efter  **Pausa arbetsflödet** eller kontrollpunkt anropades.</span><span class="sxs-lookup"><span data-stu-id="7c216-196">Because username credentials are not persisted after you call the [Suspend-Workflow](https://technet.microsoft.com/library/jj733586.aspx) activity or after the last checkpoint, you need to set the credentials to null and then retrieve them again from the asset store after **Suspend-Workflow** or checkpoint is called.</span></span>  <span data-ttu-id="7c216-197">Annars får du följande felmeddelande: *går inte att återuppta arbetsflödesjobbet, antingen eftersom beständiga data inte kunde spara helt eller spara beständiga data har skadats. Du måste starta om arbetsflödet.*</span><span class="sxs-lookup"><span data-stu-id="7c216-197">Otherwise, you may receive the following error message: *The workflow job cannot be resumed, either because persistence data could not be saved completely, or saved persistence data has been corrupted. You must restart the workflow.*</span></span>

<span data-ttu-id="7c216-198">Samma följande kod visar hur du hanterar detta i PowerShell-arbetsflöde runbooks.</span><span class="sxs-lookup"><span data-stu-id="7c216-198">The following same code demonstrates how to handle this in your PowerShell Workflow runbooks.</span></span>

    workflow CreateTestVms
    {
       $Cred = Get-AzureAutomationCredential -Name "MyCredential"
       $null = Add-AzureRmAccount -Credential $Cred

       $VmsToCreate = Get-AzureAutomationVariable -Name "VmsToCreate"

       foreach ($VmName in $VmsToCreate)
         {
          # Do work first to create the VM (code not shown)

          # Now add the VM
          New-AzureRmVm -VM $Vm -Location "WestUs" -ResourceGroupName "ResourceGroup01"

          # Checkpoint so that VM creation is not repeated if workflow suspends
          $Cred = $null
          Checkpoint-Workflow
          $Cred = Get-AzureAutomationCredential -Name "MyCredential"
          $null = Add-AzureRmAccount -Credential $Cred
         }
     }


<span data-ttu-id="7c216-199">Detta är inte nödvändigt om du autentiserar med hjälp av en Kör som-konto konfigureras med en tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="7c216-199">This is not required if you are authenticating using a Run As account configured with a service principal.</span></span>  

<span data-ttu-id="7c216-200">Mer information om kontrollpunkter finns [att lägga till kontrollpunkter till ett arbetsflöde för skript](http://technet.microsoft.com/library/jj574114.aspx).</span><span class="sxs-lookup"><span data-stu-id="7c216-200">For more information about checkpoints, see [Adding Checkpoints to a Script Workflow](http://technet.microsoft.com/library/jj574114.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7c216-201">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7c216-201">Next steps</span></span>
* <span data-ttu-id="7c216-202">Se hur du kommer igång med runbooks baserade på PowerShell-arbetsflöden i [Min första PowerShell-arbetsflödesbaserade runbook](automation-first-runbook-textual.md)</span><span class="sxs-lookup"><span data-stu-id="7c216-202">To get started with PowerShell workflow runbooks, see [My first PowerShell workflow runbook](automation-first-runbook-textual.md)</span></span>
