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
# <a name="monitor-vpn-gateways-with-network-watcher-troubleshooting"></a><span data-ttu-id="ee9d2-103">Övervaka VPN-gatewayer Nätverksbevakaren felsökning</span><span class="sxs-lookup"><span data-stu-id="ee9d2-103">Monitor VPN gateways with Network Watcher troubleshooting</span></span>

<span data-ttu-id="ee9d2-104">Får du djupa insikter på nätverkets prestanda är kritiska tooprovide reliable services toocustomers.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-104">Gaining deep insights on your network performance is critical tooprovide reliable services toocustomers.</span></span> <span data-ttu-id="ee9d2-105">Det är därför viktiga toodetect avbrott nätverksförhållanden snabbt och vidta åtgärder toomitigate hello avbrott villkor.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-105">It is therefore critical toodetect network outage conditions quickly and take corrective action toomitigate hello outage condition.</span></span> <span data-ttu-id="ee9d2-106">Azure Automation kan du tooimplement och köra en aktivitet i ett programmässiga sätt via runbooks.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-106">Azure Automation enables you tooimplement and run a task in a programmatic fashion through runbooks.</span></span> <span data-ttu-id="ee9d2-107">Med hjälp av Azure Automation skapas ett perfekt recept för att utföra kontinuerlig och proaktiv övervakning och avisering.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-107">Using Azure Automation creates a perfect recipe for performing continuous and proactive network monitoring and alerting.</span></span>

## <a name="scenario"></a><span data-ttu-id="ee9d2-108">Scenario</span><span class="sxs-lookup"><span data-stu-id="ee9d2-108">Scenario</span></span>

<span data-ttu-id="ee9d2-109">hello scenariot i hello följande bild är ett program som flera nivåer med lokal anslutning upprättas med hjälp av en VPN-Gateway och -tunnel.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-109">hello scenario in hello following image is a multi-tiered application, with on premises connectivity established using a VPN Gateway and tunnel.</span></span> <span data-ttu-id="ee9d2-110">Säkerställa hello VPN-Gateway är igång och körs är kritiska toohello program prestanda.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-110">Ensuring hello VPN Gateway is up and running is critical toohello applications performance.</span></span>

<span data-ttu-id="ee9d2-111">En runbook skapas med ett skript toocheck för anslutningsstatus hello VPN-tunnel, med hjälp av hello resurs felsökning API toocheck för tunnel anslutningsstatus.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-111">A runbook is created with a script toocheck for connection status of hello VPN tunnel, using hello Resource Troubleshooting API toocheck for connection tunnel status.</span></span> <span data-ttu-id="ee9d2-112">Om hello status inte är felfri, skickas en e-utlösare tooadministrators.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-112">If hello status is not healthy, an email trigger is sent tooadministrators.</span></span>

![Exempel på ett scenario][scenario]

<span data-ttu-id="ee9d2-114">Det här scenariot kommer:</span><span class="sxs-lookup"><span data-stu-id="ee9d2-114">This scenario will:</span></span>

- <span data-ttu-id="ee9d2-115">Skapa en runbook anropa hello `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet tootroubleshoot anslutningsstatus</span><span class="sxs-lookup"><span data-stu-id="ee9d2-115">Create a runbook calling hello `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet tootroubleshoot connection status</span></span>
- <span data-ttu-id="ee9d2-116">Länka ett schema toohello runbook</span><span class="sxs-lookup"><span data-stu-id="ee9d2-116">Link a schedule toohello runbook</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="ee9d2-117">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="ee9d2-117">Before you begin</span></span>

<span data-ttu-id="ee9d2-118">Innan du startar det här scenariot måste du ha hello följande förutsättningar:</span><span class="sxs-lookup"><span data-stu-id="ee9d2-118">Before you start this scenario, you must have hello following pre-requisites:</span></span>

