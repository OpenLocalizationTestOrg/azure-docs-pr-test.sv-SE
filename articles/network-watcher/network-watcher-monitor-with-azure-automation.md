---
title: "Övervaka VPN-gatewayer Azure Nätverksbevakaren felsökning | Microsoft Docs"
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
ms.openlocfilehash: 55ec52dd0d94a0347cc67a8785b89611da955111
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="monitor-vpn-gateways-with-network-watcher-troubleshooting"></a><span data-ttu-id="87bf7-103">Övervaka VPN-gatewayer Nätverksbevakaren felsökning</span><span class="sxs-lookup"><span data-stu-id="87bf7-103">Monitor VPN gateways with Network Watcher troubleshooting</span></span>

<span data-ttu-id="87bf7-104">Får du djupa insikter på nätverkets prestanda är viktigt att tillhandahålla tillförlitlig tjänster till kunder.</span><span class="sxs-lookup"><span data-stu-id="87bf7-104">Gaining deep insights on your network performance is critical to provide reliable services to customers.</span></span> <span data-ttu-id="87bf7-105">Det är därför viktigt att snabbt identifiera nätverksförhållanden avbrott och vidta åtgärder för att minimera avbrott villkoret.</span><span class="sxs-lookup"><span data-stu-id="87bf7-105">It is therefore critical to detect network outage conditions quickly and take corrective action to mitigate the outage condition.</span></span> <span data-ttu-id="87bf7-106">Azure Automation kan du implementera och köra en aktivitet i ett programmässiga sätt via runbooks.</span><span class="sxs-lookup"><span data-stu-id="87bf7-106">Azure Automation enables you to implement and run a task in a programmatic fashion through runbooks.</span></span> <span data-ttu-id="87bf7-107">Med hjälp av Azure Automation skapas ett perfekt recept för att utföra kontinuerlig och proaktiv övervakning och avisering.</span><span class="sxs-lookup"><span data-stu-id="87bf7-107">Using Azure Automation creates a perfect recipe for performing continuous and proactive network monitoring and alerting.</span></span>

## <a name="scenario"></a><span data-ttu-id="87bf7-108">Scenario</span><span class="sxs-lookup"><span data-stu-id="87bf7-108">Scenario</span></span>

<span data-ttu-id="87bf7-109">Scenariot i följande bild är ett program som flera nivåer med lokal anslutning upprättas med hjälp av en VPN-Gateway och -tunnel.</span><span class="sxs-lookup"><span data-stu-id="87bf7-109">The scenario in the following image is a multi-tiered application, with on premises connectivity established using a VPN Gateway and tunnel.</span></span> <span data-ttu-id="87bf7-110">Säkerställa VPN-Gateway är igång och körs är avgörande för prestanda för program.</span><span class="sxs-lookup"><span data-stu-id="87bf7-110">Ensuring the VPN Gateway is up and running is critical to the applications performance.</span></span>

<span data-ttu-id="87bf7-111">En runbook skapas med ett skript för att söka efter anslutningsstatusen för VPN-tunnel, med hjälp av Resource felsökning API för att söka efter status för användaranslutning tunnel.</span><span class="sxs-lookup"><span data-stu-id="87bf7-111">A runbook is created with a script to check for connection status of the VPN tunnel, using the Resource Troubleshooting API to check for connection tunnel status.</span></span> <span data-ttu-id="87bf7-112">Om statusen inte är felfri, skickas en e-utlösare för administratörer.</span><span class="sxs-lookup"><span data-stu-id="87bf7-112">If the status is not healthy, an email trigger is sent to administrators.</span></span>

![Exempel på ett scenario][scenario]

<span data-ttu-id="87bf7-114">Det här scenariot kommer:</span><span class="sxs-lookup"><span data-stu-id="87bf7-114">This scenario will:</span></span>

