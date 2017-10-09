---
title: aaaStarting en Azure Automation-runbook med en webhook | Microsoft Docs
description: "En webhook som tillåter en klient toostart en runbook i Azure Automation från ett HTTP-anrop.  Den här artikeln beskriver hur toocreate en webhook och hur toocall en toostart en runbook."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 9b20237c-a593-4299-bbdc-35c47ee9e55d
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: magoedte;bwren;sngun
ms.openlocfilehash: ca6cde66b3784ceb5d0bc5921cee87aea74cb150
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="starting-an-azure-automation-runbook-with-a-webhook"></a>Starta en Azure Automation-runbook med en webhook
En *webhook* kan du toostart en viss runbook i Azure Automation via en HTTP-begäran. Detta gör externa tjänster, till exempel Visual Studio Team Services, GitHub, logganalys för Microsoft Operations Management Suite eller anpassade program toostart runbooks utan att implementera en fullständig lösning med hjälp av hello Azure Automation-API.  
![WebhooksOverview](media/automation-webhooks/webhook-overview-image.png)

Du kan jämföra webhooks tooother metoder för att starta en runbook [starta en runbook i Azure Automation](automation-starting-a-runbook.md)

## <a name="details-of-a-webhook"></a>Information om en webhook
hello beskrivs följande tabell hello egenskaper som du måste konfigurera för en webhook.

| Egenskap | Beskrivning |
|:--- |:--- |
| Namn |Du kan ange ett namn som du vill använda för en webhook eftersom det är inte visas toohello klienten.  Den används endast för du tooidentify hello runbook i Azure Automation. <br>  Som bästa praxis bör ge du hello webhook en namnet relaterade toohello klient som använder den. |
| URL: EN |hello-URL för hello webhook är hello unik adress att en klient anropar med en HTTP POST toostart hello runbook länkade toohello webhooken.  Den genereras automatiskt när du skapar hello webhooken.  Du kan inte ange en anpassad URL. <br> <br>  hello Webbadressen innehåller en säkerhetstoken som gör hello runbook toobe anropas av en tredjeparts-system utan ytterligare autentisering. Det bör därför behandlas som ett lösenord.  Av säkerhetsskäl kan du endast visa hello URL: en i hello Azure-portalen på hello tid hello webhook har skapats. Du bör anteckna hello URL i en säker plats för framtida användning. |
| Förfallodatum |Varje webhook har ett sista giltighetsdatum som den kan inte längre användas som ett certifikat.  Den här upphör att gälla kan ändras när hello webhook har skapats. |
| Enabled |En webhook är aktiverad som standard när den skapas.  Om du anger det tooDisabled kommer ingen klient kommer att kunna toouse den.  Du kan ange hello **aktiverad** egenskapen när du skapar hello webhook eller när som helst när den skapas. |

### <a name="parameters"></a>Parametrar
En webhook kan definiera värden för runbookparametrar som används när hello runbook startas genom att webhooken. Hej webhook måste innehålla värden för alla obligatoriska parametrar för hello runbook och kan innehålla värden för valfria parametrar. En parameter värdet som konfigurerats tooa webhook kan ändras när hello webhoook. Flera webhooks länkade tooa enstaka runbook kan använda olika parametervärden.

Det kan inte åsidosätta hello parametervärden som definierats i hello webhook när en klient startar en runbook med en webhook.  tooreceive data från hello klient, hello runbook kan acceptera en enda parameter med namnet **$WebhookData** av typen [objekt] som innehåller data som innehåller den hello-klienten i hello POST-begäran.

![Webhookdata egenskaper](media/automation-webhooks/webhook-data-properties.png)

Hej **$WebhookData** objekt har hello följande egenskaper:

| Egenskap | Beskrivning |
|:--- |:--- |
| WebhookName |hello namnet på webhook hello. |
| RequestHeader |Hash-tabell som innehåller hello-huvuden hello inkommande POST-begäran. |
| requestBody |hello brödtext hello inkommande POST-begäran.  Detta behåller all formatering, till exempel sträng, JSON, XML, eller formulär-kodade data. Hej runbook måste skrivas toowork med hello dataformat som förväntas. |