- <span data-ttu-id="ee9d2-119">Ett Azure automation-konto i Azure.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-119">An Azure automation account in Azure.</span></span> <span data-ttu-id="ee9d2-120">Kontrollera att hello automation-konto har senaste hello-moduler och har även hello AzureRM.Network modul.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-120">Ensure that hello automation account has hello latest modules and also has hello AzureRM.Network module.</span></span> <span data-ttu-id="ee9d2-121">Hej AzureRM.Network-modulen är tillgänglig i hello modulgalleriet om du behöver tooadd den tooyour automation-konto.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-121">hello AzureRM.Network module is available in hello module gallery if you need tooadd it tooyour automation account.</span></span>
- <span data-ttu-id="ee9d2-122">Du måste ha en uppsättning autentiseringsuppgifter konfigurera i Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-122">You must have a set of credentials configure in Azure Automation.</span></span> <span data-ttu-id="ee9d2-123">Mer information finns i [Azure Automation-säkerhet](../automation/automation-security-overview.md)</span><span class="sxs-lookup"><span data-stu-id="ee9d2-123">Learn more at [Azure Automation security](../automation/automation-security-overview.md)</span></span>
- <span data-ttu-id="ee9d2-124">En giltig SMTP-server (Office 365, din lokala e-post eller en annan) och autentiseringsuppgifter som definierats i Azure Automation</span><span class="sxs-lookup"><span data-stu-id="ee9d2-124">A valid SMTP server (Office 365, your on-premises email or another) and credentials defined in Azure Automation</span></span>
- <span data-ttu-id="ee9d2-125">En konfigurerade virtuella nätverksgateway i Azure.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-125">A configured Virtual Network Gateway in Azure.</span></span>
- <span data-ttu-id="ee9d2-126">Ett befintligt lagringskonto med en befintlig behållare toostore hello loggar in.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-126">An existing storage account with an existing container toostore hello logs in.</span></span>

> [!NOTE]
> <span data-ttu-id="ee9d2-127">hello-infrastruktur som beskrivs i föregående bild hello är som illustration och har inte skapats med hello steg som ingår i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-127">hello infrastructure depicted in hello preceding image is for illustration purposes and are not created with hello steps contained in this article.</span></span>

### <a name="create-hello-runbook"></a><span data-ttu-id="ee9d2-128">Skapa hello runbook</span><span class="sxs-lookup"><span data-stu-id="ee9d2-128">Create hello runbook</span></span>

<span data-ttu-id="ee9d2-129">hello första steg tooconfiguring hello exempel är toocreate hello runbook.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-129">hello first step tooconfiguring hello example is toocreate hello runbook.</span></span> <span data-ttu-id="ee9d2-130">Det här exemplet används en Kör som-konto.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-130">This example uses a run-as account.</span></span> <span data-ttu-id="ee9d2-131">toolearn om Kör som-konton finns [autentisera Runbooks med Azure kör som-konto](../automation/automation-sec-configure-azure-runas-account.md)</span><span class="sxs-lookup"><span data-stu-id="ee9d2-131">toolearn about run-as accounts, visit [Authenticate Runbooks with Azure Run As account](../automation/automation-sec-configure-azure-runas-account.md)</span></span>

### <a name="step-1"></a><span data-ttu-id="ee9d2-132">Steg 1</span><span class="sxs-lookup"><span data-stu-id="ee9d2-132">Step 1</span></span>

