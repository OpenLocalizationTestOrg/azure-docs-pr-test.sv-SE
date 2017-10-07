---
title: "skaluppsättningar för aaaVertically skala virtuell Azure-dator | Microsoft Docs"
description: Hur toovertically skala en virtuell dator i svaret toomonitoring aviseringar med Azure Automation
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
ms.openlocfilehash: 1cc35a805b6a5742252a89c21588ca451ff547a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="vertical-autoscale-with-virtual-machine-scale-sets"></a><span data-ttu-id="5872e-103">Lodrät Autoskala med skaluppsättningar för den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="5872e-103">Vertical autoscale with Virtual Machine Scale sets</span></span>
<span data-ttu-id="5872e-104">Den här artikeln beskriver hur toovertically skala Azure [Skalningsuppsättningar i virtuella](https://azure.microsoft.com/services/virtual-machine-scale-sets/) med eller utan reprovisioning.</span><span class="sxs-lookup"><span data-stu-id="5872e-104">This article describes how toovertically scale Azure [Virtual Machine Scale Sets](https://azure.microsoft.com/services/virtual-machine-scale-sets/) with or without reprovisioning.</span></span> <span data-ttu-id="5872e-105">Lodrät skalning av virtuella datorer som inte kommer i skalningsuppsättningar finns för[lodrätt skala virtuella Azure-datorn med Azure Automation](../virtual-machines/windows/vertical-scaling-automation.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5872e-105">For vertical scaling of VMs which are not in scale sets, refer too[Vertically scale Azure virtual machine with Azure Automation](../virtual-machines/windows/vertical-scaling-automation.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="5872e-106">Lodrät skalning, även kallat *skala upp* och *skala*, betyder att öka eller minska storlekar för virtuella datorer (VM) i svaret tooa arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="5872e-106">Vertical scaling, also known as *scale up* and *scale down*, means increasing or decreasing virtual machine (VM) sizes in response tooa workload.</span></span> <span data-ttu-id="5872e-107">Jämför detta med [teckenbredden](virtual-machine-scale-sets-autoscale-overview.md), kallas även tooas *skala ut* och *skala i*, där hello antal virtuella datorer ändras beroende på hello arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="5872e-107">Compare this with [horizontal scaling](virtual-machine-scale-sets-autoscale-overview.md), also referred tooas *scale out* and *scale in*, where hello number of VMs is altered depending on hello workload.</span></span>

<span data-ttu-id="5872e-108">Reprovisioning innebär att ta bort en befintlig virtuell dator och ersätta den med en ny.</span><span class="sxs-lookup"><span data-stu-id="5872e-108">Reprovisioning means removing an existing VM and replacing it with a new one.</span></span> <span data-ttu-id="5872e-109">När du ökar eller minskar hello storleken på virtuella datorer i en Skaluppsättning i vissa fall du vill tooresize befintliga virtuella datorer och behålla dina data, medan i andra fall behöver du toodeploy nya virtuella datorer ny hello-storlek.</span><span class="sxs-lookup"><span data-stu-id="5872e-109">When you increase or decrease hello size of VMs in a VM Scale Set, in some cases you want tooresize existing VMs and retain your data, while in other cases you need toodeploy new VMs of hello new size.</span></span> <span data-ttu-id="5872e-110">Det här dokumentet beskriver båda fallen.</span><span class="sxs-lookup"><span data-stu-id="5872e-110">This document covers both cases.</span></span>

<span data-ttu-id="5872e-111">Lodrät skalning kan vara användbart när:</span><span class="sxs-lookup"><span data-stu-id="5872e-111">Vertical scaling can be useful when:</span></span>

* <span data-ttu-id="5872e-112">En tjänst som bygger på virtuella datorer är outnyttjad (till exempel på helger).</span><span class="sxs-lookup"><span data-stu-id="5872e-112">A service built on virtual machines is under-utilized (for example at weekends).</span></span> <span data-ttu-id="5872e-113">Minska hello VM-storlek kan minska kostnaderna för varje månad.</span><span class="sxs-lookup"><span data-stu-id="5872e-113">Reducing hello VM size can reduce monthly costs.</span></span>
* <span data-ttu-id="5872e-114">Ökad VM-storlek toocope med större behov utan att skapa ytterligare virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="5872e-114">Increasing VM size toocope with larger demand without creating additional VMs.</span></span>

<span data-ttu-id="5872e-115">Du kan ställa in lodrät skalning toobe utlöses baserat på mått baserade aviseringar från din Skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="5872e-115">You can set up vertical scaling toobe triggered based on metric based alerts from your VM Scale Set.</span></span> <span data-ttu-id="5872e-116">När hello avisering aktiveras utlöses en webhook som utlösare för en runbook som kan skalas nivå ange uppåt eller nedåt.</span><span class="sxs-lookup"><span data-stu-id="5872e-116">When hello alert is activated it fires a webhook that triggers a runbook which can scale your scale set up or down.</span></span> <span data-ttu-id="5872e-117">Lodrät skalning kan konfigureras genom att följa dessa steg:</span><span class="sxs-lookup"><span data-stu-id="5872e-117">Vertical scaling can be configured by following these steps:</span></span>

1. <span data-ttu-id="5872e-118">Skapa ett Azure Automation-konto med Kör som-kapacitet.</span><span class="sxs-lookup"><span data-stu-id="5872e-118">Create an Azure Automation account with run-as capability.</span></span>
2. <span data-ttu-id="5872e-119">Importera Lodrät skala för Azure Automation-runbooks för Skalningsuppsättningar i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="5872e-119">Import Azure Automation Vertical Scale runbooks for VM Scale Sets into your subscription.</span></span>
3. <span data-ttu-id="5872e-120">Lägg till en webhook tooyour runbook.</span><span class="sxs-lookup"><span data-stu-id="5872e-120">Add a webhook tooyour runbook.</span></span>
4. <span data-ttu-id="5872e-121">Lägga till en avisering tooyour VM Scale Set med hjälp av ett webhook-meddelande.</span><span class="sxs-lookup"><span data-stu-id="5872e-121">Add an alert tooyour VM Scale Set using a webhook notification.</span></span>

> [!NOTE]
> <span data-ttu-id="5872e-122">Lodrät autoskalning kan bara ske inom vissa intervall för VM-storlekar.</span><span class="sxs-lookup"><span data-stu-id="5872e-122">Vertical autoscaling can only take place within certain ranges of VM sizes.</span></span> <span data-ttu-id="5872e-123">Jämför hello specifikationer av varje storlek innan du bestämmer tooscale från en tooanother (högre nummer inte alltid anger större VM-storlek).</span><span class="sxs-lookup"><span data-stu-id="5872e-123">Compare hello specifications of each size before deciding tooscale from one tooanother (higher number does not always indicate bigger VM size).</span></span> <span data-ttu-id="5872e-124">Du kan välja tooscale mellan hello par med följande:</span><span class="sxs-lookup"><span data-stu-id="5872e-124">You can choose tooscale between hello following pairs of sizes:</span></span>
> 
> | <span data-ttu-id="5872e-125">VM-storlekar skalning par</span><span class="sxs-lookup"><span data-stu-id="5872e-125">VM sizes scaling pair</span></span> |  |
> | --- | --- |
> | <span data-ttu-id="5872e-126">Standard_A0</span><span class="sxs-lookup"><span data-stu-id="5872e-126">Standard_A0</span></span> |<span data-ttu-id="5872e-127">Standard_A11</span><span class="sxs-lookup"><span data-stu-id="5872e-127">Standard_A11</span></span> |
> | <span data-ttu-id="5872e-128">Standard_D1</span><span class="sxs-lookup"><span data-stu-id="5872e-128">Standard_D1</span></span> |<span data-ttu-id="5872e-129">Standard_D14</span><span class="sxs-lookup"><span data-stu-id="5872e-129">Standard_D14</span></span> |
> | <span data-ttu-id="5872e-130">Standard_DS1</span><span class="sxs-lookup"><span data-stu-id="5872e-130">Standard_DS1</span></span> |<span data-ttu-id="5872e-131">Standard_DS14</span><span class="sxs-lookup"><span data-stu-id="5872e-131">Standard_DS14</span></span> |
> | <span data-ttu-id="5872e-132">Standard_D1v2</span><span class="sxs-lookup"><span data-stu-id="5872e-132">Standard_D1v2</span></span> |<span data-ttu-id="5872e-133">Standard_D15v2</span><span class="sxs-lookup"><span data-stu-id="5872e-133">Standard_D15v2</span></span> |
> | <span data-ttu-id="5872e-134">Standard_G1</span><span class="sxs-lookup"><span data-stu-id="5872e-134">Standard_G1</span></span> |<span data-ttu-id="5872e-135">Standard_G5</span><span class="sxs-lookup"><span data-stu-id="5872e-135">Standard_G5</span></span> |
> | <span data-ttu-id="5872e-136">Standard_GS1</span><span class="sxs-lookup"><span data-stu-id="5872e-136">Standard_GS1</span></span> |<span data-ttu-id="5872e-137">Standard_GS5</span><span class="sxs-lookup"><span data-stu-id="5872e-137">Standard_GS5</span></span> |
> 
> 

## <a name="create-an-azure-automation-account-with-run-as-capability"></a><span data-ttu-id="5872e-138">Skapa ett Azure Automation-konto med Kör som-kapacitet</span><span class="sxs-lookup"><span data-stu-id="5872e-138">Create an Azure Automation Account with run-as capability</span></span>
<span data-ttu-id="5872e-139">hello måste du först toodo är att skapa ett Azure Automation-konto som ska vara värd för hello runbooks används tooscale hello VM Scale Set instanser.</span><span class="sxs-lookup"><span data-stu-id="5872e-139">hello first thing you need toodo is create an Azure Automation account that will host hello runbooks used tooscale hello VM Scale Set instances.</span></span> <span data-ttu-id="5872e-140">Nyligen [Azure Automation](https://azure.microsoft.com/services/automation/) introduceras hello ”kör som-konto” funktionen som gör att konfigurera hello tjänstens huvudnamn för att automatiskt köra hello runbooks på en användares vägnar mycket enkelt.</span><span class="sxs-lookup"><span data-stu-id="5872e-140">Recently [Azure Automation](https://azure.microsoft.com/services/automation/) introduced hello "Run As account" feature which makes setting up hello Service Principal for automatically running hello runbooks on a user's behalf very easy.</span></span> <span data-ttu-id="5872e-141">Du kan läsa mer om detta i hello artikel nedan:</span><span class="sxs-lookup"><span data-stu-id="5872e-141">You can read more about this in hello article below:</span></span>

* [<span data-ttu-id="5872e-142">Autentisera runbooks med ett ”Kör som”-konto i Azure</span><span class="sxs-lookup"><span data-stu-id="5872e-142">Authenticate Runbooks with Azure Run As account</span></span>](../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-azure-automation-vertical-scale-runbooks-into-your-subscription"></a><span data-ttu-id="5872e-143">Importera Lodrät skala för Azure Automation-runbooks till din prenumeration</span><span class="sxs-lookup"><span data-stu-id="5872e-143">Import Azure Automation Vertical Scale runbooks into your subscription</span></span>
<span data-ttu-id="5872e-144">Hej runbooks behövs toovertically skala din Skalningsuppsättningar har redan publicerats i hello Azure Automation Runbook-galleriet.</span><span class="sxs-lookup"><span data-stu-id="5872e-144">hello runbooks needed toovertically scale your VM Scale Sets are already published in hello Azure Automation Runbook Gallery.</span></span> <span data-ttu-id="5872e-145">tooimport dem till din prenumeration följer hello stegen i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="5872e-145">tooimport them into your subscription follow hello steps in this article:</span></span>

* [<span data-ttu-id="5872e-146">Azure Automation Runbook- och stänga</span><span class="sxs-lookup"><span data-stu-id="5872e-146">Runbook and module galleries for Azure Automation</span></span>](../automation/automation-runbook-gallery.md)

<span data-ttu-id="5872e-147">Välj alternativ för hello Bläddra galleriet hello Runbooks menyn:</span><span class="sxs-lookup"><span data-stu-id="5872e-147">Choose hello Browse Gallery option from hello Runbooks menu:</span></span>

![Runbooks toobe importeras][runbooks]

<span data-ttu-id="5872e-149">Hej runbooks som måste importeras toobe visas.</span><span class="sxs-lookup"><span data-stu-id="5872e-149">hello runbooks that need toobe imported are shown.</span></span> <span data-ttu-id="5872e-150">Välj hello runbook baserat på om du vill lodräta skalning med eller utan reprovisioning:</span><span class="sxs-lookup"><span data-stu-id="5872e-150">Select hello runbook based on whether you want vertical scaling with or without reprovisioning:</span></span>

![Runbooks-galleriet][gallery]

## <a name="add-a-webhook-tooyour-runbook"></a><span data-ttu-id="5872e-152">Lägga till en webhook tooyour runbook</span><span class="sxs-lookup"><span data-stu-id="5872e-152">Add a webhook tooyour runbook</span></span>
<span data-ttu-id="5872e-153">När du har importerat hello runbooks måste tooadd en webhook toohello runbook så att den kan aktiveras av en avisering från en Skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="5872e-153">Once you've imported hello runbooks you'll need tooadd a webhook toohello runbook so it can be triggered by an alert from a VM Scale Set.</span></span> <span data-ttu-id="5872e-154">hello information om hur du skapar en webhook för din Runbook beskrivs i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="5872e-154">hello details of creating a webhook for your Runbook are described in this article:</span></span>

* [<span data-ttu-id="5872e-155">Azure Automation-webhooks</span><span class="sxs-lookup"><span data-stu-id="5872e-155">Azure Automation webhooks</span></span>](../automation/automation-webhooks.md)

> [!NOTE]
> <span data-ttu-id="5872e-156">Kontrollera att du kopierar hello webhook URI innan du stänger hello webhook dialogrutan som du behöver det i nästa avsnitt om hello.</span><span class="sxs-lookup"><span data-stu-id="5872e-156">Make sure you copy hello webhook URI before closing hello webhook dialog as you will need this in hello next section.</span></span>
> 
> 

## <a name="add-an-alert-tooyour-vm-scale-set"></a><span data-ttu-id="5872e-157">Lägg till en avisering tooyour Skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="5872e-157">Add an alert tooyour VM Scale Set</span></span>
<span data-ttu-id="5872e-158">Nedan finns ett PowerShell-skript som visar hur tooadd en avisering tooa Skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="5872e-158">Below is a PowerShell script which shows how tooadd an alert tooa VM Scale Set.</span></span> <span data-ttu-id="5872e-159">Referera toohello efter artikel tooget hello namnet på hello mått toofire hello avisering: [Azure-Monitor autoskalning vanliga mått](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="5872e-159">Refer toohello following article tooget hello name of hello metric toofire hello alert on: [Azure Monitor autoscaling common metrics](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).</span></span>

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
> <span data-ttu-id="5872e-160">Är det rekommenderade tooconfigure en rimlig tidsfönster för hello avisering i ordning tooavoid utlösa lodräta skalning och alla associerade avbrott i tjänsten, för ofta.</span><span class="sxs-lookup"><span data-stu-id="5872e-160">It is recommended tooconfigure a reasonable time window for hello alert in order tooavoid triggering vertical scaling, and any associated service interruption, too often.</span></span> <span data-ttu-id="5872e-161">Överväg ett fönster med minst 20 – 30 minuter eller mer.</span><span class="sxs-lookup"><span data-stu-id="5872e-161">Consider a window of least 20-30 minutes or more.</span></span> <span data-ttu-id="5872e-162">Överväg att vågräta skalning om du behöver tooavoid avbrott.</span><span class="sxs-lookup"><span data-stu-id="5872e-162">Consider horizontal scaling if you need tooavoid any interruption.</span></span>
> 
> 

<span data-ttu-id="5872e-163">Mer information om hur toocreate aviseringar finns toohello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="5872e-163">For more information on how toocreate alerts refer toohello following articles:</span></span>

* [<span data-ttu-id="5872e-164">Azure-Monitor PowerShell Snabbstart-exempel</span><span class="sxs-lookup"><span data-stu-id="5872e-164">Azure Monitor PowerShell quick start samples</span></span>](../monitoring-and-diagnostics/insights-powershell-samples.md)
* [<span data-ttu-id="5872e-165">Azure CLI för övervakaren plattformsoberoende Snabbstart-exempel</span><span class="sxs-lookup"><span data-stu-id="5872e-165">Azure Monitor Cross-platform CLI quick start samples</span></span>](../monitoring-and-diagnostics/insights-cli-samples.md)

## <a name="summary"></a><span data-ttu-id="5872e-166">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="5872e-166">Summary</span></span>
<span data-ttu-id="5872e-167">Den här artikeln visade enkla lodräta skalning exempel.</span><span class="sxs-lookup"><span data-stu-id="5872e-167">This article showed simple vertical scaling examples.</span></span> <span data-ttu-id="5872e-168">Du kan ansluta många olika händelser med en anpassad uppsättning åtgärder med byggblocken - Automation-konto, runbooks, webhooks, aviseringar.</span><span class="sxs-lookup"><span data-stu-id="5872e-168">With these building blocks - Automation account, runbooks, webhooks, alerts - you can connect a rich variety of events with a customized set of actions.</span></span>

[runbooks]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks.png
[gallery]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks-gallery.png
