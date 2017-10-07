---
title: aaaGraphical redigering i Azure Automation | Microsoft Docs
description: "Grafisk redigering kan du toocreate runbooks för Azure Automation utan arbetar med kod. Den här artikeln innehåller en introduktion toographical redigering och alla hello information behövs toostart skapar en grafisk runbook."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 4b6f840c-e941-4293-a728-b33407317943
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/14/2017
ms.author: magoedte;bwren
ms.openlocfilehash: 6ddf18b992d5e5f7f4af95f344007a63ac498549
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="graphical-authoring-in-azure-automation"></a>Grafisk redigering i Azure Automation
## <a name="introduction"></a>Introduktion
Grafisk redigering kan du toocreate runbooks för Azure Automation utan hello svårigheter hello underliggande Windows PowerShell eller PowerShell-arbetsflöde kod. Du lägger till aktiviteter toohello arbetsytan från ett bibliotek med cmdlets och runbooks, kopplar ihop dem och konfigurera tooform ett arbetsflöde.  Om du någon gång har arbetat med System Center Orchestrator eller Service Management Automation (SMA) bör det se ut bekant tooyou.   

Den här artikeln innehåller en introduktion toographical redigering och hello begrepp som du behöver tooget igång för att skapa en grafisk runbook.

## <a name="graphical-runbooks"></a>Grafiska runbook-flöden
Alla runbooks i Azure Automation är Windows PowerShell-arbetsflöden.  Grafisk och grafisk PowerShell-arbetsflöde runbooks generera PowerShell-kod som körs av hello Automation arbetare, men du inte kan tooview den eller ändra den direkt.  En grafisk runbook kan vara konverterade tooa grafisk PowerShell-arbetsflödesrunbook och vice versa, men de kan inte vara konverterade tooa text-runbook. En befintlig textrepresentation runbook kan inte importeras till hello grafiska redigerare.  

## <a name="overview-of-graphical-editor"></a>Översikt över grafiska redigerare
Du kan öppna hello grafiska redigerare i hello Azure-portalen genom att skapa eller redigera en grafisk runbook.

![Grafisk arbetsytan](media/automation-graphical-authoring-intro/runbook-graphical-editor.png)

hello beskrivs följande avsnitt hello kontroller i hello grafiska redigerare.

### <a name="canvas"></a>Arbetsytan
hello arbetsytan är där du utformar din runbook.  Du lägger till aktiviteter från hello noderna i hello biblioteket kontrollen toohello runbook och Anslut dem med länkar toodefine hello logik för hello runbook.

Du kan använda hello kontroller längst hello hello arbetsytan toozoom in och ut.

![Grafisk arbetsytan](media/automation-graphical-authoring-intro/runbook-canvas-controls.png)