Det finns ingen konfiguration av hello webhook krävs toosupport hello **$WebhookData** parameter, och hello runbook är inte obligatoriska tooaccept den.  Om hello runbook inte definierar hello parametern ignoreras någon information om hello-begäran som skickades från hello-klient.

Om du anger ett värde för $WebhookData när du skapar hello webhook ska värdet åsidosättas när hello webhook startar hello runbook med hello data från hello klienten POST-begäran, även om hello klienten inte innehåller några data i begärandetexten för hello.  Om du startar en runbook med hjälp av en annan metod än en webhook $WebhookData, kan du ange ett värde för $Webhookdata som identifieras av hello runbook.  Det här värdet ska vara ett objekt med hello samma [egenskaper](#details-of-a-webhook) som $Webhookdata så att hello runbook korrekt kan arbeta med det som om den arbetade med faktiska WebhookData skickades av en webhook.

Till exempel om du startar hello följande runbook från hello Azure-portalen och vill toopass några exempel på WebhookData för att testa, eftersom WebhookData är ett objekt som ska den skickas som JSON i hello Användargränssnittet.

![WebhookData-parameter från Gränssnittet](media/automation-webhooks/WebhookData-parameter-from-UI.png)

För hello ovan runbook, om du har följande egenskaper för hello WebhookData parametern hello:

1. WebhookName: *MyWebhook*
2. RequestHeader: *från = testanvändare*
3. RequestBody: *[”VM1”, ”VM2”]*

Du skulle sedan överföra hello följande JSON-värde i hello Användargränssnittet för hello WebhookData parameter:  

* {”WebhookName”: ”MyWebhook”, ”RequestHeader”: {”från”: ”testanvändare”}, ”RequestBody” ”: [\"VM1\",\"VM2\"]”}

![Starta WebhookData parametern från Gränssnittet](media/automation-webhooks/Start-WebhookData-parameter-from-UI.png)

> [!NOTE]
> hello värdena för alla indataparametrar loggas med hello runbook-jobbet.  Detta innebär att alla indata som angetts av hello-klienten i hello webhook begäran kommer att loggas och tillgängliga tooanyone med åtkomst toohello automation-jobb.  Därför bör du vara försiktig om känslig information, inklusive i webhook-anrop.
>

## <a name="security"></a>Säkerhet
hello säkerheten för en webhook är beroende av hello sekretessen för URL: en som innehåller en säkerhetstoken som gör det toobe anropas. Azure Automation utför inte någon autentisering på hello begäran så länge det görs toohello rätt URL. Därför användas webhooks inte för runbooks som utför mycket känsliga funktioner utan att använda ett annat sätt att verifiera hello-begäran.

Du kan inkludera logik i hello runbook toodetermine att den anropades av en webhook genom att kontrollera hello **WebhookName** -egenskapen för hello $WebhookData-parametern. Hej runbook kan utföra ytterligare verifiering genom att söka efter en viss information i hello **RequestHeader** eller **RequestBody** egenskaper.

En annan strategi är toohave hello runbook utför vissa verifiering av ett externt villkor när det tog emot en webhook-begäran.  Anta till exempel att en runbook som anropas av GitHub när det finns en ny commit tooa GitHub-lagringsplats.  Hej runbook kan ansluta tooGitHub toovalidate som ett nytt genomförande inträffat faktiskt precis innan du fortsätter.

## <a name="creating-a-webhook"></a>Skapat en webhook
Använd hello följa proceduren toocreate en ny webhook länkade tooa runbook i hello Azure-portalen.

1. Från hello **Runbooks blad** i hello Azure-portalen klickar du på hello-runbook som hello webhook startar tooview dess detaljer-bladet.
2. Klicka på **Webhook** hello överst i hello bladet tooopen hello **lägga till Webhook** bladet. <br>
   ![Knappen Webhooks](media/automation-webhooks/webhooks-button.png)
3. Klicka på **skapa nya webhook** tooopen hello **skapa webhook-bladet**.
4. Ange en **namn**, **förfallodatum** för hello webhook och om den ska aktiveras. Se [information om en webhook](#details-of-a-webhook) mer dessa egenskaper.
5. Klicka på Kopiera-ikonen hello och tryck på Ctrl + C toocopy hello URL för hello webhook.  Registrera den på en säker plats.  **När du skapar hello webhook kan hämta du inte hello URL igen.** <br>
   ![Webhooksadressen](media/automation-webhooks/copy-webhook-url.png)
6. Klicka på **parametrar** tooprovide värden för hello runbook-parametrar.  Om hello runbook har obligatoriska parametrar kan blir du inte kan toocreate hello webhook om värden har angetts.
7. Klicka på **skapa** toocreate hello webhooken.

## <a name="using-a-webhook"></a>Med hjälp av en webhook
toouse en webhook när den har skapats, ditt klientprogram utfärda en HTTP POST med hello URL för hello webhooken.  hello syntaxen för hello webhook har hello följande format.

    http://<Webhook Server>/token?=<Token Value>

hello klienten får en hello följande returkoder från hello POST-begäran.  

| Kod | Text | Beskrivning |
|:--- |:--- |:--- |
| 202 |Godkänt |hello-begäran har godkänts och hello runbook placerades i kö. |
| 400 |Felaktig begäran |hello begäran accepterades inte av något av följande orsaker hello. <ul> <li>Hej webhook har gått ut.</li> <li>Hej webhook har inaktiverats.</li> <li>hello-token i hello URL: en är ogiltig.</li>  </ul> |
| 404 |Det gick inte att hitta |hello begäran accepterades inte av något av följande orsaker hello. <ul> <li>Hej webhook hittades inte.</li> <li>Hej runbook hittades inte.</li> <li>hello-konto hittades inte.</li>  </ul> |
| 500 |Internt serverfel |hello URL är giltig, men ett fel uppstod.  Skicka hello begäran igen. |

Under förutsättning att hello begäran lyckas, hello webhook svaret innehåller hello jobb-id i JSON-format på följande sätt. Den innehåller en enda jobb-id, men hello JSON-format kan för eventuella framtida förbättringar.

    {"JobIds":["<JobId>"]}  

hello-klienten kan inte fastställa när hello runbook-jobbet har slutförts eller dess status för slutförande från hello webhooken.  Det kan avgöra den här informationen med hello jobb-id med en annan metod, till exempel [Windows PowerShell](http://msdn.microsoft.com/library/azure/dn690263.aspx) eller hello [Azure Automation API](https://msdn.microsoft.com/library/azure/mt163826.aspx).

### <a name="example"></a>Exempel
hello följande exempel använder Windows PowerShell toostart en runbook med en webhook.  Observera att alla språk som kan göra en HTTP-begäran kan använda en webhook; Windows PowerShell används bara exempel.

Hej runbook förväntar sig en lista över virtuella datorer som har formaterats i JSON i hello brödtext hello-begäran. Vi också inklusive information om som startar hello runbook och hello datum och tid startas i hello-rubriken för hello-begäran.      

    $uri = "https://s1events.azure-automation.net/webhooks?token=8ud0dSrSo%2fvHWpYbklW%3c8s0GrOKJZ9Nr7zqcS%2bIQr4c%3d"
    $headers = @{"From"="user@contoso.com";"Date"="05/28/2015 15:47:00"}

    $vms  = @(
                @{ Name="vm01";ServiceName="vm01"},
                @{ Name="vm02";ServiceName="vm02"}
            )
    $body = ConvertTo-Json -InputObject $vms

    $response = Invoke-RestMethod -Method Post -Uri $uri -Headers $headers -Body $body
    $jobid = ConvertFrom-Json $response


hello följande bild visar hello rubrikinformation (med hjälp av en [Fiddler](http://www.telerik.com/fiddler) trace) från den här begäran. Detta inkluderar standard sidhuvuden för HTTP-begäran i tillägg toohello anpassade datum och från-huvuden som vi har lagt till.  Var och en av dessa värden är tillgänglig toohello runbook i hello **RequestHeaders** -egenskapen för **WebhookData**.

![Knappen Webhooks](media/automation-webhooks/webhook-request-headers.png)

hello följande bild visar hello brödtexten i begäran hello (med hjälp av en [Fiddler](http://www.telerik.com/fiddler) trace) som är tillgängliga toohello runbook i hello **RequestBody** -egenskapen för **WebhookData**. Detta formateras som JSON eftersom som var hello-format som ingick i hello brödtext hello-begäran.     

![Knappen Webhooks](media/automation-webhooks/webhook-request-body.png)

hello visar följande bild hello-begäran som skickas från Windows PowerShell och hello resulterande svar.  hello jobb-id som extraheras från hello svar och konverterade tooa sträng.

![Knappen Webhooks](media/automation-webhooks/webhook-request-response.png)

hello följande exempel-runbook accepterar hello föregående exempelbegäran och startar hello virtuella datorer som anges i begärandetexten för hello.

    workflow Test-StartVirtualMachinesFromWebhook
    {
        param (
            [object]$WebhookData
        )

        # If runbook was called from Webhook, WebhookData will not be null.
        if ($WebhookData -ne $null) {

            # Collect properties of WebhookData
            $WebhookName     =     $WebhookData.WebhookName
            $WebhookHeaders =     $WebhookData.RequestHeader
            $WebhookBody     =     $WebhookData.RequestBody

            # Collect individual headers. VMList converted from JSON.
            $From = $WebhookHeaders.From
            $VMList = ConvertFrom-Json -InputObject $WebhookBody
            Write-Output "Runbook started from webhook $WebhookName by $From."

            # Authenticate tooAzure resources
            $Cred = Get-AutomationPSCredential -Name 'MyAzureCredential'
            Add-AzureAccount -Credential $Cred

            # Start each virtual machine
            foreach ($VM in $VMList)
            {
                $VMName = $VM.Name
                Write-Output "Starting $VMName"
                Start-AzureVM -Name $VM.Name -ServiceName $VM.ServiceName
            }
        }
        else {
            Write-Error "Runbook mean toobe started only from webhook."
        }
    }


## <a name="starting-runbooks-in-response-tooazure-alerts"></a>Starta runbooks i svaret tooAzure aviseringar
Webhook-aktiverade runbooks kan vara används tooreact för[Azure aviseringar](../monitoring-and-diagnostics/insights-receive-alert-notifications.md). Resurser i Azure kan övervakas genom att samla in hello statistik som prestanda, tillgänglighet och användningsinformation med hello hjälp av Azure-aviseringar. Du kan ta emot en avisering baserat på övervakning mått eller händelser för din Azure-resurser, för närvarande Automation-konton stöder endast mått. När hello-värdet för ett visst mått överskrider hello tröskelvärdet tilldelade eller om hello konfigurerats händelsen utlöses och sedan skickas ett meddelande toohello administratören eller medadministratörer tooresolve hello varning service, mer information om mått och händelser finns för[ Azure-aviseringar](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

Förutom att använda Azure-aviseringar som ett system, kan du också startar runbooks i svaret tooalerts. Azure Automation ger hello kapaciteten toorun webhook-aktiverade runbooks med Azure-aviseringar. När ett mått överstiger konfigurerad hello tröskelvärdet hello varningsregeln blir aktiv och utlösare hello automation-webhook som i sin tur utför hello runbook.

![Webhooks](media/automation-webhooks/webhook-alert.jpg)

### <a name="alert-context"></a>Varningskontext
Överväg att en Azure-resurs, till exempel en virtuell dator, CPU-användning för den här datorn är en hello KPI mått. Om hello CPU-användning 100% eller mer än en viss mängd för lång tidsperiod, kanske du vill toorestart hello virtuella toofix hello problem. Detta kan lösas genom att konfigurera en regel för varning toohello virtuell dator och den här regeln tar CPU-procent som dess mått. CPU-procent här tas bara ett exempel men det finns många andra mått som du kan konfigurera tooyour Azure resurser och startar om hello virtuella datorn är en åtgärd som har vidtagits toofix det här problemet kan du konfigurera hello runbook tootake andra åtgärder.

När regeln hello avisering aktiveras och utlösare hello webhook-aktiverade runbook, skickar hello aviseringskontext toohello runbook. [Aviseringskontext](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) innehåller information, inklusive **SubscriptionID**, **ResourceGroupName**, **ResourceName**, **ResourceType**, **ResourceId** och **tidsstämpel** som krävs för hello runbook tooidentify hello resursen som den ska vidta åtgärder. Varna kontext är inbäddad i hello huvuddelen av hello **WebhookData** objektet skickade toohello runbook och den kan användas med **Webhook.RequestBody** egenskapen

### <a name="example"></a>Exempel
Skapa en virtuell Azure-dator i din prenumeration och koppla en [Varna toomonitor CPU-procent mått](../monitoring-and-diagnostics/insights-receive-alert-notifications.md). Kontrollera att du fylla i fältet för hello webhook med hello URL hello webhook som skapades när du skapar hello webhook när du skapar hello avisering.

hello utlöses följande exempel-runbook när hello varningsregeln blir aktiv och samlar det in hello aviseringskontext parametrar som krävs för hello runbook tooidentify hello resursen som den ska vidta åtgärder.

    workflow Invoke-RunbookUsingAlerts
    {
        param (      
            [object]$WebhookData
        )

        # If runbook was called from Webhook, WebhookData will not be null.
        if ($WebhookData -ne $null) {   
            # Collect properties of WebhookData.
            $WebhookName    =   $WebhookData.WebhookName
            $WebhookBody    =   $WebhookData.RequestBody
            $WebhookHeaders =   $WebhookData.RequestHeader

            # Outputs information on hello webhook name that called This
            Write-Output "This runbook was started from webhook $WebhookName."


            # Obtain hello WebhookBody containing hello AlertContext
            $WebhookBody = (ConvertFrom-Json -InputObject $WebhookBody)
            Write-Output "`nWEBHOOK BODY"
            Write-Output "============="
            Write-Output $WebhookBody

            # Obtain hello AlertContext     
            $AlertContext = [object]$WebhookBody.context

            # Some selected AlertContext information
            Write-Output "`nALERT CONTEXT DATA"
            Write-Output "==================="
            Write-Output $AlertContext.name
            Write-Output $AlertContext.subscriptionId
            Write-Output $AlertContext.resourceGroupName
            Write-Output $AlertContext.resourceName
            Write-Output $AlertContext.resourceType
            Write-Output $AlertContext.resourceId
            Write-Output $AlertContext.timestamp

            # Act on hello AlertContext data, in our case restarting hello VM.
            # Authenticate tooyour Azure subscription using Organization ID toobe able toorestart that Virtual Machine.
            $cred = Get-AutomationPSCredential -Name "MyAzureCredential"
            Add-AzureAccount -Credential $cred
            Select-AzureSubscription -subscriptionName "Visual Studio Ultimate with MSDN"

            #Check hello status property of hello VM
            Write-Output "Status of VM before taking action"
            Get-AzureVM -Name $AlertContext.resourceName -ServiceName $AlertContext.resourceName
            Write-Output "Restarting VM"

            # Restart hello VM by passing VM name and Service name which are same in this case
            Restart-AzureVM -ServiceName $AlertContext.resourceName -Name $AlertContext.resourceName
            Write-Output "Status of VM after alert is active and takes action"
            Get-AzureVM -Name $AlertContext.resourceName -ServiceName $AlertContext.resourceName
        }
        else  
        {
            Write-Error "This runbook is meant tooonly be started from a webhook."  
        }  
    }



## <a name="next-steps"></a>Nästa steg
* Mer information om olika sätt toostart en runbook finns [starta en Runbook](automation-starting-a-runbook.md).
* Information om visning hello Status för en Runbook-jobb finns för[Runbook som körs i Azure Automation](automation-runbook-execution.md).
* hur toouse Azure Automation tootake åtgärd på Azure aviseringar, se toolearn [åtgärda Azure VM aviseringar med Automation-Runbooks](automation-azure-vm-alert-integration.md).
