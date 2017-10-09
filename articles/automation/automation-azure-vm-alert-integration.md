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
# <a name="azure-automation-scenario---remediate-azure-vm-alerts"></a><span data-ttu-id="4777f-103">Azure Automation-scenario – åtgärda Virtuella Azure-aviseringar</span><span class="sxs-lookup"><span data-stu-id="4777f-103">Azure Automation scenario - remediate Azure VM alerts</span></span>
<span data-ttu-id="4777f-104">Azure Automation och Azure virtuella datorer har släppt en ny funktion som gör att du tooconfigure virtuell dator (VM) aviseringar toorun Automation-runbooks.</span><span class="sxs-lookup"><span data-stu-id="4777f-104">Azure Automation and Azure Virtual Machines have released a new feature allowing you tooconfigure Virtual Machine (VM) alerts toorun Automation runbooks.</span></span> <span data-ttu-id="4777f-105">Den här nya funktionen kan du tooautomatically utför standard reparation i svaret tooVM aviseringar, som startar om eller stoppa hello VM.</span><span class="sxs-lookup"><span data-stu-id="4777f-105">This new capability allows you tooautomatically perform standard remediation in response tooVM alerts, like restarting or stopping hello VM.</span></span>

<span data-ttu-id="4777f-106">Tidigare under skapande av VM varningsregeln kunde för[ange en Automation-webhook](https://azure.microsoft.com/blog/using-azure-automation-to-take-actions-on-azure-alerts/) tooa runbook i ordning toorun hello runbook när hello avisering utlöses.</span><span class="sxs-lookup"><span data-stu-id="4777f-106">Previously, during VM alert rule creation you were able too[specify an Automation webhook](https://azure.microsoft.com/blog/using-azure-automation-to-take-actions-on-azure-alerts/) tooa runbook in order toorun hello runbook whenever hello alert triggered.</span></span> <span data-ttu-id="4777f-107">Det måste dock du toodo hello arbetet med att skapa hello runbook, skapa hello webhook för hello runbook och sedan kopiera och klistra in hello webhook när regeln skapades.</span><span class="sxs-lookup"><span data-stu-id="4777f-107">However, this required you toodo hello work of creating hello runbook, creating hello webhook for hello runbook, and then copying and pasting hello webhook during alert rule creation.</span></span> <span data-ttu-id="4777f-108">Hello processen är mycket enklare med den här nya versionen eftersom du kan välja en runbook från en lista direkt under skapande av varning och du kan välja ett Automation-konto som ska köra hello runbook eller enkelt skapa ett konto.</span><span class="sxs-lookup"><span data-stu-id="4777f-108">With this new release, hello process is much easier because you can directly choose a runbook from a list during alert rule creation, and you can choose an Automation account which will run hello runbook or easily create an account.</span></span>

<span data-ttu-id="4777f-109">I den här artikeln ska vi visa hur lätt det är tooset upp en virtuell dator i Azure-avisering och konfigurera toorun ett Automation-runbook när hello avisering utlöser.</span><span class="sxs-lookup"><span data-stu-id="4777f-109">In this article, we will show you how easy it is tooset up an Azure VM alert and configure an Automation runbook toorun whenever hello alert triggers.</span></span> <span data-ttu-id="4777f-110">Exempelscenarier är att starta om en virtuell dator när hello minnesanvändning överskrider tröskelvärdet för förfallodatum tooan programmet på hello VM med en minnesläcka eller stoppa en virtuell dator när hello CPU användartid har mindre än 1% för senaste timmen och används inte.</span><span class="sxs-lookup"><span data-stu-id="4777f-110">Example scenarios include restarting a VM when hello memory usage exceeds some threshold due tooan application on hello VM with a memory leak, or stopping a VM when hello CPU user time has been below 1% for past hour and is not in use.</span></span> <span data-ttu-id="4777f-111">Vi förklarar också hur hello automatiskt skapande av ett huvudnamn för tjänsten i ditt Automation förenklar konto hello användningen av runbooks i Azure avisering reparation.</span><span class="sxs-lookup"><span data-stu-id="4777f-111">We’ll also explain how hello automated creation of a service principal in your Automation account simplifies hello use of runbooks in Azure alert remediation.</span></span>

## <a name="create-an-alert-on-a-vm"></a><span data-ttu-id="4777f-112">Skapa en avisering på en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="4777f-112">Create an alert on a VM</span></span>
<span data-ttu-id="4777f-113">Utföra hello efter steg tooconfigure en avisering toolaunch en runbook när dess tröskelvärde har uppfyllts.</span><span class="sxs-lookup"><span data-stu-id="4777f-113">Perform hello following steps tooconfigure an alert toolaunch a runbook when its threshold has been met.</span></span>

> [!NOTE]
> <span data-ttu-id="4777f-114">Med den här versionen stöder vi bara V2 virtuella datorer och stöd för klassiska virtuella datorer läggs snart.</span><span class="sxs-lookup"><span data-stu-id="4777f-114">With this release, we only support V2 virtual machines and support for classic VMs will be added soon.</span></span>  
> 
> 

1. <span data-ttu-id="4777f-115">Logga in toohello Azure-portalen och klicka på **virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="4777f-115">Log in toohello Azure portal and click **Virtual Machines**.</span></span>  
2. <span data-ttu-id="4777f-116">Välj någon av de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="4777f-116">Select one of your virtual machines.</span></span>  <span data-ttu-id="4777f-117">hello virtuella instrumentpanelen bladet ska visas och hello **inställningar** bladet tooits höger.</span><span class="sxs-lookup"><span data-stu-id="4777f-117">hello virtual machine dashboard blade will appear and hello **Settings** blade tooits right.</span></span>  
3. <span data-ttu-id="4777f-118">Från hello **inställningar** bladet under hello övervakning Välj **Varna regler**.</span><span class="sxs-lookup"><span data-stu-id="4777f-118">From hello **Settings** blade, under hello Monitoring section select **Alert rules**.</span></span>
4. <span data-ttu-id="4777f-119">På hello **Varna regler** bladet, klickar du på **Lägg till avisering**.</span><span class="sxs-lookup"><span data-stu-id="4777f-119">On hello **Alert rules** blade, click **Add alert**.</span></span>

<span data-ttu-id="4777f-120">Detta öppnar hello **lägga till en varningsregel** bladet, där du kan konfigurera hello villkor för hello aviseringen och välja mellan en eller flera av följande alternativ: skicka e-toosomeone använder ett webhook tooforward hello avisering tooanother system och/eller Kör en Automation-runbook i svaret försök tooremediate hello problemet.</span><span class="sxs-lookup"><span data-stu-id="4777f-120">This opens up hello **Add an alert rule** blade, where you can configure hello conditions for hello alert and choose among one or all of these options: send email toosomeone, use a webhook tooforward hello alert tooanother system, and/or run an Automation runbook in response attempt tooremediate hello issue.</span></span>

## <a name="configure-a-runbook"></a><span data-ttu-id="4777f-121">Konfigurera en runbook</span><span class="sxs-lookup"><span data-stu-id="4777f-121">Configure a runbook</span></span>
<span data-ttu-id="4777f-122">tooconfigure toorun en runbook när hello VM tröskelvärdena är uppfyllt, Välj **Automation-Runbook**.</span><span class="sxs-lookup"><span data-stu-id="4777f-122">tooconfigure a runbook toorun when hello VM alert threshold is met, select **Automation Runbook**.</span></span> <span data-ttu-id="4777f-123">I hello **konfigurerar runbook** bladet kan du välja hello runbook toorun och hello Automation-konto toorun hello runbook i.</span><span class="sxs-lookup"><span data-stu-id="4777f-123">In hello **Configure runbook** blade, you can select hello runbook toorun and hello Automation account toorun hello runbook in.</span></span>

![Konfigurera en Automation-runbook och skapa ett nytt Automation-konto](media/automation-azure-vm-alert-integration/ConfigureRunbookNewAccount.png)

> [!NOTE]
> <span data-ttu-id="4777f-125">Den här versionen som du kan välja mellan tre runbooks som ger hello-tjänsten – starta om VM, stoppa VM eller ta bort virtuell dator (ta bort det).</span><span class="sxs-lookup"><span data-stu-id="4777f-125">For this release you can choose from three runbooks that hello service provides – Restart VM, Stop VM, or Remove VM (delete it).</span></span>  <span data-ttu-id="4777f-126">Hej möjlighet tooselect andra runbooks eller en av dina egna runbooks blir tillgänglig i framtida versioner.</span><span class="sxs-lookup"><span data-stu-id="4777f-126">hello ability tooselect other runbooks or one of your own runbooks will be available in a future release.</span></span>
> 
> 

![Runbooks toochoose från](media/automation-azure-vm-alert-integration/RunbooksToChoose.png)

<span data-ttu-id="4777f-128">När du har valt en hello tre tillgängliga runbooks hello **Automation-konto** listrutan visas och du kan välja ett automation konto hello runbook körs i.</span><span class="sxs-lookup"><span data-stu-id="4777f-128">After you select one of hello three available runbooks, hello **Automation account** drop-down list appears and you can select an automation account hello runbook will run as.</span></span> <span data-ttu-id="4777f-129">Runbooks måste toorun hello kontexten för en [Automation-konto](automation-security-overview.md) som finns i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4777f-129">Runbooks need toorun in hello context of an [Automation account](automation-security-overview.md) that is in your Azure subscription.</span></span> <span data-ttu-id="4777f-130">Du kan välja ett Automation-konto som du redan skapat eller du kan ha ett nytt Automation-konto för dig.</span><span class="sxs-lookup"><span data-stu-id="4777f-130">You can select an Automation account that you already created, or you can have a new Automation account created for you.</span></span>

<span data-ttu-id="4777f-131">Hej runbooks som tillhandahålls autentisera tooAzure med ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="4777f-131">hello runbooks that are provided authenticate tooAzure using a service principal.</span></span> <span data-ttu-id="4777f-132">Om du väljer toorun hello runbook i en av dina befintliga Automation-konton, skapas vi automatiskt hello service principal för dig.</span><span class="sxs-lookup"><span data-stu-id="4777f-132">If you choose toorun hello runbook in one of your existing Automation accounts, we will automatically create hello service principal for you.</span></span> <span data-ttu-id="4777f-133">Om du väljer toocreate ett nytt Automation-konto kan sedan skapar automatiskt vi hello-konto och hello tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="4777f-133">If you choose toocreate a new Automation account, then we will automatically create hello account and hello service principal.</span></span> <span data-ttu-id="4777f-134">I båda fallen två tillgångar skapas också i hello Automation-konto – ett med namnet certifikattillgång **AzureRunAsCertificate** och en anslutningstillgång med namnet **AzureRunAsConnection**.</span><span class="sxs-lookup"><span data-stu-id="4777f-134">In both cases, two assets will also be created in hello Automation account – a certificate asset named **AzureRunAsCertificate** and a connection asset named **AzureRunAsConnection**.</span></span> <span data-ttu-id="4777f-135">Hej runbooks använder **AzureRunAsConnection** tooauthenticate med Azure i ordning tooperform hello management-åtgärd mot hello VM.</span><span class="sxs-lookup"><span data-stu-id="4777f-135">hello runbooks will use **AzureRunAsConnection** tooauthenticate with Azure in order tooperform hello management action against hello VM.</span></span>

> [!NOTE]
> <span data-ttu-id="4777f-136">hello tjänstens huvudnamn skapas i hello prenumerationsomfattningen och hello deltagarrollen tilldelas.</span><span class="sxs-lookup"><span data-stu-id="4777f-136">hello service principal is created in hello subscription scope and is assigned hello Contributor role.</span></span> <span data-ttu-id="4777f-137">Den här rollen krävs för hello konto toohave behörighet toorun Automation runbooks toomanage virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="4777f-137">This role is required in order for hello account toohave permission toorun Automation runbooks toomanage Azure VMs.</span></span>  <span data-ttu-id="4777f-138">hello skapandet av ett Automaton konto eller tjänstens huvudnamn är en enstaka händelse.</span><span class="sxs-lookup"><span data-stu-id="4777f-138">hello creation of an Automaton account and/or service principal is a one-time event.</span></span> <span data-ttu-id="4777f-139">När de skapas, kan du använda detta konto toorun runbooks för andra Virtuella Azure-aviseringar.</span><span class="sxs-lookup"><span data-stu-id="4777f-139">Once they are created, you can use that account toorun runbooks for other Azure VM alerts.</span></span>
> 
> 

<span data-ttu-id="4777f-140">När du klickar på **OK** hello aviseringen har konfigurerats och om du har valt hello alternativet toocreate ett nytt Automation-konto skapas tillsammans med hello tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="4777f-140">When you click **OK** hello alert is configured and if you selected hello option toocreate a new Automation account, it is created along with hello service principal.</span></span>  <span data-ttu-id="4777f-141">Det kan ta några sekunder toocomplete.</span><span class="sxs-lookup"><span data-stu-id="4777f-141">This can take a few seconds toocomplete.</span></span>  

![Runbook som konfigureras](media/automation-azure-vm-alert-integration/RunbookBeingConfigured.png)

<span data-ttu-id="4777f-143">När hello konfigurationen har slutförts visas hello namnet på hello runbook visas i hello **lägga till en varningsregel** bladet.</span><span class="sxs-lookup"><span data-stu-id="4777f-143">After hello configuration is completed you will see hello name of hello runbook appear in hello **Add an alert rule** blade.</span></span>

![Runbook har konfigurerats](media/automation-azure-vm-alert-integration/RunbookConfigured.png)

<span data-ttu-id="4777f-145">Klicka på **OK** i hello **lägga till en varningsregel** bladet och hello varningsregeln ska skapas och aktivera om hello virtuella datorn är i körningstillstånd.</span><span class="sxs-lookup"><span data-stu-id="4777f-145">Click **OK** in hello **Add an alert rule** blade and hello alert rule will be created and activate if hello virtual machine is in a running state.</span></span>

### <a name="enable-or-disable-a-runbook"></a><span data-ttu-id="4777f-146">Aktivera eller inaktivera en runbook</span><span class="sxs-lookup"><span data-stu-id="4777f-146">Enable or disable a runbook</span></span>
<span data-ttu-id="4777f-147">Om du har en runbook som konfigurerats för en avisering kan inaktivera du det utan att ta bort hello konfiguration av runbook.</span><span class="sxs-lookup"><span data-stu-id="4777f-147">If you have a runbook configured for an alert, you can disable it without removing hello runbook configuration.</span></span> <span data-ttu-id="4777f-148">Detta kan du tookeep hello avisering körs och testa kanske vissa av hello Varningsregler och sedan återaktivera senare hello runbook.</span><span class="sxs-lookup"><span data-stu-id="4777f-148">This allows you tookeep hello alert running and perhaps test some of hello alert rules and then later re-enable hello runbook.</span></span>

## <a name="create-a-runbook-that-works-with-an-azure-alert"></a><span data-ttu-id="4777f-149">Skapa en runbook som fungerar med en Azure avisering</span><span class="sxs-lookup"><span data-stu-id="4777f-149">Create a runbook that works with an Azure alert</span></span>
<span data-ttu-id="4777f-150">När du väljer en runbook som en del av en Azure aviseringsregel måste hello runbook toohave logiken i den toomanage hello avisering data som överförs tooit.</span><span class="sxs-lookup"><span data-stu-id="4777f-150">When you choose a runbook as part of an Azure alert rule, hello runbook needs toohave logic in it toomanage hello alert data that is passed tooit.</span></span>  <span data-ttu-id="4777f-151">När en runbook är konfigurerad i en aviseringsregel, har en webhook skapats för hello runbook; Denna webhook är och sedan använda toostart hello runbook varje gång hello avisering utlösare.</span><span class="sxs-lookup"><span data-stu-id="4777f-151">When a runbook is configured in an alert rule, a webhook is created for hello runbook; that webhook is then used toostart hello runbook each time hello alert triggers.</span></span>  <span data-ttu-id="4777f-152">hello faktiska anropet toostart hello runbook är en HTTP POST-begäran toohello Webhooksadressen.</span><span class="sxs-lookup"><span data-stu-id="4777f-152">hello actual call toostart hello runbook is an HTTP POST request toohello webhook URL.</span></span> <span data-ttu-id="4777f-153">hello innehåller hello POST-begäran ett JSON-formaterad objekt som innehåller användbara egenskaper relaterade toohello avisering.</span><span class="sxs-lookup"><span data-stu-id="4777f-153">hello body of hello POST request contains a JSON-formated object that contains useful properties related toohello alert.</span></span>  <span data-ttu-id="4777f-154">Som du ser nedan innehåller hello aviseringsdata detaljer som subscriptionID, resourceGroupName, resourceName och resourceType.</span><span class="sxs-lookup"><span data-stu-id="4777f-154">As you can see below, hello alert data contains details like subscriptionID, resourceGroupName, resourceName, and resourceType.</span></span>

### <a name="example-of-alert-data"></a><span data-ttu-id="4777f-155">Exempel på aviseringsdata</span><span class="sxs-lookup"><span data-stu-id="4777f-155">Example of Alert data</span></span>
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

<span data-ttu-id="4777f-156">När hello Automation-webhook-tjänst tar emot hello HTTP POST extraherar hello aviseringsdata och skickar den toohello runbook i hello WebhookData runbookinmatningsparameter.</span><span class="sxs-lookup"><span data-stu-id="4777f-156">When hello Automation webhook service receives hello HTTP POST it extracts hello alert data and passes it toohello runbook in hello WebhookData runbook input parameter.</span></span>  <span data-ttu-id="4777f-157">Nedan finns en exempel-runbook som visar hur toouse hello WebhookData-parametern och extrahera hello aviseringsdata och använda den toomanage hello Azure-resurs som utlöste aviseringen hello.</span><span class="sxs-lookup"><span data-stu-id="4777f-157">Below is a sample runbook that shows how toouse hello WebhookData parameter and extract hello alert data and use it toomanage hello Azure resource that triggered hello alert.</span></span>

### <a name="example-runbook"></a><span data-ttu-id="4777f-158">Exempel-runbook</span><span class="sxs-lookup"><span data-stu-id="4777f-158">Example runbook</span></span>
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

## <a name="summary"></a><span data-ttu-id="4777f-159">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="4777f-159">Summary</span></span>
<span data-ttu-id="4777f-160">När du konfigurerar en avisering på en Azure VM, nu har du hello möjlighet tooeasily konfigurera en Automation runbook tooautomatically utföra Reparationsåtgärd när hello avisering utlöses.</span><span class="sxs-lookup"><span data-stu-id="4777f-160">When you configure an alert on an Azure VM, you now have hello ability tooeasily configure an Automation runbook tooautomatically perform remediation action when hello alert triggers.</span></span> <span data-ttu-id="4777f-161">För den här versionen, du kan välja bland runbooks toorestart stoppa eller ta bort en virtuell dator beroende på aviseringen scenario.</span><span class="sxs-lookup"><span data-stu-id="4777f-161">For this release, you can choose from runbooks toorestart, stop, or delete a VM depending on your alert scenario.</span></span> <span data-ttu-id="4777f-162">Detta är bara hello början för att aktivera scenarier där du kan bestämma hello åtgärder (meddelande, felsökning, reparation) som ska vidtas automatiskt när en avisering utlöses.</span><span class="sxs-lookup"><span data-stu-id="4777f-162">This is just hello beginning of enabling scenarios where you control hello actions (notification, troubleshooting, remediation) that will be taken automatically when an alert triggers.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4777f-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4777f-163">Next Steps</span></span>
* <span data-ttu-id="4777f-164">tooget igång med grafiska runbooks finns [min första grafisk runbook](automation-first-runbook-graphical.md)</span><span class="sxs-lookup"><span data-stu-id="4777f-164">tooget started with Graphical runbooks, see [My first graphical runbook](automation-first-runbook-graphical.md)</span></span>
* <span data-ttu-id="4777f-165">tooget igång med PowerShell arbetsflöde runbooks finns [min första PowerShell-arbetsflödesrunbook](automation-first-runbook-textual.md)</span><span class="sxs-lookup"><span data-stu-id="4777f-165">tooget started with PowerShell workflow runbooks, see [My first PowerShell workflow runbook](automation-first-runbook-textual.md)</span></span>
* <span data-ttu-id="4777f-166">toolearn mer om runbook-typer, fördelar och begränsningar finns [typer för Azure Automation-runbook](automation-runbook-types.md)</span><span class="sxs-lookup"><span data-stu-id="4777f-166">toolearn more about runbook types, their advantages and limitations, see [Azure Automation runbook types](automation-runbook-types.md)</span></span>