<span data-ttu-id="ee9d2-133">Navigera tooAzure Automation i hello [Azure-portalen](https://portal.azure.com) och på **Runbooks**</span><span class="sxs-lookup"><span data-stu-id="ee9d2-133">Navigate tooAzure Automation in hello [Azure portal](https://portal.azure.com) and click **Runbooks**</span></span>

![Översikt över Automation-konto][1]

### <a name="step-2"></a><span data-ttu-id="ee9d2-135">Steg 2</span><span class="sxs-lookup"><span data-stu-id="ee9d2-135">Step 2</span></span>

<span data-ttu-id="ee9d2-136">Klicka på **lägga till en runbook** toostart hello skapandeprocessen av hello runbook.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-136">Click **Add a runbook** toostart hello creation process of hello runbook.</span></span>

![runbooks blad][2]

### <a name="step-3"></a><span data-ttu-id="ee9d2-138">Steg 3</span><span class="sxs-lookup"><span data-stu-id="ee9d2-138">Step 3</span></span>

<span data-ttu-id="ee9d2-139">Under **Snabbregistrering**, klickar du på **skapa en ny runbook** toocreate hello runbook.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-139">Under **Quick Create**, click **Create a new runbook** toocreate hello runbook.</span></span>

![Lägg till en runbook-bladet][3]

### <a name="step-4"></a><span data-ttu-id="ee9d2-141">Steg 4</span><span class="sxs-lookup"><span data-stu-id="ee9d2-141">Step 4</span></span>

<span data-ttu-id="ee9d2-142">I det här steget kan vi ge hello runbook ett namn, i hello exempel kallas **Get-VPNGatewayStatus**.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-142">In this step, we give hello runbook a name, in hello example it is called **Get-VPNGatewayStatus**.</span></span> <span data-ttu-id="ee9d2-143">Det är viktigt toogive hello runbook ett beskrivande namn och rekommenderas ett namn som följer standard PowerShell namngivning standarder.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-143">It is important toogive hello runbook a descriptive name, and recommended giving it a name that follows standard PowerShell naming standards.</span></span> <span data-ttu-id="ee9d2-144">Hej runbook-typen för det här exemplet är **PowerShell**, hello andra alternativ är grafisk PowerShell-arbetsflöde och grafisk PowerShell-arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-144">hello runbook type for this example is **PowerShell**, hello other options are Graphical, PowerShell workflow, and Graphical PowerShell workflow.</span></span>

![runbook-bladet][4]

### <a name="step-5"></a><span data-ttu-id="ee9d2-146">Steg 5</span><span class="sxs-lookup"><span data-stu-id="ee9d2-146">Step 5</span></span>

<span data-ttu-id="ee9d2-147">I det här steget hello runbook skapas, hello följande kodexempel visar alla hello koden behövs hello t.ex.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-147">In this step hello runbook is created, hello following code example provides all hello code needed for hello example.</span></span> <span data-ttu-id="ee9d2-148">Hej objekt i hello kod som innehåller \<värdet\> måste toobe ersättas med hello värden från din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-148">hello items in hello code that contain \<value\> need toobe replaced with hello values from your subscription.</span></span>

<span data-ttu-id="ee9d2-149">Använd hello följande kod som Klicka **spara**</span><span class="sxs-lookup"><span data-stu-id="ee9d2-149">Use hello following code as click **Save**</span></span>

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

### <a name="step-6"></a><span data-ttu-id="ee9d2-150">Steg 6</span><span class="sxs-lookup"><span data-stu-id="ee9d2-150">Step 6</span></span>

<span data-ttu-id="ee9d2-151">När du har sparat hello runbook ett schema måste vara länkad tooit tooautomate hello start av hello runbook.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-151">Once hello runbook is saved, a schedule must be linked tooit tooautomate hello start of hello runbook.</span></span> <span data-ttu-id="ee9d2-152">toostart hello processen klickar du på **schema**.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-152">toostart hello process, click **Schedule**.</span></span>

![Steg 6][6]

## <a name="link-a-schedule-toohello-runbook"></a><span data-ttu-id="ee9d2-154">Länka ett schema toohello runbook</span><span class="sxs-lookup"><span data-stu-id="ee9d2-154">Link a schedule toohello runbook</span></span>

<span data-ttu-id="ee9d2-155">Du måste skapa ett nytt schema.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-155">A new schedule must be created.</span></span> <span data-ttu-id="ee9d2-156">Klicka på **länka ett schema tooyour runbook**.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-156">Click **Link a schedule tooyour runbook**.</span></span>

![Steg 7][7]

### <a name="step-1"></a><span data-ttu-id="ee9d2-158">Steg 1</span><span class="sxs-lookup"><span data-stu-id="ee9d2-158">Step 1</span></span>

<span data-ttu-id="ee9d2-159">På hello **schema** bladet, klickar du på **skapa ett nytt schema**</span><span class="sxs-lookup"><span data-stu-id="ee9d2-159">On hello **Schedule** blade, click **Create a new schedule**</span></span>

![Steg 8][8]

### <a name="step-2"></a><span data-ttu-id="ee9d2-161">Steg 2</span><span class="sxs-lookup"><span data-stu-id="ee9d2-161">Step 2</span></span>

<span data-ttu-id="ee9d2-162">På hello **nytt schema** bladet registrerar hello schemainformation.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-162">On hello **New Schedule** blade fill out hello schedule information.</span></span> <span data-ttu-id="ee9d2-163">hello-värden som kan anges finns i följande lista hello:</span><span class="sxs-lookup"><span data-stu-id="ee9d2-163">hello values that can be set are in hello following list:</span></span>

- <span data-ttu-id="ee9d2-164">**Namnet** -hello eget namn för hello schema.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-164">**Name** - hello friendly name of hello schedule.</span></span>
- <span data-ttu-id="ee9d2-165">**Beskrivning** -en beskrivning av hello schema.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-165">**Description** - A description of hello schedule.</span></span>
- <span data-ttu-id="ee9d2-166">**Startar** -värdet är en kombination av datum, tid och tidszon som utgör hello tid hello schema utlösare.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-166">**Starts** - This value is a combination of date, time, and time zone that make up hello time hello schedule triggers.</span></span>
- <span data-ttu-id="ee9d2-167">**Återkommande** – det här värdet fastställer hello scheman upprepning.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-167">**Recurrence** - This value determines hello schedules repetition.</span></span>  <span data-ttu-id="ee9d2-168">Giltiga värden är **när** eller **återkommande**.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-168">Valid values are **Once** or **Recurring**.</span></span>
- <span data-ttu-id="ee9d2-169">**Upprepas var** -hello upprepningsintervallet på hello schema i timmar, dagar, veckor eller månader.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-169">**Recur every** - hello recurrence interval of hello schedule in hours, days, weeks, or months.</span></span>
- <span data-ttu-id="ee9d2-170">**Ställa in ett utgångsdatum** -hello värdet avgör om hello schema ska upphöra att gälla eller inte.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-170">**Set Expiration** - hello value determines if hello schedule should expire or not.</span></span> <span data-ttu-id="ee9d2-171">Kan ställas in för**Ja** eller **nr**.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-171">Can be set too**Yes** or **No**.</span></span> <span data-ttu-id="ee9d2-172">Ett giltigt datum och tid är toobe som om du väljer Ja.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-172">A valid date and time are toobe provided if yes is chosen.</span></span>

> [!NOTE]
> <span data-ttu-id="ee9d2-173">Om du behöver toohave en runbook körs oftare än varje timme, måste flera scheman skapas med olika intervall (det vill säga 15, 30, 45 minuter efter hello timme)</span><span class="sxs-lookup"><span data-stu-id="ee9d2-173">If you need toohave a runbook run more often than every hour, multiple schedules must be created at different intervals (that is, 15, 30, 45 minutes after hello hour)</span></span>

![Steg 9][9]

### <a name="step-3"></a><span data-ttu-id="ee9d2-175">Steg 3</span><span class="sxs-lookup"><span data-stu-id="ee9d2-175">Step 3</span></span>

<span data-ttu-id="ee9d2-176">Klicka på Spara toosave hello schema toohello runbook.</span><span class="sxs-lookup"><span data-stu-id="ee9d2-176">Click Save toosave hello schedule toohello runbook.</span></span>

![Steg 10][10]

## <a name="next-steps"></a><span data-ttu-id="ee9d2-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ee9d2-178">Next steps</span></span>

<span data-ttu-id="ee9d2-179">Nu när du har en förståelse för hur toointegrate Nätverksbevakaren felsökning med Azure Automation lär du dig hur tootrigger paket avbildas på VM-aviseringar genom att besöka [skapar en avisering utlösta paketinsamling med Azure Nätverksbevakaren](network-watcher-alert-triggered-packet-capture.md).</span><span class="sxs-lookup"><span data-stu-id="ee9d2-179">Now that you have an understanding on how toointegrate Network Watcher troubleshooting with Azure Automation, learn how tootrigger packet captures on VM alerts by visiting [Create an alert triggered packet capture with Azure Network Watcher](network-watcher-alert-triggered-packet-capture.md).</span></span>

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
