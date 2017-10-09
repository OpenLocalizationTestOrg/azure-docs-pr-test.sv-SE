---
title: "aaa ”åtgärda Azure VM aviseringar med Automation-Runbooks | Microsoft Docs ”"
description: "Den här artikeln visar hur toointegrate Azure-dator aviseringar med Azure Automation-runbooks och åtgärda problem automatiskt"
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 1f7baa7f-7283-4a4f-9385-3f5cd1062c7f
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/14/2016
ms.author: csand;magoedte
ms.openlocfilehash: c226368a5c4c51fbfb331f4b97f7f2f239e701c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-scenario---remediate-azure-vm-alerts"></a>Azure Automation-scenario – åtgärda Virtuella Azure-aviseringar
Azure Automation och Azure virtuella datorer har släppt en ny funktion som gör att du tooconfigure virtuell dator (VM) aviseringar toorun Automation-runbooks. Den här nya funktionen kan du tooautomatically utför standard reparation i svaret tooVM aviseringar, som startar om eller stoppa hello VM.

Tidigare under skapande av VM varningsregeln kunde för[ange en Automation-webhook](https://azure.microsoft.com/blog/using-azure-automation-to-take-actions-on-azure-alerts/) tooa runbook i ordning toorun hello runbook när hello avisering utlöses. Det måste dock du toodo hello arbetet med att skapa hello runbook, skapa hello webhook för hello runbook och sedan kopiera och klistra in hello webhook när regeln skapades. Hello processen är mycket enklare med den här nya versionen eftersom du kan välja en runbook från en lista direkt under skapande av varning och du kan välja ett Automation-konto som ska köra hello runbook eller enkelt skapa ett konto.

I den här artikeln ska vi visa hur lätt det är tooset upp en virtuell dator i Azure-avisering och konfigurera toorun ett Automation-runbook när hello avisering utlöser. Exempelscenarier är att starta om en virtuell dator när hello minnesanvändning överskrider tröskelvärdet för förfallodatum tooan programmet på hello VM med en minnesläcka eller stoppa en virtuell dator när hello CPU användartid har mindre än 1% för senaste timmen och används inte. Vi förklarar också hur hello automatiskt skapande av ett huvudnamn för tjänsten i ditt Automation förenklar konto hello användningen av runbooks i Azure avisering reparation.

## <a name="create-an-alert-on-a-vm"></a>Skapa en avisering på en virtuell dator
Utföra hello efter steg tooconfigure en avisering toolaunch en runbook när dess tröskelvärde har uppfyllts.

> [!NOTE]
> Med den här versionen stöder vi bara V2 virtuella datorer och stöd för klassiska virtuella datorer läggs snart.  
> 
> 

1. Logga in toohello Azure-portalen och klicka på **virtuella datorer**.  
2. Välj någon av de virtuella datorerna.  hello virtuella instrumentpanelen bladet ska visas och hello **inställningar** bladet tooits höger.  
3. Från hello **inställningar** bladet under hello övervakning Välj **Varna regler**.
4. På hello **Varna regler** bladet, klickar du på **Lägg till avisering**.

Detta öppnar hello **lägga till en varningsregel** bladet, där du kan konfigurera hello villkor för hello aviseringen och välja mellan en eller flera av följande alternativ: skicka e-toosomeone använder ett webhook tooforward hello avisering tooanother system och/eller Kör en Automation-runbook i svaret försök tooremediate hello problemet.

## <a name="configure-a-runbook"></a>Konfigurera en runbook
tooconfigure toorun en runbook när hello VM tröskelvärdena är uppfyllt, Välj **Automation-Runbook**. I hello **konfigurerar runbook** bladet kan du välja hello runbook toorun och hello Automation-konto toorun hello runbook i.

![Konfigurera en Automation-runbook och skapa ett nytt Automation-konto](media/automation-azure-vm-alert-integration/ConfigureRunbookNewAccount.png)

> [!NOTE]
> Den här versionen som du kan välja mellan tre runbooks som ger hello-tjänsten – starta om VM, stoppa VM eller ta bort virtuell dator (ta bort det).  Hej möjlighet tooselect andra runbooks eller en av dina egna runbooks blir tillgänglig i framtida versioner.
> 
> 

![Runbooks toochoose från](media/automation-azure-vm-alert-integration/RunbooksToChoose.png)

När du har valt en hello tre tillgängliga runbooks hello **Automation-konto** listrutan visas och du kan välja ett automation konto hello runbook körs i. Runbooks måste toorun hello kontexten för en [Automation-konto](automation-security-overview.md) som finns i din Azure-prenumeration. Du kan välja ett Automation-konto som du redan skapat eller du kan ha ett nytt Automation-konto för dig.

Hej runbooks som tillhandahålls autentisera tooAzure med ett huvudnamn för tjänsten. Om du väljer toorun hello runbook i en av dina befintliga Automation-konton, skapas vi automatiskt hello service principal för dig. Om du väljer toocreate ett nytt Automation-konto kan sedan skapar automatiskt vi hello-konto och hello tjänstens huvudnamn. I båda fallen två tillgångar skapas också i hello Automation-konto – ett med namnet certifikattillgång **AzureRunAsCertificate** och en anslutningstillgång med namnet **AzureRunAsConnection**. Hej runbooks använder **AzureRunAsConnection** tooauthenticate med Azure i ordning tooperform hello management-åtgärd mot hello VM.

> [!NOTE]
> hello tjänstens huvudnamn skapas i hello prenumerationsomfattningen och hello deltagarrollen tilldelas. Den här rollen krävs för hello konto toohave behörighet toorun Automation runbooks toomanage virtuella Azure-datorer.  hello skapandet av ett Automaton konto eller tjänstens huvudnamn är en enstaka händelse. När de skapas, kan du använda detta konto toorun runbooks för andra Virtuella Azure-aviseringar.
> 
> 

När du klickar på **OK** hello aviseringen har konfigurerats och om du har valt hello alternativet toocreate ett nytt Automation-konto skapas tillsammans med hello tjänstens huvudnamn.  Det kan ta några sekunder toocomplete.  

![Runbook som konfigureras](media/automation-azure-vm-alert-integration/RunbookBeingConfigured.png)

När hello konfigurationen har slutförts visas hello namnet på hello runbook visas i hello **lägga till en varningsregel** bladet.

![Runbook har konfigurerats](media/automation-azure-vm-alert-integration/RunbookConfigured.png)

Klicka på **OK** i hello **lägga till en varningsregel** bladet och hello varningsregeln ska skapas och aktivera om hello virtuella datorn är i körningstillstånd.

### <a name="enable-or-disable-a-runbook"></a>Aktivera eller inaktivera en runbook
Om du har en runbook som konfigurerats för en avisering kan inaktivera du det utan att ta bort hello konfiguration av runbook. Detta kan du tookeep hello avisering körs och testa kanske vissa av hello Varningsregler och sedan återaktivera senare hello runbook.

## <a name="create-a-runbook-that-works-with-an-azure-alert"></a>Skapa en runbook som fungerar med en Azure avisering
När du väljer en runbook som en del av en Azure aviseringsregel måste hello runbook toohave logiken i den toomanage hello avisering data som överförs tooit.  När en runbook är konfigurerad i en aviseringsregel, har en webhook skapats för hello runbook; Denna webhook är och sedan använda toostart hello runbook varje gång hello avisering utlösare.  hello faktiska anropet toostart hello runbook är en HTTP POST-begäran toohello Webhooksadressen. hello innehåller hello POST-begäran ett JSON-formaterad objekt som innehåller användbara egenskaper relaterade toohello avisering.  Som du ser nedan innehåller hello aviseringsdata detaljer som subscriptionID, resourceGroupName, resourceName och resourceType.

### <a name="example-of-alert-data"></a>Exempel på aviseringsdata
```
{
    "WebhookName": "AzureAlertTest",
    "RequestBody": "{
    \"status\":\"Activated\",
    \"context\": {
        \"id\":\"/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/microsoft.insights/alertrules/AlertTest\",
        \"name\":\"AlertTest\",
        \"description\":\"\",
        \"condition\": {
            \"metricName\":\"CPU percentage guest OS\",
            \"metricUnit\":\"Percent\",
            \"metricValue\":\"4.26337916666667\",
            \"threshold\":\"1\",
            \"windowSize\":\"60\",
            \"timeAggregation\":\"Average\",
            \"operator\":\"GreaterThan\"},
        \"subscriptionId\":\<subscriptionID> \",
        \"resourceGroupName\":\"TestResourceGroup\",
        \"timestamp\":\"2016-04-24T23:19:50.1440170Z\",
        \"resourceName\":\"TestVM\",
        \"resourceType\":\"microsoft.compute/virtualmachines\",
        \"resourceRegion\":\"westus\",
        \"resourceId\":\"/subscriptions/<subscriptionId>/resourceGroups/TestResourceGroup/providers/Microsoft.Compute/virtualMachines/TestVM\",
        \"portalLink\":\"https://portal.azure.com/#resource/subscriptions/<subscriptionId>/resourceGroups/TestResourceGroup/providers/Microsoft.Compute/virtualMachines/TestVM\"
        },
    \"properties\":{}
    }",
    "RequestHeader": {
        "Connection": "Keep-Alive",
        "Host": "<webhookURL>"
    }
}
```

När hello Automation-webhook-tjänst tar emot hello HTTP POST extraherar hello aviseringsdata och skickar den toohello runbook i hello WebhookData runbookinmatningsparameter.  Nedan finns en exempel-runbook som visar hur toouse hello WebhookData-parametern och extrahera hello aviseringsdata och använda den toomanage hello Azure-resurs som utlöste aviseringen hello.

### <a name="example-runbook"></a>Exempel-runbook
```
#  This runbook will restart an ARM (V2) VM in response tooan Azure VM alert.

[OutputType("PSAzureOperationResponse")]

param ( [object] $WebhookData )

if ($WebhookData)
{
    # Get hello data object from WebhookData
    $WebhookBody = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

    # Assure that hello alert status is 'Activated' (alert condition went from false tootrue)
    # and not 'Resolved' (alert condition went from true toofalse)
    if ($WebhookBody.status -eq "Activated")
    {
        # Get hello info needed tooidentify hello VM
        $AlertContext = [object] $WebhookBody.context
        $ResourceName = $AlertContext.resourceName
        $ResourceType = $AlertContext.resourceType
        $ResourceGroupName = $AlertContext.resourceGroupName
        $SubId = $AlertContext.subscriptionId

        # Assure that this is hello expected resource type
        Write-Verbose "ResourceType: $ResourceType"
        if ($ResourceType -eq "microsoft.compute/virtualmachines")
        {
            # This is an ARM (V2) VM

            # Authenticate tooAzure with service principal and certificate
            $ConnectionAssetName = "AzureRunAsConnection"
            $Conn = Get-AutomationConnection -Name $ConnectionAssetName
            if ($Conn -eq $null) {
                throw "Could not retrieve connection asset: $ConnectionAssetName. Check that this asset exists in hello Automation account."
            }
            Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint | Write-Verbose
            Set-AzureRmContext -SubscriptionId $SubId -ErrorAction Stop | Write-Verbose

            # Restart hello VM
            Restart-AzureRmVM -Name $ResourceName -ResourceGroupName $ResourceGroupName
        } else {
            Write-Error "$ResourceType is not a supported resource type for this runbook."
        }
    } else {
        # hello alert status was not 'Activated' so no action taken
        Write-Verbose ("No action taken. Alert status: " + $WebhookBody.status)
    }
} else {
    Write-Error "This runbook is meant toobe started from an Azure alert only."
}
```

## <a name="summary"></a>Sammanfattning
När du konfigurerar en avisering på en Azure VM, nu har du hello möjlighet tooeasily konfigurera en Automation runbook tooautomatically utföra Reparationsåtgärd när hello avisering utlöses. För den här versionen, du kan välja bland runbooks toorestart stoppa eller ta bort en virtuell dator beroende på aviseringen scenario. Detta är bara hello början för att aktivera scenarier där du kan bestämma hello åtgärder (meddelande, felsökning, reparation) som ska vidtas automatiskt när en avisering utlöses.

## <a name="next-steps"></a>Nästa steg
* tooget igång med grafiska runbooks finns [min första grafisk runbook](automation-first-runbook-graphical.md)
* tooget igång med PowerShell arbetsflöde runbooks finns [min första PowerShell-arbetsflödesrunbook](automation-first-runbook-textual.md)
* toolearn mer om runbook-typer, fördelar och begränsningar finns [typer för Azure Automation-runbook](automation-runbook-types.md)