### <a name="library-control"></a>Biblioteket kontroll
hello biblioteket kontroll är där du väljer [aktiviteter](#activities) tooadd tooyour runbook.  Du lägger till dem toohello arbetsyta där du ansluter dem tooother aktiviteter.  Den innehåller fyra avsnitt som beskrivs i följande tabell hello.

| Avsnittet | Beskrivning |
|:--- |:--- |
| Cmdlet: ar |Innehåller alla hello-cmdletar som kan användas i din runbook.  Cmdlets ordnas efter modulen.  Alla hello-moduler som du har installerat i ditt automation-konto blir tillgängliga. |
| Runbooks |Innehåller hello runbooks i ditt automation-konto. Dessa runbooks kan läggas till toohello arbetsytan toobe används som underordnade runbooks. Endast runbooks av hello samma core typ som hello runbook redigeras visas; för grafisk som runbooks endast PowerShell-baserade runbooks visas, medan för grafisk PowerShell-arbetsflöde runbooks endast PowerShell--Arbetsflödesbaserade runbooks visas. |
| Tillgångar |Innehåller hello [automation tillgångar](http://msdn.microsoft.com/library/dn939988.aspx) i ditt automation-konto som kan användas i din runbook.  När du lägger till en tillgång tooa runbook läggs en arbetsflödesaktivitet som hämtar hello markerade tillgången.  Hello gäller variabeln tillgångar, du kan välja om tooadd en aktivitet tooget hello variabel eller ange hello-variabeln. |
| Runbook-kontroll |Innehåller runbook kontroll aktiviteter som kan användas i din aktuella runbook. En *knutpunkt* tar flera inmatningar och väntar tills alla har slutförts innan du fortsätter hello-arbetsflöde. En *kod* aktiviteten körs en eller flera rader med PowerShell eller PowerShell-arbetsflöde kod beroende på typ av hello grafisk runbook.  Du kan använda den här aktiviteten för anpassad kod eller funktioner som är svår tooachieve med andra aktiviteter. |

### <a name="configuration-control"></a>Konfigurationskontroll
Hej Konfigurationskontroll kan du ge information för ett objekt som valts på hello arbetsytan. hello-egenskaper som är tillgängliga i den här kontrollen beror på hello typ av objekt som valts.  När du väljer ett alternativ i hello Konfigurationskontroll öppnas ytterligare blad i ordning tooprovide ytterligare information.

### <a name="test-control"></a>Testa kontrollen
hello Test kontrollen visas inte när hello grafiska redigerare först startas. Det öppnas när du interaktivt [testa en grafisk runbook](#graphical-runbook-procedures).  

## <a name="graphical-runbook-procedures"></a>Grafisk runbook procedurer
### <a name="exporting-and-importing-a-graphical-runbook"></a>Exportera och importera en grafisk runbook
Du kan bara exportera hello publicerade versionen av en grafisk runbook.  Om hello runbook inte har ännu publicerats, sedan hello **Export publicerade** knappen inaktiveras.  När du klickar på hello **Export publicerade** knappen, hello runbook är hämtade tooyour lokala dator.  hello filen hello namn matchar hello namnet på hello runbook med en *graphrunbook* tillägg.

![Exportera publicerade](media/automation-graphical-authoring-intro/runbook-export.png)

Du kan importera en grafisk eller grafisk PowerShell-arbetsflöde runbook-filen genom att välja hello **importera** alternativ när du lägger till en runbook.   När du väljer hello filen tooimport, kan du behålla hello samma **namn** eller ange en ny.  Hej Runbooktyp fältet visar hello typ av runbook när den utvärderar hello-filen som valts och om du försöker tooselect en annan typ som inte är korrekt, ett meddelande visas om det finns möjliga konflikter och under konverteringen, kan det finnas syntaxfel.  

![Importera runbook](media/automation-graphical-authoring-intro/runbook-import-revised20165.png)

### <a name="testing-a-graphical-runbook"></a>Testa en grafisk runbook
Du kan testa hello utkastversionen för en runbook i hello Azure-portalen medan lämnar hello publicerade versionen av hello runbook oförändrade, eller en ny runbook kan du testa innan den har publicerats. Detta gör att du tooverify som hello runbook fungerar korrekt innan du ersätter hello publicerad version. När du testar en runbook körs hello utkast-runbooken och alla åtgärder som den utför slutförs. Ingen jobbhistorik skapas, men visas utdata i hello rutan Testutdata. 

Öppna hello Test-kontroll för en runbook genom att öppna hello runbook för redigering och klicka sedan på hello **Test rutan** knappen.

![Fönstret testknappen](media/automation-graphical-authoring-intro/runbook-edit-test-pane.png)

hello Test kontrollen uppmanas för någon indataparametrar och du kan starta hello runbook genom att klicka på hello **starta** knappen.

![Testa kontrollknappar](media/automation-graphical-authoring-intro/runbook-test-start.png)

### <a name="publishing-a-graphical-runbook"></a>Publicera en grafisk runbook
Varje runbook i Azure Automation har ett utkast och en publicerad version. Endast hello publicerade versionen är tillgänglig toobe kör och bara utkastet hello kan redigeras. hello publicerade versionen påverkas inte av några ändringar toohello utkastet. När hello utkastversionen är klar toobe som är tillgängliga, sedan publicerar du den som skriver över hello publicerade versionen med hello utkastet.

Du kan publicera en grafisk runbook genom att öppna hello runbook för redigering och sedan klicka på hello **publicera** knappen.

![Knappen Publicera](media/automation-graphical-authoring-intro/runbook-edit-publish.png)

När en runbook inte har ännu publicerats, har den statusen **ny**.  När den publiceras, har den statusen **publicerade**.  Om du redigerar hello runbook när den har publicerats och hello utkast och publicerad versioner är olika, hello runbook har statusen **i Redigera**.

![Runbook-status](media/automation-graphical-authoring-intro/runbook-statuses-revised20165.png) 

Du kan också ha hello alternativet toorevert toohello publicerade versionen av en runbook.  Detta raderas alla ändringar som gjorts sedan hello senast publicerades och ersätter hello utkastversionen för hello runbook med hello publicerade versionen.

![Återställa toopublished knappen](media/automation-graphical-authoring-intro/runbook-edit-revert-published.png)

## <a name="activities"></a>Aktiviteter
Aktiviteter är hello byggblocken i en runbook.  En aktivitet kan vara en PowerShell-cmdlet, en underordnad runbook eller en arbetsflödesaktivitet.  Du lägger till en aktivitet toohello runbook genom att högerklicka på den i hello biblioteket kontroll och välja **lägga till toocanvas**.  Sedan kan du klicka och dra hello aktiviteten tooplace den någonstans på hello arbetsytan som du.  hello påverkas platsen för hello hello aktivitet på hello arbetsytan inte hello driften av hello runbook på något sätt.  Du kan layout din runbook men du tycker att det lämpligaste toovisualize dess drift. 

![Lägg till toocanvas](media/automation-graphical-authoring-intro/add-to-canvas-revised20165.png)

Välj hello aktivitet på hello arbetsytan tooconfigure dess egenskaper och parametrar i hello Configuration bladet.  Du kan ändra hello **etikett** av hello aktiviteten toosomething som är beskrivande tooyou.  hello ursprungliga cmdlet körs fortfarande kan du bara ändrar dess namn som ska användas i hello grafiska redigerare.  hello etikett måste vara unika inom hello runbook. 

### <a name="parameter-sets"></a>Parameteruppsättningarna
En parameteruppsättning definierar hello obligatoriska och valfria parametrar som tar emot värden för en viss cmdlet.  Alla cmdletar har minst en parameter har angetts och vissa har flera.  Om en cmdlet har flera parameteruppsättningar, måste du välja vilket som du ska använda innan du kan konfigurera parametrar.  hello-parametrar som du kan konfigurera beror på hello parameteruppsättning som du väljer.  Du kan ändra hello parameteruppsättning som används av en aktivitet genom att välja **parameterinställning** och välja en annan uppsättning.  I det här fallet går ett parametervärde som du har konfigurerat förlorade.

I följande exempel hello, har hello Get-AzureRmVM cmdlet tre parameteruppsättningar.  Du kan konfigurera parametervärden förrän du väljer ett av hello parameteruppsättningar.  Hej ListVirtualMachineInResourceGroupParamSet parameteruppsättning finns för att returnera alla virtuella datorer i en resursgrupp och en valfri parameter.  Hej GetVirtualMachineInResourceGroupParamSet är för att ange hello virtuell dator och du vill tooreturn har två obligatoriska och en valfri parameter.

![Parameteruppsättningen](media/automation-graphical-authoring-intro/get-azurermvm-parameter-sets.png)

#### <a name="parameter-values"></a>Parametervärden
När du anger ett värde för en parameter kan du välja en datakälla toodetermine hur hello värde ska anges.  hello-datakällor som är tillgängliga för en viss parameter beror på hello giltiga värden för parametern.  Till exempel blir inte Null ett tillgängligt alternativ för en parameter som inte tillåter null-värden.

| Datakälla | Beskrivning |
|:--- |:--- |
| Konstant värde |Ange ett värde för parametern hello.  Detta är endast tillgängligt för hello följande datatyper: Int32, Int64, String, Boolean, DateTime, växel. |
| Aktivitetsutdata |Utdata från en aktivitet som föregår hello aktuell aktivitet i hello arbetsflöde.  Alla giltiga aktiviteter visas.  Välj bara hello aktiviteten toouse utdata för hello parametervärde.  Om aktiviteten hello matar ut ett objekt med flera egenskaper, kan du ange i hello namn för hello egenskapen när du har valt hello-aktivitet. |
| Runbook-indata |Välj en runbookinmatningsparameter som inkommande toohello Aktivitetsparametern. |
| Variabeltillgång |Välj ett Automation-variabel som indata. |
| Autentiseringsuppgiftstillgång |Välj autentiseringsuppgifter för Automation som indata. |
| Certifikattillgång |Välj ett Automation-certifikat som indata. |
| Anslutningstillgång |Välj en Automationsanslutning som indata. |
| PowerShell-uttryck |Ange enkla [PowerShell-uttryck](#powershell-expressions).  hello uttrycket ska utvärderas innan hello aktivitet och hello resultatet som används för hello parametervärde.  Du kan använda variabler toorefer toohello utdata från en aktivitet eller en runbookinmatningsparameter. |
| Inte konfigurerat |Rensar alla värden som tidigare har konfigurerats. |

#### <a name="optional-additional-parameters"></a>Valfri ytterligare parametrar
Alla cmdletar har hello alternativet tooprovide ytterligare parametrar.  Dessa är vanliga PowerShell-parametrar eller andra anpassade parametrar.  Visas med en textruta där du kan ange parametrar med PowerShell-syntax.  Till exempel toouse hello **utförlig** gemensamma parametern anger du **”-Verbose: $True”**.

### <a name="retry-activity"></a>Gör om aktivitet
**Omförsök** kan en aktivitet toobe köras flera gånger tills ett visst villkor är uppfyllt, ungefär som en loop.  Du kan använda den här funktionen för aktiviteter som ska köras flera gånger, är tillförlitligt och kanske behöver fler än en försöka för lyckad eller testa hello Utdatainformationen hello aktivitet för giltiga data.    

När du aktiverar retry för en aktivitet kan ange du en fördröjning och ett villkor.  hello fördröjningen är hello tid (i sekunder eller minuter) som hello runbook ska vänta innan den kör hello aktiviteten igen.  Om ingen fördröjning anges körs aktiviteten hello igen omedelbart när den är klar. 

![Fördröjning av försök igen om en aktivitet](media/automation-graphical-authoring-intro/retry-delay.png)

hello försök villkor är ett PowerShell-uttryck som utvärderas när varje gång hello aktivitet har körts.  Om hello-uttrycket matchar tooTrue, sedan körs hello aktiviteten igen.  Om hello-uttrycket matchar tooFalse sedan hello aktiviteten körs inte igen och hello runbook flyttar toohello nästa aktivitet. 

![Fördröjning av försök igen om en aktivitet](media/automation-graphical-authoring-intro/retry-condition.png)

hello försök villkor kan använda en variabel med namnet $RetryData som ger åtkomst tooinformation om hello återförsök för aktiviteten.  Den här variabeln har hello egenskaper i hello i den följande tabellen.

| Egenskap | Beskrivning |
|:--- |:--- |
| NumberOfAttempts |Antalet gånger som hello aktiviteten har körts. |
| Resultat |Utdata från hello senast kördes på hello-aktivitet. |
| TotalDuration |Tidsgränsen gått sedan hello aktiviteten startades hello första gången. |
| StartedAt |Tid i UTC-format hello aktiviteten startades först. |

Följande är exempel på aktiviteten försök villkor.

    # Run hello activity exactly 10 times.
    $RetryData.NumberOfAttempts -ge 10 

    # Run hello activity repeatedly until it produces any output.
    $RetryData.Output.Count -ge 1 

    # Run hello activity repeatedly until 2 minutes has elapsed. 
    $RetryData.TotalDuration.TotalMinutes -ge 2

När du har konfigurerat ett villkor för återförsök för aktiviteten hello omfattar två visuella tips tooremind du.  En presenteras i hello aktivitet och hello andra är när du granskar hello konfigurationen av hello-aktivitet.

![Visuella indikatorer för återförsök för aktivitet](media/automation-graphical-authoring-intro/runbook-activity-retry-visual-cue.png)

### <a name="workflow-script-control"></a>Arbetsflödeskontroll för skript
En kod kontroll är en särskild aktivitet som accepterar PowerShell eller PowerShell-arbetsflöde skript beroende på hello typ av grafisk runbook som skapats i ordning tooprovide funktioner som annars inte kanske tillgänglig.  Den kan inte acceptera parametrar, men den kan använda variabler för aktivitetens utdata och runbook-indataparametrar.  Alla utdata för hello aktiviteten läggs toohello databussen om den inte har ingen utgående länk i vilket fall det läggs toohello utdata från hello runbook.

Till exempel beräkningar hello följande kod datum med hjälp av en runbook inkommande variabel med namnet $NumberOfDays.  Sedan skickar den ett beräknat datum tid som utdata toobe som används av efterföljande aktiviteter i hello runbook.

    $DateTimeNow = (Get-Date).ToUniversalTime()
    $DateTimeStart = ($DateTimeNow).AddDays(-$NumberOfDays)}
    $DateTimeStart


## <a name="links-and-workflow"></a>Länkar och arbetsflöde
En **länk** ansluter två aktiviteter i en grafisk runbook.  Den visas på arbetsytan hello som en pil som pekar från hello källa aktiviteten toohello målaktiviteten.  hello aktiviteter körs i hello riktning hello pilen med hello målaktiviteten startar när hello källaktiviteten har slutförts.  

### <a name="create-a-link"></a>Skapa en länk
Skapa en länk mellan två aktiviteter genom att välja hello källaktiviteten och klicka på hello cirkel längst hello hello form.  Dra hello pilen toohello målaktiviteten och version.

![Skapa en länk](media/automation-graphical-authoring-intro/create-link-revised20165.png)

Välj hello länken tooconfigure dess egenskaper i hello Configuration bladet.  Detta inkluderar hello länktypen som beskrivs i följande tabell hello.

| Länktypen | Beskrivning |
|:--- |:--- |
| Pipeline |hello-målaktiviteten körs en gång för varje objektutdata från hello källaktiviteten.  hello-målaktiviteten körs inte om hello källaktiviteten ger inga utdata.  Utdata från källaktiviteten hello är tillgänglig som ett objekt. |
| Sekvens |hello-målaktiviteten körs en gång.  Den tar emot en matris med objekt från hello källaktiviteten.  Utdata från hello källa aktivitet är tillgänglig som en matris med objekt. |

### <a name="starting-activity"></a>Startar aktivitet
En grafisk runbook startar med aktiviteter som inte har en inkommande anslutning.  Det här är ofta bara en aktivitet som skulle fungera som hello startar aktivitet för hello runbook.  Om flera aktiviteter inte har en inkommande anslutning, startar hello runbook genom att köra dem parallellt.  Den sedan följer hello länkar toorun andra aktiviteter som varje är klar.

### <a name="conditions"></a>Villkor
När du anger ett villkor på en länk körs bara hello målaktiviteten om hello villkoret löser tootrue.  Normalt använder du en $ActivityOutput variabel i ett villkor tooretrieve hello utdata från hello källaktiviteten.  

Du anger ett villkor för ett enskilt objekt för en pipelinelänk och hello villkoret har utvärderats för varje objektutdata i hello källaktiviteten.  hello-målaktiviteten körs sedan för varje objekt som uppfyller hello villkor.  Till exempel med en Källaktivitet för Get-AzureRmVm hello följande syntax kan användas för en villkorlig pipeline länken tooretrieve endast virtuella datorer i hello resursgrupp med namnet *Group1*.  

    $ActivityOutput['Get Azure VMs'].Name -match "Group1"

För en sekvens-länk, är bara hello villkoret utvärderas som det en gång eftersom returneras en matris som innehåller alla objekt utdata från hello källaktiviteten.  Därför en länk i aktivitetssekvensen kan inte användas för att filtrera som en pipelinelänk, men bara avgör huruvida hello nästa aktivitet körs. Ta till exempel hello följande uppsättning aktiviteter i vår starta VM-runbook.<br> ![Villkorlig länk med sekvenser](media/automation-graphical-authoring-intro/runbook-conditional-links-sequence.png)<br>
Det finns tre olika sekvens länkar som verifierar värden angavs tootwo runbook indataparametrar som representerar namn på virtuell dator och resursgruppens namn i ordning toodetermine som är hello lämplig åtgärd tootake - startar en enskild virtuell dator, starta alla virtuella datorer i hello resurs-grupp, eller alla virtuella datorer i en prenumeration.  Här är hello sekvens länken mellan Anslut tooAzure och Get enskild VM, hello villkoret logik:

    <# 
    Both VMName and ResourceGroupName runbook input parameters have values 
    #>
    (
    (($VMName -ne $null) -and ($VMName.Length -gt 0))
    ) -and (
    (($ResourceGroupName -ne $null) -and ($ResourceGroupName.Length -gt 0))
    )

När du använder en villkorlig länk filtreras hello data är tillgängliga från hello källa aktivitet tooother aktiviteter i den grenen av hello villkoret.  Om en aktivitet är hello källa toomultiple länkar, sedan hello data tillgängliga tooactivities i varje gren beror på hello villkor i hello länken ansluter toothat grenen.

Till exempel hello **Start AzureRmVm** i hello runbook nedan startar alla virtuella datorer.  Den har två villkorliga länkar.  hello första villkorlig länken använder hello uttryck *$ActivityOutput ['Start-AzureRmVM']. IsSuccessStatusCode - eq $true* toofilter om hello Start AzureRmVm aktivitet har slutförts.  hello använder andra hello uttryck *$ActivityOutput ['Start-AzureRmVM']. IsSuccessStatusCode - ne $true* toofilter om hello Start AzureRmVm aktiviteten misslyckades toostart hello virtuell dator.  

![Exempel på villkorlig länk](media/automation-graphical-authoring-intro/runbook-conditional-links.png)

Alla aktiviteter som följer hello första länken och använder hello aktivitetsutdata från Get-AzureVM får endast hello virtuella datorer som startades vid hello tidpunkt som Get-AzureVM kördes.  Alla aktiviteter som följer hello andra länken får endast hello hello virtuella datorer som har stoppats för närvarande hello som Get-AzureVM kördes.  Alla aktiviteter efter hello tredje länken kommer alla virtuella datorer oavsett deras körtillstånd.

### <a name="junctions"></a>Vägkorsningar
Knutpunkt är en särskild aktivitet som ska vänta tills inkommande grenar har slutförts.  Detta gör att du toorun flera aktiviteter parallellt och se till att alla har slutförts innan du fortsätter.

När en förgrening kan ha ett obegränsat antal inkommande länkar, kan inte mer än en av dessa länkar vara en pipeline.  hello antalet inkommande sekvens länkar är inte begränsad.  Du får toocreate hello knutpunkt med flera inkommande pipeline-länkar och sparar hello runbook, men misslyckas när den körs.

hello exemplet nedan är en del av en runbook som startar en uppsättning virtuella datorer när samtidigt hämta korrigeringsfiler toobe tillämpas toothose datorer.  Knutpunkt är används tooensure att båda dessa förfaranden måste slutföras innan hello runbook fortsätter.

![Knutpunkt](media/automation-graphical-authoring-intro/runbook-junction.png)

### <a name="cycles"></a>Cykler
En cykel är när en målaktiviteten länkar tillbaka tooits datakällan aktivitet eller tooanother aktiviteten att så småningom länkar tillbaka tooits källa.  Cykler tillåts inte för närvarande i grafiska redigering.  Om din runbook har en cykel, kommer att sparas korrekt men får ett felmeddelande när den körs.

![Cykel](media/automation-graphical-authoring-intro/runbook-cycle.png)

### <a name="sharing-data-between-activities"></a>Dela data mellan aktiviteter
Alla data som är resultatet av en aktivitet med en utgående anslutning av skrivs toohello *databussen* för hello runbook.  Alla aktiviteter i hello runbook kan använda data på hello databussen toopopulate parametervärden eller inkludera i skriptkod.  En aktivitet kan komma åt hello utdata från alla föregående aktiviteter i hello arbetsflöde.     

Hur hello data skrivs toohello databussen beror på hello typ av länk på hello-aktivitet.  För en **pipeline**, hello data visas som multiplar objekt.  För en **sekvens** länka, hello data visas som en matris.  Om det finns bara ett värde, blir resultatet som ett enstaka element-matris.

Du kan komma åt data på hello databussen på något av två sätt.  Först använder en **Aktivitetsutdata** datakällan toopopulate en parameter av en annan aktivitet.  Om hello utdata är ett objekt, kan du ange en enskild egenskap.

![Aktivitetsutdata](media/automation-graphical-authoring-intro/activity-output-datasource-revised20165.png)

Du kan också hämta hello utdata för en aktivitet i en **PowerShell-uttryck** datakällan eller från en **Arbetsflödesskriptet** aktivitet med en ActivityOutput-variabel.  Om hello utdata är ett objekt, kan du ange en enskild egenskap.  ActivityOutput variabler använda hello följande syntax.

    $ActivityOutput['Activity Label']
    $ActivityOutput['Activity Label'].PropertyName 

### <a name="checkpoints"></a>Kontrollpunkter
Du kan ange [kontrollpunkter](automation-powershell-workflow.md#checkpoints) i en grafisk PowerShell-arbetsflödesrunbook genom att välja *kontrollpunkt runbook* på alla aktiviteter.  Detta leder till en kontrollpunkt toobe efter hello aktiviteten körs.

![Kontrollpunkt](media/automation-graphical-authoring-intro/set-checkpoint.png)

Kontrollpunkter är endast aktiverad i grafisk PowerShell-arbetsflöde runbooks, den är inte tillgänglig i grafiska runbook-flöden.  Om hello runbook använder Azure-cmdlets, ska du följa eventuella kontrollpunkt aktivitet med en Add-AzureRMAccount om hello runbook stoppas och startas om från den här kontrollpunkten på en annan worker. 

## <a name="authenticating-tooazure-resources"></a>Autentisera tooAzure resurser
Runbooks som hanterar Azure-resurser i Azure Automation kräver autentisering tooAzure.  Hej [kör som-konto](automation-offering-get-started.md#creating-an-automation-account) (även hänvisade tooas ett huvudnamn för tjänsten) är hello standard metoden tooaccess Azure Resource Manager-resurser i din prenumeration med Automation-runbooks.  Du kan lägga till den här funktionen tooa grafisk runbook genom att lägga till hello **AzureRunAsConnection** anslutningstillgång som använder hello PowerShell [Get-AutomationConnection](https://technet.microsoft.com/library/dn919922%28v=sc.16%29.aspx) cmdlet, och [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet toohello arbetsytan. Detta illustreras i följande exempel hello.<br>![Kör som-aktiviteter för autentisering](media/automation-graphical-authoring-intro/authenticate-run-as-account.png)<br>
hello aktiviteten hämta kör som-anslutning (d.v.s. Get-AutomationConnection), har konfigurerats med ett konstant värde-datakälla med namnet AzureRunAsConnection.<br>![Kör som-anslutningskonfiguration](media/automation-graphical-authoring-intro/authenticate-runas-parameterset.png)<br>
hello nästa aktivitet, Add-AzureRmAccount lägger till hello autentiserad kör som-konto för användning i hello runbook.<br>
![Lägg till AzureRmAccount parameteruppsättning](media/automation-graphical-authoring-intro/authenticate-conn-to-azure-parameter-set.png)<br>
För hello parametrar **APPLICATIONID**, **CERTIFICATETHUMBPRINT**, och **TENANTID** behöver du toospecify hello namnet på hello egenskap för hello fältet sökvägen eftersom hello aktivitet matar ut ett objekt med flera egenskaper.  När du kör hello runbook misslyckas det annars när tooauthenticate.  Det här är vad du behöver en minsta tooauthenticate din runbook med hello kör som-konto.

toomaintain bakåtkompatibilitet kompatibilitetsnivån för prenumeranter som har skapat ett Automation-konto med hjälp av en [Azure AD-användarkontot](automation-create-aduser-account.md) toomanage Azure klassisk distribution eller hello metoden tooauthenticate för Azure Resource Manager-resurser är hello Add-AzureAccount cmdlet med ett [autentiseringsuppgiftstillgång](automation-credentials.md) som representerar en Active Directory-användare med åtkomst toohello Azure-konto.

Du kan lägga till den här funktionen tooa grafisk runbook genom att lägga till autentiseringsuppgifter tillgången toohello arbetsytan följt av en Add-AzureAccount aktivitet.  Lägg till AzureAccount använder hello autentiseringsuppgifter aktivitet för indata.  Detta illustreras i följande exempel hello.

![Autentisering aktiviteter](media/automation-graphical-authoring-intro/authentication-activities.png)

Du har tooauthenticate hello början hello runbook och efter varje kontrollpunkt.  Det innebär att lägga till en tillägg Add-AzureAccount aktivitet efter alla Checkpoint-Workflow aktivitet. Du behöver inte lägga autentiseringsuppgifter aktivitet eftersom du kan använda hello samma 

![Aktivitetsutdata](media/automation-graphical-authoring-intro/authentication-activity-output.png)

## <a name="runbook-input-and-output"></a>Runbook indata och utdata
### <a name="runbook-input"></a>Runbook-indata
En runbook kan kräva indata antingen från en användare när de startar hello runbook via hello Azure-portalen eller från en annan runbook om hello aktuella används som en underordnad.
Till exempel om du har en runbook som skapar en virtuell dator kan behöva du tooprovide information, till exempel hello namn för hello virtuell dator och andra egenskaper varje gång du startar hello runbook.  

Du accepterar indata för en runbook genom att definiera en eller flera indataparametrar.  Du kan ange värden för parametrarna varje gång hello runbook startas.  När du startar en runbook med hello Azure-portalen, blir du ombedd tooprovide värden för hello av hello runbook indataparametrar.

Du kan komma åt indataparametrar för en runbook genom att klicka på hello **ingående och utgående** i verktygsfältet hello runbook.  

![Runbook-o](media/automation-graphical-authoring-intro/runbook-edit-input-output.png) 

Då öppnas hello **indata och utdata** kontroll där du kan redigera en befintlig indataparameter eller skapa en ny genom att klicka på **lägga till indata**. 

![Lägga till indata](media/automation-graphical-authoring-intro/runbook-edit-add-input.png)

Varje Indataparametern definieras av hello egenskaper i hello i den följande tabellen.

| Egenskap | Beskrivning |
|:--- |:--- |
| Namn |hello unikt namn för hello-parametern.  Detta kan bara innehålla alfanumeriska tecken och får inte innehålla blanksteg. |
| Beskrivning |En valfri beskrivning för hello indataparameter. |
| Typ |Datatypen för hello parametervärdet.  hello Azure-portalen anger en lämplig kontroll för hello-datatypen för varje parameter vid fråga om indata. |
| Obligatorisk |Anger om ett värde måste anges för hello-parametern.  Hej runbook kan inte startas om du inte anger ett värde för varje obligatorisk parameter som inte har något definierat standardvärde. |
| Standardvärde |Anger vilket värde som ska användas för hello-parameter om inget anges.  Detta kan antingen vara Null eller ett specifikt värde. |

### <a name="runbook-output"></a>Utdata från Runbooks
Data som skapats av alla aktiviteter som inte har en utgående anslutning läggs toohello [utdata från hello runbook](http://msdn.microsoft.com/library/azure/dn879148.aspx).  hello utdata sparas med hello runbook-jobb och är tillgängliga tooa överordnad runbook när hello runbook används som en underordnad.  

## <a name="powershell-expressions"></a>PowerShell-uttryck
En av hello fördelarna med grafiska redigering är att ge dig hello möjlighet toobuild en runbook med minimal kunskap om PowerShell.  För närvarande behöver tooknow lite PowerShell även om för att fylla vissa [parametervärden](#activities) och för inställningen [länka villkor](#links-and-workflow).  Det här avsnittet ger en snabb introduktion tooPowerShell uttryck för de användare som inte kanske är bekant med den.  Fullständig information om PowerShell finns på [med Windows PowerShell-skript](http://technet.microsoft.com/library/bb978526.aspx). 

### <a name="powershell-expression-data-source"></a>Datakälla för PowerShell-uttryck
Du kan använda ett PowerShell-uttryck som källa toopopulate hello värdet ett [Aktivitetsparametern](#activities) med PowerShell kod hello resultat.  Detta kan vara en rad med kod som utför en enkel funktion eller flera rader som utför vissa komplex logik.  Alla utdata från ett kommando som inte är tilldelade tooa variabeln är utdata toohello parametervärdet. 

Hello följande kommando skulle till exempel spara hello aktuellt datum. 

    Get-Date

hello följande kommandon och skapa en sträng från hello aktuellt datum och tilldela den tooa variabeln.  hello innehållet i hello variabeln skickas sedan toohello utdata 

    $string = "hello current date is " + (Get-Date)
    $string

hello följande kommandon och utvärdera hello aktuellt datum och returnerar en sträng som anger om hello aktuell dag är en helg eller veckodag. 

    $date = Get-Date
    if (($date.DayOfWeek = "Saturday") -or ($date.DayOfWeek = "Sunday")) { "Weekend" }
    else { "Weekday" }


### <a name="activity-output"></a>Aktivitetsutdata
toouse hello utdata från en tidigare aktivitet i hello runbook, Använd hello $ActivityOutput variabel med hello följande syntax.

    $ActivityOutput['Activity Label'].PropertyName

Du kan till exempel ha en aktivitet med en egenskap som kräver hello namnet på en virtuell dator i så fall kan du använda hello efter uttryck.

    $ActivityOutput['Get-AzureVm'].Name

Om hello-egenskap som krävs för hello virtuella datorobjekt i stället för bara en egenskap och du vill returnera hello hello hela objektet med hjälp av följande syntax.

    $ActivityOutput['Get-AzureVm']

Du kan också använda hello utdata för en aktivitet i ett mer komplext uttryck, till exempel hello följande som sammanfogar text toohello virtuellt datornamn.

    "hello computer name is " + $ActivityOutput['Get-AzureVm'].Name


### <a name="conditions"></a>Villkor
Använd [jämförelseoperatorer](https://technet.microsoft.com/library/hh847759.aspx) toocompare värden eller ta reda på om ett värde som matchar ett specifikt mönster.  En jämförelse returnerar värdet $true eller $false.

Till exempel följande villkor hello avgör om hello virtuell dator från en aktivitet med namnet *Get-AzureVM* är för närvarande *stoppats*. 

    $ActivityOutput["Get-AzureVM"].PowerState –eq "Stopped"

hello följande villkor kontrollerar om hello samma virtuella dator finns i några tillstånd än *stoppats*.

    $ActivityOutput["Get-AzureVM"].PowerState –ne "Stopped"

Du kan ansluta till flera villkor med hjälp av en [logisk operator](https://technet.microsoft.com/library/hh847789.aspx) som **- och** eller **- eller**.  Till exempel hello följande villkor kontrollerar om hello samma virtuella dator i hello föregående exempel är i tillståndet *stoppats* eller *stoppar*.

    ($ActivityOutput["Get-AzureVM"].PowerState –eq "Stopped") -or ($ActivityOutput["Get-AzureVM"].PowerState –eq "Stopping") 


### <a name="hashtables"></a>Hashtabeller
[Hashtabeller](http://technet.microsoft.com/library/hh847780.aspx) är namn/värde-par som är användbara för att returnera en uppsättning värden.  Egenskaper för vissa aktiviteter kan förvänta dig en hash-tabell i stället för ett enkelt värde.  Du kan också se enligt hashtable tooas en ordlista. 

Du kan skapa en hash-tabell med följande syntax hello.  En hash-tabell kan innehålla valfritt antal poster men varje definieras av namn och värde.

    @{ <name> = <value>; [<name> = <value> ] ...}

Till exempel skapar hello följande uttryck en hash-tabell toobe som används i hello datakälla för en aktivitetsparameter som förväntas en hash-tabell med värden för en Internetsökning.

    $query = "Azure Automation"
    $count = 10
    $h = @{'q'=$query; 'lr'='lang_ja';  'count'=$Count}
    $h

hello följande exempel används utdata från en aktivitet som kallas *hämta Twitter-anslutningen* toopopulate en hash-tabell.

    @{'ApiKey'=$ActivityOutput['Get Twitter Connection'].ConsumerAPIKey;
      'ApiSecret'=$ActivityOutput['Get Twitter Connection'].ConsumerAPISecret;
      'AccessToken'=$ActivityOutput['Get Twitter Connection'].AccessToken;
      'AccessTokenSecret'=$ActivityOutput['Get Twitter Connection'].AccessTokenSecret}



## <a name="next-steps"></a>Nästa steg
* tooget igång med PowerShell arbetsflöde runbooks finns [min första PowerShell-arbetsflödesrunbook](automation-first-runbook-textual.md) 
* tooget igång med grafiska runbooks finns [min första grafisk runbook](automation-first-runbook-graphical.md)
* tooknow mer om runbook-typer, fördelar och begränsningar finns [typer för Azure Automation-runbook](automation-runbook-types.md)
* toounderstand hur tooauthenticate med hello Automation kör som-konto finns [konfigurera Azure kör som-konto](automation-sec-configure-azure-runas-account.md)

