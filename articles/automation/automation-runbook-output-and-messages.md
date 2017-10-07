---
title: aaaRunbook utdata och meddelanden i Azure Automation | Microsoft Docs
description: "Beskriver hur toocreate och hämta utdata och fel meddelanden från runbooks i Azure Automation."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 13a414f5-0e2c-4be2-9558-a3e3ec84b6b2
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/11/2016
ms.author: magoedte;bwren
ms.openlocfilehash: c1505fa889473766bfa47e43aaed2449d60ad851
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="runbook-output-and-messages-in-azure-automation"></a>Runbook-utdata och meddelanden i Azure Automation
De flesta Azure Automation-runbooks har någon form av utdata, till exempel en fel meddelande toohello användare eller ett komplext objekt avsedda toobe som används av ett annat arbetsflöde. Windows PowerShell innehåller [flera strömmar](http://blogs.technet.com/heyscriptingguy/archive/2014/03/30/understanding-streams-redirection-and-write-host-in-powershell.aspx) toosend utdata från ett skript eller ett arbetsflöde. Azure Automation fungerar olika med var och en av dessa strömmar och du bör följa bästa praxis för hur toouse varje när du skapar en runbook.

hello följande tabell innehåller en kort beskrivning av varje hello dataströmmar och deras beteende i hello Azure-hanteringsportalen både när du kör en publicerad runbook och när [testar en runbook](automation-testing-runbook.md). Mer information om varje dataström finns i följande avsnitt.

| Dataströmmen | Beskrivning | Publicerad | Testa |
|:--- |:--- |:--- |:--- |
| Resultat |Objekt avsedd toobe som används av andra runbooks. |Skrivs toohello jobbhistorik. |Visas i hello rutan Testutdata. |
| Varning |Varningsmeddelande avsett för hello användare. |Skrivs toohello jobbhistorik. |Visas i hello rutan Testutdata. |
| Fel |Felmeddelande avsett för hello användare. Till skillnad från ett undantag fortsätter runbooken hello när ett felmeddelande visas som standard. |Skrivs toohello jobbhistorik. |Visas i hello rutan Testutdata. |
| Utförlig |Meddelanden som ger information om allmänna eller felsökning. |Skrivs toojob historik endast om utförlig loggning är aktiverad för hello runbook. |Visas i utdatafönstret för hello Test bara om $VerbosePreference är tooContinue i hello runbook. |
| Pågår |Poster som genereras automatiskt före och efter varje aktivitet i hello runbook. Hej runbook ska inte försöka toocreate till sina egna förloppsposter eftersom de är avsedda för en interaktiv användare. |Skrivs toojob historik endast om förloppsloggning är aktiverad för hello runbook. |Visas inte i hello rutan Testutdata. |
| Felsökning |Meddelanden avsedda för en interaktiv användare. Bör inte användas i runbooks. |Skrivs inte toojob historik. |Skrivs inte tooTest Utdatarutan. |

## <a name="output-stream"></a>Utdataström
hello utdataströmmen är avsedd för utdata från objekt som skapats av ett skript eller ett arbetsflöde när det körs korrekt. I Azure Automation den här strömmen framför allt för objekt toobe som används av [överordnade runbooks som anropar hello aktuell runbook](automation-child-runbooks.md). När du [anropar en infogad runbook](automation-child-runbooks.md#invoking-a-child-runbook-using-inline-execution) från en överordnad runbook returnerar data från hello utdata dataströmmen toohello överordnade. Du bör endast använda hello utdata dataströmmen toocommunicate allmän information tillbaka toohello användare om du vet hello runbook aldrig kommer att anropas av en annan runbook. Som bästa praxis bör du vanligtvis använder, hello [utförliga strömmen](#Verbose) toocommunicate allmän information toohello användare.

Du kan skriva data toohello utdata dataström med [Write-Output](http://technet.microsoft.com/library/hh849921.aspx) eller genom att placera hello-objekt på en egen rad i hello runbook.

    #hello following lines both write an object toohello output stream.
    Write-Output –InputObject $object
    $object

### <a name="output-from-a-function"></a>Utdata från en funktion
När du skriver toohello utdataströmmen i en funktion som ingår i din runbook skickas hello utdata tillbaka toohello runbook. Om hello runbook tilldelar utdata tooa variabeln, är det inte skriva toohello utdataströmmen. Skriva tooany andra dataströmmar inifrån funktionen hello skrivs toohello motsvarande dataström för hello runbook.

Överväg att hello följande exempel-runbook.

    Workflow Test-Runbook
    {
        Write-Verbose "Verbose outside of function" -Verbose
        Write-Output "Output outside of function"
        $functionOutput = Test-Function
        $functionOutput

    Function Test-Function
     {
        Write-Verbose "Verbose inside of function" -Verbose
        Write-Output "Output inside of function"
      }
    }


hello utdataströmmen för hello runbook-jobbet är:

    Output inside of function
    Output outside of function

hello utförliga dataströmmen för hello runbook-jobbet är:

    Verbose outside of function
    Verbose inside of function

När du har publicerat hello runbook och innan du startar den, måste du också aktivera utförlig loggning i hello runbook inställningarna i ordning tooget hello utförlig strömmad utdata.

### <a name="declaring-output-data-type"></a>Deklarerande Utdatatyp
Ett arbetsflöde kan specificera hello datatypen i utdata med hello [OutputType-attributet](http://technet.microsoft.com/library/hh847785.aspx). Det här attributet har ingen effekt under körning, men det ger en indikation toohello runbook-redigerare vid designtillfället hello förväntades utdata från hello runbook. Eftersom hello verktygsuppsättningen för runbooks fortsätter tooevolve, ökar hello vikten av att deklarera utdatatyper i designläge. Därför är det en god rutin tooinclude deklarationen i alla runbooks som du skapar.

Här är en lista över exempel utdata typer:

* System.String
* System.Int32
* System.Collections.Hashtable
* Microsoft.Azure.Commands.Compute.Models.PSVirtualMachine

hello följande exempel-runbook matar ut ett strängobjekt och innehåller en förklaring av utdatatypen. Om din runbook matar ut en matris med en viss typ, bör du fortfarande ange hello typ som skillnad från tooan matris av typen hello.

    Workflow Test-Runbook
    {
       [OutputType([string])]

       $output = "This is some string output."
       Write-Output $output
    }

toodeclare utdata Skriv i Grapical eller grafisk PowerShell-arbetsflöde runbooks, kan du välja hello **indata och utdata** menyalternativet och anger hello namn för hello utdata typen.  Vi rekommenderar att du använder hello fullständig .NET klassen namnet toomake det lätt att identifiera när du refererar till den från en överordnad runbook.  Detta visar alla hello egenskaper för klass toohello databussen i hello runbook och ger stor flexibilitet när du använder dem för att införa villkorslogik, loggning och refererar till som värden för andra aktiviteter i hello runbook.<br> ![Alternativet Runbook indata och utdata](media/automation-runbook-output-and-messages/runbook-menu-input-and-output-option.png)

I följande exempel hello, har vi två grafiska runbook-flöden toodemonstrate den här funktionen.  Om det använda hello modulära runbook designmodellen har vi en runbook som fungerar som hello *mall för autentisering Runbook* hantera autentisering med Azure med hjälp av hello kör som-kontot.  Våra andra runbook, som normalt skulle utföra hello core logik tooautomate ett visst scenario kommer i så fall tooexecute hello *mall för autentisering Runbook* och visa hello resultat tooyour **Test** utdatarutan.  Under normala omständigheter har vi runbook gör något mot en resurs användning hello utdata från hello underordnad runbook.    

Här är hello grundläggande logiken för hello **AuthenticateTo Azure** runbook.<br> ![Autentisera mall för Runbook-exempel](media/automation-runbook-output-and-messages/runbook-authentication-template.png).  

Den omfattar hello utdatatypen *Microsoft.Azure.Commands.Profile.Models.PSAzureContext*, vilket returnerar hello profilegenskaper för autentisering.<br> ![Exempel på utdata för Runbook typ](media/automation-runbook-output-and-messages/runbook-input-and-output-add-blade.png) 

Denna runbook är mycket enkelt, finns men det en konfiguration objektet toocall ut här.  hello senaste aktiviteten körs hello **Write-Output** cmdlet och skrivningar hello profil data tooa $_ variabeln med ett PowerShell-uttryck för hello **Inputobject** som krävs för att cmdlet.  

Hello andra runbook i det här exemplet heter *Test ChildOutputType*, vi bara har två aktiviteter.<br> ![Exempel underordnade utdata typen Runbook](media/automation-runbook-output-and-messages/runbook-display-authentication-results-example.png) 

hello första aktivitet anropar hello **AuthenticateTo Azure** runbook och hello andra aktiviteten körs hello **Write-Verbose** med hello **datakällan** av ** Aktivitetsutdata** och hello värde för **fältet sökväg** är **Context.Subscription.SubscriptionName**, vilket är att ange hello kontexten utdata från hello ** AuthenticateTo Azure** runbook.<br> ![Write-Verbose cmdlet parametern datakälla](media/automation-runbook-output-and-messages/runbook-write-verbose-parameters-config.png)    

hello resultatet är hello namn hello prenumeration.<br> ![Testa ChildOutputType Runbook-resultat](media/automation-runbook-output-and-messages/runbook-test-childoutputtype-results.png)

En anteckning om hello funktionssätt hello utdatatypen kontroll.  När du skriver ett värde i hello utdatatypen fältet på hello indata och utdata egenskapsbladet har tooclick hello kontroll när du skriver in, för att din post toobe som identifieras av hello kontroll.  

## <a name="message-streams"></a>Meddelandeströmmar
Till skillnad från hello utdataströmmen är meddelandeströmmar avsedda toocommunicate information toohello användare. Det finns flera meddelandeströmmar för olika typer av information och var och en hanteras olika av Azure Automation.

### <a name="warning-and-error-streams"></a>Varningar och felströmmar
hello varningar och felströmmar är avsedda toolog problem som uppstår i en runbook. De skrivs toohello jobbhistoriken när en runbook körs och ingår i hello rutan Testutdata i hello Azure-hanteringsportalen när en runbook testas. Som standard att hello fortsätter runbook köras när en varning eller fel. Du kan ange att hello runbook ska göra uppehåll när en varning eller fel genom att ange en [inställningsvariabeln](#PreferenceVariables) i hello runbook innan du skapar hello-meddelande. Till exempel toocause en runbook toosuspend på ett fel som ett undantag, ange **$ErrorActionPreference** tooStop.

Skapa en varning eller fel meddelande med hello [Write-Warningg](https://technet.microsoft.com/library/hh849931.aspx) eller [Write-Error](http://technet.microsoft.com/library/hh849962.aspx) cmdlet. Aktiviteter kan också skriva toothese dataströmmar.

    #hello following lines create a warning message and then an error message that will suspend hello runbook.

    $ErrorActionPreference = "Stop"
    Write-Warning –Message "This is a warning message."
    Write-Error –Message "This is an error message that will stop hello runbook because of hello preference variable."

### <a name="verbose-stream"></a>Utförlig dataström
hello utförliga meddelandeströmmen är avsedd för allmän information om hello runbook-åtgärder. Eftersom hello [Felsökningsströmmen](#Debug) är inte tillgänglig i en runbook utförliga meddelanden ska användas för felsökningsinformation. Som standard lagras utförliga meddelanden från publicerade runbooks inte i hello jobbhistorik. toostore utförliga meddelanden, konfigurera publicerade runbooks tooLog utförliga poster på hello konfigurera fliken för hello runbook i hello Azure-hanteringsportalen. I de flesta fall bör du behålla standardinställningen hello inte logga utförliga poster för en runbook av prestandaskäl. Aktivera det här alternativet endast tootroubleshoot eller felsöka en runbook.

När [testar en runbook](automation-testing-runbook.md), visas inte utförlig meddelanden även om hello runbook är konfigurerade toolog utförliga poster. toodisplay utförliga meddelanden vid [testar en runbook](automation-testing-runbook.md), måste du ange hello $VerbosePreference variabeln tooContinue. Med dessa variabeln visas utförliga meddelanden i hello rutan Testutdata i hello Azure-portalen.

Skapa ett utförligt meddelande med hello [Write-Verbose](http://technet.microsoft.com/library/hh849951.aspx) cmdlet.

    #hello following line creates a verbose message.

    Write-Verbose –Message "This is a verbose message."

### <a name="debug-stream"></a>Felsökningsströmmen
Hej felsökningsströmmen är avsedd att användas med en interaktiv användare och ska inte användas i runbooks.

## <a name="progress-records"></a>Förloppsposter
Om du konfigurerar en runbook toolog förlopp registrerar (på fliken med hello konfigurera av hello runbook i hello Azure-portalen) och sedan en post skrivs toohello jobbhistoriken före och efter varje aktivitet körs. I de flesta fall bör du behålla hello standardinställningen att inte logga förloppsposter för en runbook i ordning toomaximize prestanda. Aktivera det här alternativet endast tootroubleshoot eller felsöka en runbook. När du testar en runbook visas inte förloppsmeddelanden även om hello runbook är konfigurerade toolog förloppsposter.

Hej [Write-Progress](http://technet.microsoft.com/library/hh849902.aspx) cmdlet är inte giltig i en runbook eftersom den är avsedd för användning med en interaktiv användare.

## <a name="preference-variables"></a>Preferensvariabler
Windows PowerShell använder [preferensvariabler](http://technet.microsoft.com/library/hh847796.aspx) toodetermine hur toorespond toodata skickas toodifferent utdataströmmar. Du kan ange dessa variabler i en runbook toocontrol hur de ska svara toodata skickas till olika dataströmmar.

hello visar följande tabell hello-preferensvariabler som kan användas i runbooks med sina giltiga värden och standardvärden. Observera att den här tabellen bara innehåller hello-värden som är giltiga i en runbook. Ytterligare värden är giltiga för preferensvariabler hello när de används i Windows PowerShell utanför Azure Automation.

| Variabel | Standardvärde | Giltiga värden |
|:--- |:--- |:--- |
| WarningPreference |Fortsätt |Stoppa<br>Fortsätt<br>SilentlyContinue |
| ErrorActionPreference |Fortsätt |Stoppa<br>Fortsätt<br>SilentlyContinue |
| VerbosePreference |SilentlyContinue |Stoppa<br>Fortsätt<br>SilentlyContinue |

hello i den följande tabellen listas hello beteende för hello preferensvariabelvärden som är giltiga i runbooks.

| Värde | Beteende |
|:--- |:--- |
| Fortsätt |Loggar hello-meddelande och fortsätter att köra hello runbook. |
| SilentlyContinue |Fortsätter att köra hello runbook utan att logga hello-meddelande. Detta påverkar hello ignoreras hello-meddelande. |
| Stoppa |Loggar hello-meddelande och pausar hello runbook. |

## <a name="retrieving-runbook-output-and-messages"></a>Hämtning av runbook-utdata och meddelanden
### <a name="azure-portal"></a>Azure Portal
Du kan visa hello information på runbook-jobb i hello Azure-portalen från hello på fliken jobb i en runbook. hello sammanfattning av hello jobbet visar hello indataparametrar och hello [utdataströmmen](#Output) i toogeneral informationen om hello jobbet och eventuella undantag om de inträffade. hello historik innehåller meddelanden från hello [utdataströmmen](#Output) och [varningar och Felströmmar](#WarningError) i tillägg toohello [utförliga strömmen](#Verbose) och [pågår Registrerar](#Progress) om hello runbook är konfigurerade toolog utförliga poster och förloppsposter.

### <a name="windows-powershell"></a>Windows PowerShell
I Windows PowerShell kan du hämta utdata och meddelanden från en runbook med hello [Get-AzureAutomationJobOutput](https://msdn.microsoft.com/library/mt603476.aspx) cmdlet. Den här cmdleten kräver hello-ID för hello jobb och har en parameter som kallas dataströmmen där du kan ange vilken dataström tooreturn. Du kan ange eventuella tooreturn alla dataströmmar för hello jobb.

hello följande exempel startas en exempel-runbook och väntar tills den toocomplete. När slutfört, utdataström dess från hello jobb.

    $job = Start-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook"

    $doLoop = $true
    While ($doLoop) {
       $job = Get-AzureRmAutomationJob -ResourceGroupName "ResourceGroup01" `
       –AutomationAccountName "MyAutomationAccount" -Id $job.JobId
       $status = $job.Status
       $doLoop = (($status -ne "Completed") -and ($status -ne "Failed") -and ($status -ne "Suspended") -and ($status -ne "Stopped")
    }

    Get-AzureRmAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName "MyAutomationAccount" -Id $job.JobId –Stream Output

### <a name="graphical-authoring"></a>Grafisk redigering
Extra loggning är tillgängliga i hello form av aktivitetsnivå spårning för grafiska runbook-flöden.  Det finns två nivåer av spårning: Basic och detaljerad.  Du kan se hello starta i grundläggande spårning och sluttid för varje aktivitet i hello runbook samt information relaterad tooany aktivitet omförsök, till exempel antal försök och starttiden för hello aktivitet.  I Detaljerad spårning får du grundläggande spårning plus indata och utdata för varje aktivitet.  Observera att för närvarande hello trace poster skrivs med hello utförlig dataström, så du måste aktivera utförlig loggning när du aktiverar spårning.  För grafiska runbook-flöden med aktiverad spårning det behövs ingen toolog förloppsposter eftersom hello grundläggande spårning fungerar hello samma syfte och är mer informativ.

![Grafisk redigering jobbet strömmar vy](media/automation-runbook-output-and-messages/job-streams-view-blade.png)

Du kan se från hello ovan skärmbild att när du aktiverar utförlig loggning och spårning för grafiska runbook-flöden mycket mer information finns i hello produktion visa jobb dataströmmar.  Extra informationen kan vara nödvändigt för att felsöka problem med en runbook i produktionen och därför bör du bara aktivera det för detta ändamål och inte som en allmän regel.    
hello Trace poster kan vara särskilt många.  Med grafisk runbook får spårning av du två toofour poster per aktivitet beroende på om du har konfigurerat grundläggande eller detaljerat spårning.  Om du inte behöver den här informationen tootrack hello förloppet för en runbook för att felsöka, kanske du vill tookeep spårning avstängd.

**tooenable aktivitetsnivå spårning, utföra hello följande steg.**

1. Hello Azure-portalen, öppna ditt Automation-konto.
2. Klicka på hello **Runbooks** panelen tooopen hello lista över runbooks.
3. Klicka på tooselect en grafisk runbook från listan med runbooks på hello Runbooks blad.
4. Klicka på hello inställningsbladet för hello markerad runbook **loggning och spårning**.
5. Hello loggning och spårning bladet under logga utförliga meddelanden, klicka på **på** tooenable utförlig loggning och udner aktivitetsnivå spårning, ändra hello Spårningsnivån för**grundläggande** eller **detaljerad ** utifrån hello nivå av spårning som du behöver.<br>
   
   ![Grafisk redigering loggning och spårning bladet](media/automation-runbook-output-and-messages/logging-and-tracing-settings-blade.png)

### <a name="microsoft-operations-management-suite-oms-log-analytics"></a>Microsoft Operations Management Suite (OMS) logganalys
Automatisering kan skicka runbook-jobbet status och jobbstatus dataströmmar tooyour Microsoft Operations Management Suite (OMS) logganalys-arbetsytan.  Du kan göra följande med logganalys,

* Skaffa dig insikter om dina Automation-jobb 
* Utlösare som en e-post eller en avisering baserat på din runbook jobbets status (t.ex. misslyckades eller pausas) 
* Skriva avancerade frågor över jobb-strömmar 
* Korrelera jobb över Automation-konton 
* Visualisera dina jobbhistorik över tid    

Mer information om hur tooconfigure integrering med logganalys toocollect korrelera och agera på jobbdata finns [vidarebefordra jobbstatus och jobbet strömmar från Automation tooLog Analytics (OMS)](automation-manage-send-joblogs-log-analytics.md).

## <a name="next-steps"></a>Nästa steg
* Mer om runbook-körningen hur toomonitor runbook-jobb och annan teknisk information finns i toolearn [spåra ett runbook-jobb](automation-runbook-execution.md)
* hur toodesign och använda underordnade runbooks, se toounderstand [underordnade runbooks i Azure Automation](automation-child-runbooks.md)

