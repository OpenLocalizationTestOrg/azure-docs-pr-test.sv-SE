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
# <a name="learning-key-windows-powershell-workflow-concepts-for-automation-runbooks"></a>Learning nyckelbegrepp i Windows PowerShell-arbetsflöde för Automation-runbooks 
Azure Automation-Runbooks implementeras som Windows PowerShell-arbetsflöden.  En Windows PowerShell-arbetsflöde är liknande tooa Windows PowerShell-skript men vissa viktiga skillnader som kan vara förvirrande tooa ny användare.  Den här artikeln är avsedd toohelp du skriver runbooks med PowerShell-arbetsflöde, rekommenderar vi att du skriver runbooks med PowerShell om du inte behöver kontrollpunkter.  Det finns flera syntax skillnader vid redigering av runbooks med PowerShell-arbetsflöde och skillnaderna kräver lite mer arbete toowrite effektiva arbetsflöden.  

Ett arbetsflöde är en sekvens med programmerade, nätverksanslutna steg som utför tidskrävande uppgifter eller kräver hello samordning av flera steg i flera enheter eller hanterade noder. hello fördelarna med ett arbetsflöde jämfört med ett vanligt skript inkluderar hello möjlighet toosimultaneously utföra en åtgärd mot flera enheter och hello möjlighet tooautomatically återställning vid fel. En Windows PowerShell-arbetsflöde är ett Windows PowerShell-skript som använder Windows Workflow Foundation. Även om hello arbetsflödet skrivs med Windows PowerShell-syntax och startas av Windows PowerShell, bearbetas men det av Windows Workflow Foundation.

