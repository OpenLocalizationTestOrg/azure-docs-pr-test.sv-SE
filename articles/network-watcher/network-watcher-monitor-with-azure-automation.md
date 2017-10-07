---
title: "aaaMonitor VPN-gatewayer Azure Nätverksbevakaren felsökning | Microsoft Docs"
description: "Den här artikeln beskriver hur diagnostisera lokal anslutning med Azure Automation och Nätverksbevakaren"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: a607d0c862ea1be63c687717f0c5dc137db58a43
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-vpn-gateways-with-network-watcher-troubleshooting"></a>Övervaka VPN-gatewayer Nätverksbevakaren felsökning

Får du djupa insikter på nätverkets prestanda är kritiska tooprovide reliable services toocustomers. Det är därför viktiga toodetect avbrott nätverksförhållanden snabbt och vidta åtgärder toomitigate hello avbrott villkor. Azure Automation kan du tooimplement och köra en aktivitet i ett programmässiga sätt via runbooks. Med hjälp av Azure Automation skapas ett perfekt recept för att utföra kontinuerlig och proaktiv övervakning och avisering.

## <a name="scenario"></a>Scenario

hello scenariot i hello följande bild är ett program som flera nivåer med lokal anslutning upprättas med hjälp av en VPN-Gateway och -tunnel. Säkerställa hello VPN-Gateway är igång och körs är kritiska toohello program prestanda.

En runbook skapas med ett skript toocheck för anslutningsstatus hello VPN-tunnel, med hjälp av hello resurs felsökning API toocheck för tunnel anslutningsstatus. Om hello status inte är felfri, skickas en e-utlösare tooadministrators.

![Exempel på ett scenario][scenario]

Det här scenariot kommer:

- Skapa en runbook anropa hello `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet tootroubleshoot anslutningsstatus
- Länka ett schema toohello runbook

## <a name="before-you-begin"></a>Innan du börjar

Innan du startar det här scenariot måste du ha hello följande förutsättningar:

- Ett Azure automation-konto i Azure. Kontrollera att hello automation-konto har senaste hello-moduler och har även hello AzureRM.Network modul. Hej AzureRM.Network-modulen är tillgänglig i hello modulgalleriet om du behöver tooadd den tooyour automation-konto.
- Du måste ha en uppsättning autentiseringsuppgifter konfigurera i Azure Automation. Mer information finns i [Azure Automation-säkerhet](../automation/automation-security-overview.md)
- En giltig SMTP-server (Office 365, din lokala e-post eller en annan) och autentiseringsuppgifter som definierats i Azure Automation
- En konfigurerade virtuella nätverksgateway i Azure.
- Ett befintligt lagringskonto med en befintlig behållare toostore hello loggar in.

> [!NOTE]
> hello-infrastruktur som beskrivs i föregående bild hello är som illustration och har inte skapats med hello steg som ingår i den här artikeln.

### <a name="create-hello-runbook"></a>Skapa hello runbook

hello första steg tooconfiguring hello exempel är toocreate hello runbook. Det här exemplet används en Kör som-konto. toolearn om Kör som-konton finns [autentisera Runbooks med Azure kör som-konto](../automation/automation-sec-configure-azure-runas-account.md)

### <a name="step-1"></a>Steg 1

Navigera tooAzure Automation i hello [Azure-portalen](https://portal.azure.com) och på **Runbooks**

![Översikt över Automation-konto][1]

### <a name="step-2"></a>Steg 2

Klicka på **lägga till en runbook** toostart hello skapandeprocessen av hello runbook.

![runbooks blad][2]

### <a name="step-3"></a>Steg 3

Under **Snabbregistrering**, klickar du på **skapa en ny runbook** toocreate hello runbook.

![Lägg till en runbook-bladet][3]

### <a name="step-4"></a>Steg 4

I det här steget kan vi ge hello runbook ett namn, i hello exempel kallas **Get-VPNGatewayStatus**. Det är viktigt toogive hello runbook ett beskrivande namn och rekommenderas ett namn som följer standard PowerShell namngivning standarder. Hej runbook-typen för det här exemplet är **PowerShell**, hello andra alternativ är grafisk PowerShell-arbetsflöde och grafisk PowerShell-arbetsflöde.

![runbook-bladet][4]

### <a name="step-5"></a>Steg 5

I det här steget hello runbook skapas, hello följande kodexempel visar alla hello koden behövs hello t.ex. Hej objekt i hello kod som innehåller \<värdet\> måste toobe ersättas med hello värden från din prenumeration.

Använd hello följande kod som Klicka **spara**

```PowerShell
# Set these variables toohello proper values for your environment
$o365AutomationCredential = "<Office 365 account>"
$fromEmail = "<from email address>"
$toEmail = "<tooemail address>"
$smtpServer = "<smtp.office365.com>"
$smtpPort = 587
$runAsConnectionName = "<AzureRunAsConnection>"
$subscriptionId = "<subscription id>"
$region = "<Azure region>"
$vpnConnectionName = "<vpn connection name>"
$vpnConnectionResourceGroup = "<resource group name>"
$storageAccountName = "<storage account name>"
$storageAccountResourceGroup = "<resource group name>"
$storageAccountContainer = "<container name>"