- <span data-ttu-id="87bf7-115">Skapa en runbook anropar den `Start-AzureRmNetworkWatcherResourceTroubleshooting` för att felsöka anslutningsstatus</span><span class="sxs-lookup"><span data-stu-id="87bf7-115">Create a runbook calling the `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet to troubleshoot connection status</span></span>
- <span data-ttu-id="87bf7-116">Länka ett schema till runbook</span><span class="sxs-lookup"><span data-stu-id="87bf7-116">Link a schedule to the runbook</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="87bf7-117">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="87bf7-117">Before you begin</span></span>

<span data-ttu-id="87bf7-118">Innan du startar det här scenariot måste du ha följande krav:</span><span class="sxs-lookup"><span data-stu-id="87bf7-118">Before you start this scenario, you must have the following pre-requisites:</span></span>

- <span data-ttu-id="87bf7-119">Ett Azure automation-konto i Azure.</span><span class="sxs-lookup"><span data-stu-id="87bf7-119">An Azure automation account in Azure.</span></span> <span data-ttu-id="87bf7-120">Kontrollera att automation-konto har de senaste modulerna och har även AzureRM.Network-modulen.</span><span class="sxs-lookup"><span data-stu-id="87bf7-120">Ensure that the automation account has the latest modules and also has the AzureRM.Network module.</span></span> <span data-ttu-id="87bf7-121">Modulen AzureRM.Network är tillgängliga i modulgalleriet om du behöver lägga till den i ditt automation-konto.</span><span class="sxs-lookup"><span data-stu-id="87bf7-121">The AzureRM.Network module is available in the module gallery if you need to add it to your automation account.</span></span>
- <span data-ttu-id="87bf7-122">Du måste ha en uppsättning autentiseringsuppgifter konfigurera i Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="87bf7-122">You must have a set of credentials configure in Azure Automation.</span></span> <span data-ttu-id="87bf7-123">Mer information finns i [Azure Automation-säkerhet](../automation/automation-security-overview.md)</span><span class="sxs-lookup"><span data-stu-id="87bf7-123">Learn more at [Azure Automation security](../automation/automation-security-overview.md)</span></span>
- <span data-ttu-id="87bf7-124">En giltig SMTP-server (Office 365, din lokala e-post eller en annan) och autentiseringsuppgifter som definierats i Azure Automation</span><span class="sxs-lookup"><span data-stu-id="87bf7-124">A valid SMTP server (Office 365, your on-premises email or another) and credentials defined in Azure Automation</span></span>
- <span data-ttu-id="87bf7-125">En konfigurerade virtuella nätverksgateway i Azure.</span><span class="sxs-lookup"><span data-stu-id="87bf7-125">A configured Virtual Network Gateway in Azure.</span></span>
- <span data-ttu-id="87bf7-126">Ett befintligt lagringskonto med en befintlig behållare för lagring i loggarna.</span><span class="sxs-lookup"><span data-stu-id="87bf7-126">An existing storage account with an existing container to store the logs in.</span></span>

> [!NOTE]
> <span data-ttu-id="87bf7-127">Den infrastruktur som beskrivs i föregående bild är som illustration och är inte skapade i steg i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="87bf7-127">The infrastructure depicted in the preceding image is for illustration purposes and are not created with the steps contained in this article.</span></span>

### <a name="create-the-runbook"></a><span data-ttu-id="87bf7-128">Skapa en runbook</span><span class="sxs-lookup"><span data-stu-id="87bf7-128">Create the runbook</span></span>

<span data-ttu-id="87bf7-129">Det första steget att konfigurera exemplet är att skapa runbook.</span><span class="sxs-lookup"><span data-stu-id="87bf7-129">The first step to configuring the example is to create the runbook.</span></span> <span data-ttu-id="87bf7-130">Det här exemplet används en Kör som-konto.</span><span class="sxs-lookup"><span data-stu-id="87bf7-130">This example uses a run-as account.</span></span> <span data-ttu-id="87bf7-131">Läs om Kör som-konton [autentisera Runbooks med Azure kör som-konto](../automation/automation-sec-configure-azure-runas-account.md)</span><span class="sxs-lookup"><span data-stu-id="87bf7-131">To learn about run-as accounts, visit [Authenticate Runbooks with Azure Run As account](../automation/automation-sec-configure-azure-runas-account.md)</span></span>

### <a name="step-1"></a><span data-ttu-id="87bf7-132">Steg 1</span><span class="sxs-lookup"><span data-stu-id="87bf7-132">Step 1</span></span>

<span data-ttu-id="87bf7-133">Gå till Azure Automation i den [Azure-portalen](https://portal.azure.com) och på **Runbooks**</span><span class="sxs-lookup"><span data-stu-id="87bf7-133">Navigate to Azure Automation in the [Azure portal](https://portal.azure.com) and click **Runbooks**</span></span>

![Översikt över Automation-konto][1]

### <a name="step-2"></a><span data-ttu-id="87bf7-135">Steg 2</span><span class="sxs-lookup"><span data-stu-id="87bf7-135">Step 2</span></span>

<span data-ttu-id="87bf7-136">Klicka på **lägga till en runbook** att starta processen för runbook.</span><span class="sxs-lookup"><span data-stu-id="87bf7-136">Click **Add a runbook** to start the creation process of the runbook.</span></span>

![runbooks blad][2]

### <a name="step-3"></a><span data-ttu-id="87bf7-138">Steg 3</span><span class="sxs-lookup"><span data-stu-id="87bf7-138">Step 3</span></span>

<span data-ttu-id="87bf7-139">Under **Snabbregistrering**, klickar du på **skapa en ny runbook** att skapa en runbook.</span><span class="sxs-lookup"><span data-stu-id="87bf7-139">Under **Quick Create**, click **Create a new runbook** to create the runbook.</span></span>

![Lägg till en runbook-bladet][3]

### <a name="step-4"></a><span data-ttu-id="87bf7-141">Steg 4</span><span class="sxs-lookup"><span data-stu-id="87bf7-141">Step 4</span></span>

<span data-ttu-id="87bf7-142">I det här steget kan vi ge runbook ett namn, i exempel kallas **Get-VPNGatewayStatus**.</span><span class="sxs-lookup"><span data-stu-id="87bf7-142">In this step, we give the runbook a name, in the example it is called **Get-VPNGatewayStatus**.</span></span> <span data-ttu-id="87bf7-143">Det är viktigt att ange ett beskrivande namn för runbook och rekommenderade ett namn som följer standard PowerShell namngivning standarder.</span><span class="sxs-lookup"><span data-stu-id="87bf7-143">It is important to give the runbook a descriptive name, and recommended giving it a name that follows standard PowerShell naming standards.</span></span> <span data-ttu-id="87bf7-144">Runbook-typen för det här exemplet är **PowerShell**, de andra alternativ är grafisk PowerShell-arbetsflöde och grafisk PowerShell-arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="87bf7-144">The runbook type for this example is **PowerShell**, the other options are Graphical, PowerShell workflow, and Graphical PowerShell workflow.</span></span>

![runbook-bladet][4]

### <a name="step-5"></a><span data-ttu-id="87bf7-146">Steg 5</span><span class="sxs-lookup"><span data-stu-id="87bf7-146">Step 5</span></span>

<span data-ttu-id="87bf7-147">Följande kodexempel innehåller all kod som behövs för exemplet i det här steget som skapats i runbook.</span><span class="sxs-lookup"><span data-stu-id="87bf7-147">In this step the runbook is created, the following code example provides all the code needed for the example.</span></span> <span data-ttu-id="87bf7-148">Objekten i kod som innehåller \<värdet\> måste ersättas med värden från din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="87bf7-148">The items in the code that contain \<value\> need to be replaced with the values from your subscription.</span></span>

<span data-ttu-id="87bf7-149">Använd följande kod som Klicka **spara**</span><span class="sxs-lookup"><span data-stu-id="87bf7-149">Use the following code as click **Save**</span></span>

```PowerShell
# Set these variables to the proper values for your environment
$o365AutomationCredential = "<Office 365 account>"
$fromEmail = "<from email address>"
$toEmail = "<to email address>"
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