Fullständig information om hello avsnitt i den här artikeln finns [komma igång med Windows PowerShell-arbetsflöde](http://technet.microsoft.com/library/jj134242.aspx).

## <a name="basic-structure-of-a-workflow"></a>Grundläggande struktur för ett arbetsflöde
hello första steg tooconverting ett PowerShell-skript tooa PowerShell-arbetsflöde omsluta den med hello **arbetsflöde** nyckelord.  Ett arbetsflöde som startar med hello **arbetsflöde** följt av hello brödtext hello skriptkoden omgiven av klammerparenteser. hello namnet på arbetsflödet hello följer hello **arbetsflöde** nyckelordet enligt hello följande syntax:

    Workflow Test-Workflow
    {
       <Commands>
    }

hello namnet på hello arbetsflödet måste matcha hello namnet i hello Automation-runbook. Om hello runbook importeras sedan hello filename måste matcha hello Arbetsflödesnamn och måste sluta med *.ps1*.

tooadd parametrar toohello arbetsflöde, Använd hello **Param** nyckelordet samma sätt som tooa skript.

## <a name="code-changes"></a>Kodändringar
PowerShell-arbetsflöde kod verkar nästan samma tooPowerShell skriptkod förutom några betydande förändringar.  hello följande avsnitt beskrivs de ändringar som du behöver toomake tooa PowerShell-skript för den toorun i ett arbetsflöde.

### <a name="activities"></a>Aktiviteter
En aktivitet är en viss aktivitet i ett arbetsflöde. Precis som ett skript består av ett eller flera kommandon, består ett arbetsflöde av en eller flera aktiviteter som utförs i en sekvens. Windows PowerShell-arbetsflöde konverterar automatiskt många av hello tooactivities för Windows PowerShell-cmdlets när ett arbetsflöde körs. När du anger en av dessa cmdletar i din runbook, körs hello motsvarande aktivitet i Windows Workflow Foundation. För dessa cmdlets utan en motsvarande aktivitet körs Windows PowerShell-arbetsflöde automatiskt hello cmdlet inom en [InlineScript](#inlinescript) aktivitet. Det finns en uppsättning cmdlets som är undantagna och inte kan användas i ett arbetsflöde om du uttryckligen lägga till dem i ett InlineScript-block. Mer information om dessa koncept finns [med hjälp av aktiviteter i Skriptarbetsflöden](http://technet.microsoft.com/library/jj574194.aspx).

Arbetsflödesaktiviteter delar en uppsättning gemensamma parametrar tooconfigure driften. Mer information om gemensamma arbetsflödesparametrar hello finns [about_WorkflowCommonParameters](http://technet.microsoft.com/library/jj129719.aspx).

### <a name="positional-parameters"></a>Namngivna parametrar
Du kan använda namngivna parametrar med aktiviteter och cmdlets i ett arbetsflöde.  Det innebär är att du måste använda parameternamn.

Anta till exempel att hello efter koden som hämtar alla tjänster som körs.

     Get-Service | Where-Object {$_.Status -eq "Running"}

Om du försöker toorun samma kod i ett arbetsflöde, meddelande ett som ”parametern set inte kan lösas med hello angetts namngivna parametrar”.  toocorrect, ange hello parameternamn som hello följande.

    Workflow Get-RunningServices
    {
        Get-Service | Where-Object -FilterScript {$_.Status -eq "Running"}
    }

### <a name="deserialized-objects"></a>Avserialiserat objekt
Objekt i arbetsflöden är avserialiseras.  Det innebär att deras egenskaper finns kvar, men inte deras metoder.  Tänk dig följande PowerShell-koden som stoppar en tjänst med hello stoppmetoden av hello webbtjänstobjektet hello.

    $Service = Get-Service -Name MyService
    $Service.Stop()

Om du försöker toorun detta i ett arbetsflöde, får du ett felmeddelande som säger ”metodanropet inte stöds i en Windows PowerShell-arbetsflöde”.  

Ett alternativ är toowrap dessa två rader med kod i en [InlineScript](#inlinescript) blockera i vilket fall $Service skulle vara en serviceobjektet inom hello-block.

    Workflow Stop-Service
    {
        InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
        }
    }

Ett annat alternativ är toouse en annan cmdlet som utför hello samma funktioner som hello-metoden, om det inte finns.  I vårt exempel hello Stop-Service cmdlet ger hello samma funktioner som hello stoppmetoden, och du kan använda följande hello för ett arbetsflöde.

    Workflow Stop-MyService
    {
        $Service = Get-Service -Name MyService
        Stop-Service -Name $Service.Name
    }


## <a name="inlinescript"></a>InlineScript
Hej **InlineScript** aktivitet är användbart när du behöver toorun ett eller flera kommandon som traditionella PowerShell-skript i stället för PowerShell-arbetsflöde.  Medan kommandon i ett arbetsflöde skickas tooWindows Workflow Foundation för bearbetning, bearbetas kommandona i ett InlineScript-block av Windows PowerShell.

InlineScript använder hello följande syntax som visas nedan.

    InlineScript
    {
      <Script Block>
    } <Common Parameters>

Du kan returnera utdata från en InlineScript genom att tilldela hello utdata tooa variabeln. hello följande exempel stoppar en tjänst och matar ut hello tjänstnamn.

    Workflow Stop-MyService
    {
        $Output = InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


Du kan skicka värden i ett InlineScript-block, men du måste använda **$Using** omfångsmodifieraren.  hello följande exempel är identiska toohello föregående exempel förutom att hello tjänstnamn tillhandahålls av en variabel.

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


InlineScript-aktiviteter kan vara avgörande i vissa arbetsflöden, stöder inte arbetsflöde för konstruktionerna och bör endast användas när det behövs för hello följande orsaker:

* Du kan inte använda [kontrollpunkter](#checkpoints) i ett InlineScript-block. Om ett fel inträffar inom hello block, måste det köras från hello början av hello-block.
* Du kan inte använda [parallell körning](#parallel-processing) inuti en InlineScriptBlock.
* InlineScript påverkar skalbarhet hello arbetsflödet eftersom den innehåller hello Windows PowerShell-session för hello hello InlineScript-blockets hela längd.

Läs mer om hur du använder InlineScript [kör du Windows PowerShell-kommandon i ett arbetsflöde](http://technet.microsoft.com/library/jj574197.aspx) och [about_InlineScript](http://technet.microsoft.com/library/jj649082.aspx).

## <a name="parallel-processing"></a>Parallell bearbetning
En fördel med Windows PowerShell-arbetsflöden är hello möjlighet tooperform en uppsättning kommandon parallellt i stället för sekventiellt som i ett vanligt skript.

Du kan använda hello **parallella** nyckelordet toocreate ett skriptblock med flera kommandon som körs samtidigt. Här används hello följande syntax som visas nedan. I det här fallet Activity1 och Activity2 börjar vid hello samtidigt. Activity3 startas endast efter att både Activity1 och Activity2 har slutförts.

    Parallel
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>


Anta till exempel att hello följande PowerShell-kommandon som kopierar flera filer tooa nätverk mål.  Dessa kommandon körs sekventiellt så att en fil måste avslutas kopierats innan hello nästa gång.     

    Copy-Item -Path C:\LocalPath\File1.txt -Destination \\NetworkPath\File1.txt
    Copy-Item -Path C:\LocalPath\File2.txt -Destination \\NetworkPath\File2.txt
    Copy-Item -Path C:\LocalPath\File3.txt -Destination \\NetworkPath\File3.txt

hello följande arbetsflöde kör dessa samma kommandon parallellt så att alla börja kopiera på hello samma tid.  Endast när de är alla visas kopieras slutförande hälsningsmeddelande.

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


Du kan använda hello **ForEach-Parallel** konstruera tooprocess kommandon för varje objekt i en samling samtidigt. hello objekten i hello samlingen bearbetas parallellt medan hello kommandona i hello-skriptblocket körs sekventiellt. Här används hello följande syntax som visas nedan. I det här fallet Activity1 startar enligt hello samma tid för alla objekt i hello samling. För varje objekt startar Activity2 efter att Activity1 har slutförts. Activity3 startas endast efter att både Activity1 och Activity2 har slutförts för alla objekt.

    ForEach -Parallel ($<item> in $<collection>)
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>

följande exempel hello är liknande toohello-föregående exempel filkopiering parallellt.  I det här fallet visas ett meddelande för varje fil när den kopierar.  Endast när de är alla visas kopierats slutliga slutförande hälsningsmeddelande.

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
> Vi rekommenderar inte underordnade runbooks som körs parallellt eftersom det har visats toogive osäkra resultat.  hello utdata från hello underordnad runbook som ibland visas inte och inställningar i en underordnad runbook kan påverka hello andra parallella underordnade runbooks
>

## <a name="checkpoints"></a>Kontrollpunkter
En *kontrollpunkt* är en ögonblicksbild av hello aktuell status för hello arbetsflöde som inkluderar hello aktuella värdet för variabler och eventuella utdata genererade toothat punkt. Om ett arbetsflöde slutar i fel eller har pausats, sedan startar hello nästa gång den körs den från den senaste kontrollpunkten i stället för hello början av hello worfklow.  Du kan ange en kontrollpunkt i ett arbetsflöde med hello **Checkpoint-Workflow** aktivitet.

I följande exempelkod hello, inträffar ett undantag efter Activity2 orsakar hello arbetsflöde tooend. När hello arbetsflödet körs igen, startar det genom att köra Activity2 eftersom den bara när hello senast lagrade kontrollpunkten.

    <Activity1>
    Checkpoint-Workflow
    <Activity2>
    <Exception>
    <Activity3>

Du bör lagra kontrollpunkter i ett arbetsflöde efter aktiviteter som kan vara felbenägna tooexception och får inte vara upprepas om hello arbetsflödet återupptas. Arbetsflödet kan till exempel skapa en virtuell dator. Du kan ange en kontrollpunkt både före och efter hello kommandon toocreate hello virtuell dator. Om hello misslyckas, skulle sedan hello kommandon upprepas om hello arbetsflödet startas igen. Om hello worfklow misslyckas när hello skapandet lyckas sedan skapas hello virtuella datorn inte igen när hello arbetsflödet återupptas.

hello följande exempel kopierar flera filer tooa nätverksplats och anger en kontrollpunkt efter varje fil.  Om hello nätverksplats tappas bort, slutar hello arbetsflödet i fel.  När den startas igen fortsätter den på hello senaste kontrollpunkten, vilket innebär att endast hello-filer som redan har kopierats hoppas över.

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

Eftersom användarnamn autentiseringsuppgifter inte sparas när du anropar hello [Suspend-Workflow](https://technet.microsoft.com/library/jj733586.aspx) aktivitet eller efter hello senaste kontrollpunkten, du behöver tooset hello autentiseringsuppgifter toonull och sedan hämta dem igen från hello tillgången store efter  **Pausa arbetsflödet** eller kontrollpunkt anropades.  I annat fall visas följande felmeddelande hello: *går inte att återuppta arbetsflödesjobbet hello, antingen eftersom beständiga data inte kunde spara helt eller spara beständiga data har skadats. Du måste starta om hello arbetsflöde.*

hello följa samma kod visar hur toohandle detta i dina runbooks med PowerShell-arbetsflöde.

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


Detta är inte nödvändigt om du autentiserar med hjälp av en Kör som-konto konfigureras med en tjänstens huvudnamn.  

Mer information om kontrollpunkter finns [att lägga till kontrollpunkter tooa arbetsflöde för skript](http://technet.microsoft.com/library/jj574114.aspx).

## <a name="next-steps"></a>Nästa steg
* tooget igång med PowerShell arbetsflöde runbooks finns [min första PowerShell-arbetsflödesrunbook](automation-first-runbook-textual.md)
