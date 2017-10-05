---
title: "Lodrätt skala skalningsuppsättningar i virtuella Azure-datorn | Microsoft Docs"
description: "Hur man lodrätt skala en virtuell dator för att övervaka aviseringar med Azure Automation"
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: madhana
editor: 
tags: azure-resource-manager
ms.assetid: 16b17421-6b8f-483e-8a84-26327c44e9d3
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 08/03/2016
ms.author: guybo
ms.openlocfilehash: 9159a5a9041864fe06785829121233379c46bb03
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="vertical-autoscale-with-virtual-machine-scale-sets"></a><span data-ttu-id="41d3f-103">Lodrät Autoskala med skaluppsättningar för den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="41d3f-103">Vertical autoscale with Virtual Machine Scale sets</span></span>
<span data-ttu-id="41d3f-104">Den här artikeln beskriver hur man lodrätt skala Azure [Skalningsuppsättningar i virtuella](https://azure.microsoft.com/services/virtual-machine-scale-sets/) med eller utan reprovisioning.</span><span class="sxs-lookup"><span data-stu-id="41d3f-104">This article describes how to vertically scale Azure [Virtual Machine Scale Sets](https://azure.microsoft.com/services/virtual-machine-scale-sets/) with or without reprovisioning.</span></span> <span data-ttu-id="41d3f-105">Lodrät skalning för virtuella datorer som inte är i skalningsuppsättningar, finns i [lodrätt skala virtuella Azure-datorn med Azure Automation](../virtual-machines/windows/vertical-scaling-automation.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="41d3f-105">For vertical scaling of VMs which are not in scale sets, refer to [Vertically scale Azure virtual machine with Azure Automation](../virtual-machines/windows/vertical-scaling-automation.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="41d3f-106">Lodrät skalning, även kallat *skala upp* och *skala*, betyder att öka eller minska storlekar för virtuella datorer (VM) som svar på en arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="41d3f-106">Vertical scaling, also known as *scale up* and *scale down*, means increasing or decreasing virtual machine (VM) sizes in response to a workload.</span></span> <span data-ttu-id="41d3f-107">Jämför detta med [teckenbredden](virtual-machine-scale-sets-autoscale-overview.md), vilket även kallas *skala ut* och *skala i*, där antalet VMs ändras beroende på arbetsbelastningen.</span><span class="sxs-lookup"><span data-stu-id="41d3f-107">Compare this with [horizontal scaling](virtual-machine-scale-sets-autoscale-overview.md), also referred to as *scale out* and *scale in*, where the number of VMs is altered depending on the workload.</span></span>

<span data-ttu-id="41d3f-108">Reprovisioning innebär att ta bort en befintlig virtuell dator och ersätta den med en ny.</span><span class="sxs-lookup"><span data-stu-id="41d3f-108">Reprovisioning means removing an existing VM and replacing it with a new one.</span></span> <span data-ttu-id="41d3f-109">När du ökar eller minskar storleken på virtuella datorer i en Skaluppsättning i vissa fall vill du ändra storlek på befintliga virtuella datorer och behålla dina data, medan i andra fall måste du distribuera nya virtuella datorer i den nya storleken.</span><span class="sxs-lookup"><span data-stu-id="41d3f-109">When you increase or decrease the size of VMs in a VM Scale Set, in some cases you want to resize existing VMs and retain your data, while in other cases you need to deploy new VMs of the new size.</span></span> <span data-ttu-id="41d3f-110">Det här dokumentet beskriver båda fallen.</span><span class="sxs-lookup"><span data-stu-id="41d3f-110">This document covers both cases.</span></span>

<span data-ttu-id="41d3f-111">Lodrät skalning kan vara användbart när:</span><span class="sxs-lookup"><span data-stu-id="41d3f-111">Vertical scaling can be useful when:</span></span>

* <span data-ttu-id="41d3f-112">En tjänst som bygger på virtuella datorer är outnyttjad (till exempel på helger).</span><span class="sxs-lookup"><span data-stu-id="41d3f-112">A service built on virtual machines is under-utilized (for example at weekends).</span></span> <span data-ttu-id="41d3f-113">Minska VM-storlek kan minska kostnaderna för varje månad.</span><span class="sxs-lookup"><span data-stu-id="41d3f-113">Reducing the VM size can reduce monthly costs.</span></span>
* <span data-ttu-id="41d3f-114">Öka storleken på virtuella datorn ska kunna hantera större begäran utan att skapa ytterligare virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="41d3f-114">Increasing VM size to cope with larger demand without creating additional VMs.</span></span>

<span data-ttu-id="41d3f-115">Du kan ställa in lodräta skalning ska utlösta baserat på mått baserade aviseringar från din Skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="41d3f-115">You can set up vertical scaling to be triggered based on metric based alerts from your VM Scale Set.</span></span> <span data-ttu-id="41d3f-116">När aviseringen aktiveras utlöses en webhook som utlösare för en runbook som kan skalas nivå ange uppåt eller nedåt.</span><span class="sxs-lookup"><span data-stu-id="41d3f-116">When the alert is activated it fires a webhook that triggers a runbook which can scale your scale set up or down.</span></span> <span data-ttu-id="41d3f-117">Lodrät skalning kan konfigureras genom att följa dessa steg:</span><span class="sxs-lookup"><span data-stu-id="41d3f-117">Vertical scaling can be configured by following these steps:</span></span>

1. <span data-ttu-id="41d3f-118">Skapa ett Azure Automation-konto med Kör som-kapacitet.</span><span class="sxs-lookup"><span data-stu-id="41d3f-118">Create an Azure Automation account with run-as capability.</span></span>
2. <span data-ttu-id="41d3f-119">Importera Lodrät skala för Azure Automation-runbooks för Skalningsuppsättningar i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="41d3f-119">Import Azure Automation Vertical Scale runbooks for VM Scale Sets into your subscription.</span></span>
3. <span data-ttu-id="41d3f-120">Lägg till en webhook i din runbook.</span><span class="sxs-lookup"><span data-stu-id="41d3f-120">Add a webhook to your runbook.</span></span>
4. <span data-ttu-id="41d3f-121">Lägga till en avisering till din VM Scale Set med hjälp av ett webhook-meddelande.</span><span class="sxs-lookup"><span data-stu-id="41d3f-121">Add an alert to your VM Scale Set using a webhook notification.</span></span>

> [!NOTE]
> <span data-ttu-id="41d3f-122">Lodrät autoskalning kan bara ske inom vissa intervall för VM-storlekar.</span><span class="sxs-lookup"><span data-stu-id="41d3f-122">Vertical autoscaling can only take place within certain ranges of VM sizes.</span></span> <span data-ttu-id="41d3f-123">Jämför specifikationerna för varje storlek innan du beslutar att skala från varandra (högre nummer inte alltid anger större VM-storlek).</span><span class="sxs-lookup"><span data-stu-id="41d3f-123">Compare the specifications of each size before deciding to scale from one to another (higher number does not always indicate bigger VM size).</span></span> <span data-ttu-id="41d3f-124">Du kan välja att skala mellan de följande paren storlekar:</span><span class="sxs-lookup"><span data-stu-id="41d3f-124">You can choose to scale between the following pairs of sizes:</span></span>
> 
> | <span data-ttu-id="41d3f-125">VM-storlekar skalning par</span><span class="sxs-lookup"><span data-stu-id="41d3f-125">VM sizes scaling pair</span></span> |  |
> | --- | --- |
> | <span data-ttu-id="41d3f-126">Standard_A0</span><span class="sxs-lookup"><span data-stu-id="41d3f-126">Standard_A0</span></span> |<span data-ttu-id="41d3f-127">Standard_A11</span><span class="sxs-lookup"><span data-stu-id="41d3f-127">Standard_A11</span></span> |
> | <span data-ttu-id="41d3f-128">Standard_D1</span><span class="sxs-lookup"><span data-stu-id="41d3f-128">Standard_D1</span></span> |<span data-ttu-id="41d3f-129">Standard_D14</span><span class="sxs-lookup"><span data-stu-id="41d3f-129">Standard_D14</span></span> |
> | <span data-ttu-id="41d3f-130">Standard_DS1</span><span class="sxs-lookup"><span data-stu-id="41d3f-130">Standard_DS1</span></span> |<span data-ttu-id="41d3f-131">Standard_DS14</span><span class="sxs-lookup"><span data-stu-id="41d3f-131">Standard_DS14</span></span> |
> | <span data-ttu-id="41d3f-132">Standard_D1v2</span><span class="sxs-lookup"><span data-stu-id="41d3f-132">Standard_D1v2</span></span> |<span data-ttu-id="41d3f-133">Standard_D15v2</span><span class="sxs-lookup"><span data-stu-id="41d3f-133">Standard_D15v2</span></span> |
> | <span data-ttu-id="41d3f-134">Standard_G1</span><span class="sxs-lookup"><span data-stu-id="41d3f-134">Standard_G1</span></span> |<span data-ttu-id="41d3f-135">Standard_G5</span><span class="sxs-lookup"><span data-stu-id="41d3f-135">Standard_G5</span></span> |
> | <span data-ttu-id="41d3f-136">Standard_GS1</span><span class="sxs-lookup"><span data-stu-id="41d3f-136">Standard_GS1</span></span> |<span data-ttu-id="41d3f-137">Standard_GS5</span><span class="sxs-lookup"><span data-stu-id="41d3f-137">Standard_GS5</span></span> |
> 
> 

## <a name="create-an-azure-automation-account-with-run-as-capability"></a><span data-ttu-id="41d3f-138">Skapa ett Azure Automation-konto med Kör som-kapacitet</span><span class="sxs-lookup"><span data-stu-id="41d3f-138">Create an Azure Automation Account with run-as capability</span></span>
<span data-ttu-id="41d3f-139">Det första du behöver göra är att skapa ett Azure Automation-konto som ska vara värd för runbooks som används för att anpassa VM Scale Set-instanser.</span><span class="sxs-lookup"><span data-stu-id="41d3f-139">The first thing you need to do is create an Azure Automation account that will host the runbooks used to scale the VM Scale Set instances.</span></span> <span data-ttu-id="41d3f-140">Nyligen [Azure Automation](https://azure.microsoft.com/services/automation/) introducerade funktionen ”Kör som-konto” som gör att du kan konfigurera tjänstens huvudnamn för att automatiskt köra runbooks på en användares vägnar mycket enkelt.</span><span class="sxs-lookup"><span data-stu-id="41d3f-140">Recently [Azure Automation](https://azure.microsoft.com/services/automation/) introduced the "Run As account" feature which makes setting up the Service Principal for automatically running the runbooks on a user's behalf very easy.</span></span> <span data-ttu-id="41d3f-141">Mer information finns i artikeln nedan:</span><span class="sxs-lookup"><span data-stu-id="41d3f-141">You can read more about this in the article below:</span></span>

* [<span data-ttu-id="41d3f-142">Autentisera runbooks med ett ”Kör som”-konto i Azure</span><span class="sxs-lookup"><span data-stu-id="41d3f-142">Authenticate Runbooks with Azure Run As account</span></span>](../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-azure-automation-vertical-scale-runbooks-into-your-subscription"></a><span data-ttu-id="41d3f-143">Importera Lodrät skala för Azure Automation-runbooks till din prenumeration</span><span class="sxs-lookup"><span data-stu-id="41d3f-143">Import Azure Automation Vertical Scale runbooks into your subscription</span></span>
<span data-ttu-id="41d3f-144">Runbooks som behövs för att skala din Skalningsuppsättningar lodrätt har redan publicerats i Azure Automation Runbook-galleriet.</span><span class="sxs-lookup"><span data-stu-id="41d3f-144">The runbooks needed to vertically scale your VM Scale Sets are already published in the Azure Automation Runbook Gallery.</span></span> <span data-ttu-id="41d3f-145">Om du vill importera följer dem till din prenumeration du stegen i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="41d3f-145">To import them into your subscription follow the steps in this article:</span></span>

* [<span data-ttu-id="41d3f-146">Azure Automation Runbook- och stänga</span><span class="sxs-lookup"><span data-stu-id="41d3f-146">Runbook and module galleries for Azure Automation</span></span>](../automation/automation-runbook-gallery.md)

<span data-ttu-id="41d3f-147">Välj alternativet Bläddra galleriet Runbooks-menyn:</span><span class="sxs-lookup"><span data-stu-id="41d3f-147">Choose the Browse Gallery option from the Runbooks menu:</span></span>

![Runbooks som ska importeras][runbooks]

<span data-ttu-id="41d3f-149">Runbooks som ska importeras visas.</span><span class="sxs-lookup"><span data-stu-id="41d3f-149">The runbooks that need to be imported are shown.</span></span> <span data-ttu-id="41d3f-150">Välj runbook baserat på om du vill lodräta skalning med eller utan reprovisioning:</span><span class="sxs-lookup"><span data-stu-id="41d3f-150">Select the runbook based on whether you want vertical scaling with or without reprovisioning:</span></span>

![Runbooks-galleriet][gallery]

## <a name="add-a-webhook-to-your-runbook"></a><span data-ttu-id="41d3f-152">Lägga till en webhook i din runbook</span><span class="sxs-lookup"><span data-stu-id="41d3f-152">Add a webhook to your runbook</span></span>
<span data-ttu-id="41d3f-153">När du har importerat förse runbooks måste du en webhook runbook så att den kan aktiveras av en avisering från en Skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="41d3f-153">Once you've imported the runbooks you'll need to add a webhook to the runbook so it can be triggered by an alert from a VM Scale Set.</span></span> <span data-ttu-id="41d3f-154">Information om hur du skapar en webhook för din Runbook beskrivs i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="41d3f-154">The details of creating a webhook for your Runbook are described in this article:</span></span>

* [<span data-ttu-id="41d3f-155">Azure Automation-webhooks</span><span class="sxs-lookup"><span data-stu-id="41d3f-155">Azure Automation webhooks</span></span>](../automation/automation-webhooks.md)

> [!NOTE]
> <span data-ttu-id="41d3f-156">Kontrollera att du kopierar webhook URI innan du stänger dialogrutan webhook som du behöver det i nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="41d3f-156">Make sure you copy the webhook URI before closing the webhook dialog as you will need this in the next section.</span></span>
> 
> 

## <a name="add-an-alert-to-your-vm-scale-set"></a><span data-ttu-id="41d3f-157">Lägga till en avisering till din Skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="41d3f-157">Add an alert to your VM Scale Set</span></span>
<span data-ttu-id="41d3f-158">Nedan finns ett PowerShell-skript som visar hur du lägger till en avisering till en Skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="41d3f-158">Below is a PowerShell script which shows how to add an alert to a VM Scale Set.</span></span> <span data-ttu-id="41d3f-159">Finns i följande artikel för att hämta namnet på måttet ska utlösa aviseringen på: [Azure-Monitor autoskalning vanliga mått](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="41d3f-159">Refer to the following article to get the name of the metric to fire the alert on: [Azure Monitor autoscaling common metrics](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).</span></span>

```
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail user@contoso.com
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri <uri-of-the-webhook>
$threshold = <value-of-the-threshold>
$rg = <resource-group-name>
$id = <resource-id-to-add-the-alert-to>
$location = <location-of-the-resource>
$alertName = <name-of-the-resource>
$metricName = <metric-to-fire-the-alert-on>
$timeWindow = <time-window-in-hh:mm:ss-format>
$condition = <condition-for-the-threshold> # Other valid values are LessThanOrEqual, GreaterThan, GreaterThanOrEqual
$description = <description-for-the-alert>

Add-AzureRmMetricAlertRule  -Name  $alertName `
                            -Location  $location `
                            -ResourceGroup $rg `
                            -TargetResourceId $id `
                            -MetricName $metricName `
                            -Operator  $condition `
                            -Threshold $threshold `
                            -WindowSize  $timeWindow `
                            -TimeAggregationOperator Average `
                            -Actions $actionEmail, $actionWebhook `
                            -Description $description
```

> [!NOTE]
> <span data-ttu-id="41d3f-160">Det rekommenderas att konfigurera en rimlig tidsfönster för aviseringen för att undvika utlösa lodräta skalning och alla associerade avbrott, för ofta.</span><span class="sxs-lookup"><span data-stu-id="41d3f-160">It is recommended to configure a reasonable time window for the alert in order to avoid triggering vertical scaling, and any associated service interruption, too often.</span></span> <span data-ttu-id="41d3f-161">Överväg ett fönster med minst 20 – 30 minuter eller mer.</span><span class="sxs-lookup"><span data-stu-id="41d3f-161">Consider a window of least 20-30 minutes or more.</span></span> <span data-ttu-id="41d3f-162">Överväg att vågräta skalning om du vill undvika avbrott.</span><span class="sxs-lookup"><span data-stu-id="41d3f-162">Consider horizontal scaling if you need to avoid any interruption.</span></span>
> 
> 

<span data-ttu-id="41d3f-163">Mer information om hur du skapar aviseringar finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="41d3f-163">For more information on how to create alerts refer to the following articles:</span></span>

* [<span data-ttu-id="41d3f-164">Azure-Monitor PowerShell Snabbstart-exempel</span><span class="sxs-lookup"><span data-stu-id="41d3f-164">Azure Monitor PowerShell quick start samples</span></span>](../monitoring-and-diagnostics/insights-powershell-samples.md)
* [<span data-ttu-id="41d3f-165">Azure CLI för övervakaren plattformsoberoende Snabbstart-exempel</span><span class="sxs-lookup"><span data-stu-id="41d3f-165">Azure Monitor Cross-platform CLI quick start samples</span></span>](../monitoring-and-diagnostics/insights-cli-samples.md)

## <a name="summary"></a><span data-ttu-id="41d3f-166">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="41d3f-166">Summary</span></span>
<span data-ttu-id="41d3f-167">Den här artikeln visade enkla lodräta skalning exempel.</span><span class="sxs-lookup"><span data-stu-id="41d3f-167">This article showed simple vertical scaling examples.</span></span> <span data-ttu-id="41d3f-168">Du kan ansluta många olika händelser med en anpassad uppsättning åtgärder med byggblocken - Automation-konto, runbooks, webhooks, aviseringar.</span><span class="sxs-lookup"><span data-stu-id="41d3f-168">With these building blocks - Automation account, runbooks, webhooks, alerts - you can connect a rich variety of events with a customized set of actions.</span></span>

[runbooks]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks.png
[gallery]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks-gallery.png