# Get the connection "AzureRunAsConnection "
$servicePrincipalConnection=Get-AutomationConnection -Name $runAsConnectionName

"Logging in to Azure..."
Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $servicePrincipalConnection.TenantId `
    -ApplicationId $servicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
"Setting context to a specific subscription"
Set-AzureRmContext -SubscriptionId $subscriptionId

$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $region }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName
$connection = Get-AzureRmVirtualNetworkGatewayConnection -Name $vpnConnectionName -ResourceGroupName $vpnConnectionResourceGroup
$sa = Get-AzureRmStorageAccount -Name $storageAccountName -ResourceGroupName $storageAccountResourceGroup 
$storagePath = "$($sa.PrimaryEndpoints.Blob)$($storageAccountContainer)"
$result = Start-AzureRmNetworkWatcherResourceTroubleshooting -NetworkWatcher $networkWatcher -TargetResourceId $connection.Id -StorageId $sa.Id -StoragePath $storagePath

if($result.code -ne "Healthy")
    {
        $body = "Connection for $($connection.name) is: $($result.code) `n$($result.results[0].summary) `nView the logs at $($storagePath) to learn more."
        Write-Output $body
        $subject = "$($connection.name) Status"
        Send-MailMessage `
        -To $toEmail `
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

### <a name="step-6"></a><span data-ttu-id="87bf7-150">Steg 6</span><span class="sxs-lookup"><span data-stu-id="87bf7-150">Step 6</span></span>

<span data-ttu-id="87bf7-151">När du har sparat runbook måste ett schema länkas till den att automatisera start av runbook.</span><span class="sxs-lookup"><span data-stu-id="87bf7-151">Once the runbook is saved, a schedule must be linked to it to automate the start of the runbook.</span></span> <span data-ttu-id="87bf7-152">Starta processen, klicka på **schema**.</span><span class="sxs-lookup"><span data-stu-id="87bf7-152">To start the process, click **Schedule**.</span></span>

![Steg 6][6]

## <a name="link-a-schedule-to-the-runbook"></a><span data-ttu-id="87bf7-154">Länka ett schema till runbook</span><span class="sxs-lookup"><span data-stu-id="87bf7-154">Link a schedule to the runbook</span></span>

<span data-ttu-id="87bf7-155">Du måste skapa ett nytt schema.</span><span class="sxs-lookup"><span data-stu-id="87bf7-155">A new schedule must be created.</span></span> <span data-ttu-id="87bf7-156">Klicka på **länka ett schema till din runbook**.</span><span class="sxs-lookup"><span data-stu-id="87bf7-156">Click **Link a schedule to your runbook**.</span></span>

![Steg 7][7]

### <a name="step-1"></a><span data-ttu-id="87bf7-158">Steg 1</span><span class="sxs-lookup"><span data-stu-id="87bf7-158">Step 1</span></span>

<span data-ttu-id="87bf7-159">På den **schema** bladet, klickar du på **skapa ett nytt schema**</span><span class="sxs-lookup"><span data-stu-id="87bf7-159">On the **Schedule** blade, click **Create a new schedule**</span></span>

![Steg 8][8]

### <a name="step-2"></a><span data-ttu-id="87bf7-161">Steg 2</span><span class="sxs-lookup"><span data-stu-id="87bf7-161">Step 2</span></span>

<span data-ttu-id="87bf7-162">På den **nytt schema** bladet fill schema-information.</span><span class="sxs-lookup"><span data-stu-id="87bf7-162">On the **New Schedule** blade fill out the schedule information.</span></span> <span data-ttu-id="87bf7-163">De värden som kan anges finns i följande lista:</span><span class="sxs-lookup"><span data-stu-id="87bf7-163">The values that can be set are in the following list:</span></span>

- <span data-ttu-id="87bf7-164">**Namnet** -ett eget namn för schemat.</span><span class="sxs-lookup"><span data-stu-id="87bf7-164">**Name** - The friendly name of the schedule.</span></span>
- <span data-ttu-id="87bf7-165">**Beskrivning** -en beskrivning av schemat.</span><span class="sxs-lookup"><span data-stu-id="87bf7-165">**Description** - A description of the schedule.</span></span>
- <span data-ttu-id="87bf7-166">**Startar** -värdet är en kombination av datum, tid och tidszon som utgör schema-utlösare.</span><span class="sxs-lookup"><span data-stu-id="87bf7-166">**Starts** - This value is a combination of date, time, and time zone that make up the time the schedule triggers.</span></span>
- <span data-ttu-id="87bf7-167">**Återkommande** – det här värdet fastställer scheman upprepning.</span><span class="sxs-lookup"><span data-stu-id="87bf7-167">**Recurrence** - This value determines the schedules repetition.</span></span>  <span data-ttu-id="87bf7-168">Giltiga värden är **när** eller **återkommande**.</span><span class="sxs-lookup"><span data-stu-id="87bf7-168">Valid values are **Once** or **Recurring**.</span></span>
- <span data-ttu-id="87bf7-169">**Upprepas var** -intervall för schemat i timmar, dagar, veckor eller månader.</span><span class="sxs-lookup"><span data-stu-id="87bf7-169">**Recur every** - The recurrence interval of the schedule in hours, days, weeks, or months.</span></span>
- <span data-ttu-id="87bf7-170">**Ställa in ett utgångsdatum** -värdet fastställer om schemat ska upphöra att gälla eller inte.</span><span class="sxs-lookup"><span data-stu-id="87bf7-170">**Set Expiration** - The value determines if the schedule should expire or not.</span></span> <span data-ttu-id="87bf7-171">Kan anges till **Ja** eller **nr**.</span><span class="sxs-lookup"><span data-stu-id="87bf7-171">Can be set to **Yes** or **No**.</span></span> <span data-ttu-id="87bf7-172">Ett giltigt datum och tid är ska tillhandahållas om du väljer Ja.</span><span class="sxs-lookup"><span data-stu-id="87bf7-172">A valid date and time are to be provided if yes is chosen.</span></span>

> [!NOTE]
> <span data-ttu-id="87bf7-173">Om du behöver ha en runbook körs oftare än en gång i timmen måste flera scheman skapas med olika intervall (det vill säga 15, 30, 45 minuter efter timmen)</span><span class="sxs-lookup"><span data-stu-id="87bf7-173">If you need to have a runbook run more often than every hour, multiple schedules must be created at different intervals (that is, 15, 30, 45 minutes after the hour)</span></span>

![Steg 9][9]

### <a name="step-3"></a><span data-ttu-id="87bf7-175">Steg 3</span><span class="sxs-lookup"><span data-stu-id="87bf7-175">Step 3</span></span>

<span data-ttu-id="87bf7-176">Klicka på Spara för att spara schemat till runbook.</span><span class="sxs-lookup"><span data-stu-id="87bf7-176">Click Save to save the schedule to the runbook.</span></span>

![Steg 10][10]

## <a name="next-steps"></a><span data-ttu-id="87bf7-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="87bf7-178">Next steps</span></span>

<span data-ttu-id="87bf7-179">Nu när du har en förståelse för hur du integrerar Nätverksbevakaren felsökning med Azure Automation lär du dig hur du utlöser paket insamlingar på VM-aviseringar genom att besöka [skapar en avisering utlösta paketinsamling med Azure Nätverksbevakaren](network-watcher-alert-triggered-packet-capture.md).</span><span class="sxs-lookup"><span data-stu-id="87bf7-179">Now that you have an understanding on how to integrate Network Watcher troubleshooting with Azure Automation, learn how to trigger packet captures on VM alerts by visiting [Create an alert triggered packet capture with Azure Network Watcher](network-watcher-alert-triggered-packet-capture.md).</span></span>

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