# Get credentials for Office 365 account
$cred = Get-AutomationPSCredential -Name $o365AutomationCredential

# Get hello connection "AzureRunAsConnection "
$servicePrincipalConnection=Get-AutomationConnection -Name $runAsConnectionName

"Logging in tooAzure..."
Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $servicePrincipalConnection.TenantId `
    -ApplicationId $servicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
"Setting context tooa specific subscription"
Set-AzureRmContext -SubscriptionId $subscriptionId

$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $region }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName
$connection = Get-AzureRmVirtualNetworkGatewayConnection -Name $vpnConnectionName -ResourceGroupName $vpnConnectionResourceGroup
$sa = Get-AzureRmStorageAccount -Name $storageAccountName -ResourceGroupName $storageAccountResourceGroup 
$storagePath = "$($sa.PrimaryEndpoints.Blob)$($storageAccountContainer)"
$result = Start-AzureRmNetworkWatcherResourceTroubleshooting -NetworkWatcher $networkWatcher -TargetResourceId $connection.Id -StorageId $sa.Id -StoragePath $storagePath

if($result.code -ne "Healthy")
    {
        $body = "Connection for $($connection.name) is: $($result.code) `n$($result.results[0].summary) `nView hello logs at $($storagePath) toolearn more."
        Write-Output $body
        $subject = "$($connection.name) Status"
        Send-MailMessage `
        -too$toEmail `
        -Subject $subject `
        -Body $body `
        -UseSsl `
        -Port $smtpPort `
        -SmtpServer $smtpServer `
        -From $fromEmail `
        -BodyAsHtml `
        -Credential $cred
    }
else
    {
    Write-Output ("Connection Status is: $($result.code)")
    }
```

### <a name="step-6"></a>Steg 6

När du har sparat hello runbook ett schema måste vara länkad tooit tooautomate hello start av hello runbook. toostart hello processen klickar du på **schema**.

![Steg 6][6]

## <a name="link-a-schedule-toohello-runbook"></a>Länka ett schema toohello runbook

Du måste skapa ett nytt schema. Klicka på **länka ett schema tooyour runbook**.

![Steg 7][7]

### <a name="step-1"></a>Steg 1

På hello **schema** bladet, klickar du på **skapa ett nytt schema**

![Steg 8][8]

### <a name="step-2"></a>Steg 2

På hello **nytt schema** bladet registrerar hello schemainformation. hello-värden som kan anges finns i följande lista hello:

- **Namnet** -hello eget namn för hello schema.
- **Beskrivning** -en beskrivning av hello schema.
- **Startar** -värdet är en kombination av datum, tid och tidszon som utgör hello tid hello schema utlösare.
- **Återkommande** – det här värdet fastställer hello scheman upprepning.  Giltiga värden är **när** eller **återkommande**.
- **Upprepas var** -hello upprepningsintervallet på hello schema i timmar, dagar, veckor eller månader.
- **Ställa in ett utgångsdatum** -hello värdet avgör om hello schema ska upphöra att gälla eller inte. Kan ställas in för**Ja** eller **nr**. Ett giltigt datum och tid är toobe som om du väljer Ja.

> [!NOTE]
> Om du behöver toohave en runbook körs oftare än varje timme, måste flera scheman skapas med olika intervall (det vill säga 15, 30, 45 minuter efter hello timme)

![Steg 9][9]

### <a name="step-3"></a>Steg 3

Klicka på Spara toosave hello schema toohello runbook.

![Steg 10][10]

## <a name="next-steps"></a>Nästa steg

Nu när du har en förståelse för hur toointegrate Nätverksbevakaren felsökning med Azure Automation lär du dig hur tootrigger paket avbildas på VM-aviseringar genom att besöka [skapar en avisering utlösta paketinsamling med Azure Nätverksbevakaren](network-watcher-alert-triggered-packet-capture.md).

<!-- images -->
[scenario]: ./media/network-watcher-monitor-with-azure-automation/scenario.png
[1]: ./media/network-watcher-monitor-with-azure-automation/figure1.png
[2]: ./media/network-watcher-monitor-with-azure-automation/figure2.png
[3]: ./media/network-watcher-monitor-with-azure-automation/figure3.png
[4]: ./media/network-watcher-monitor-with-azure-automation/figure4.png
[5]: ./media/network-watcher-monitor-with-azure-automation/figure5.png
[6]: ./media/network-watcher-monitor-with-azure-automation/figure6.png
[7]: ./media/network-watcher-monitor-with-azure-automation/figure7.png
[8]: ./media/network-watcher-monitor-with-azure-automation/figure8.png
[9]: ./media/network-watcher-monitor-with-azure-automation/figure9.png
[10]: ./media/network-watcher-monitor-with-azure-automation/figure10.png
