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
# <a name="vertical-autoscale-with-virtual-machine-scale-sets"></a>Lodrät Autoskala med skaluppsättningar för den virtuella datorn
Den här artikeln beskriver hur toovertically skala Azure [Skalningsuppsättningar i virtuella](https://azure.microsoft.com/services/virtual-machine-scale-sets/) med eller utan reprovisioning. Lodrät skalning av virtuella datorer som inte kommer i skalningsuppsättningar finns för[lodrätt skala virtuella Azure-datorn med Azure Automation](../virtual-machines/windows/vertical-scaling-automation.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Lodrät skalning, även kallat *skala upp* och *skala*, betyder att öka eller minska storlekar för virtuella datorer (VM) i svaret tooa arbetsbelastning. Jämför detta med [teckenbredden](virtual-machine-scale-sets-autoscale-overview.md), kallas även tooas *skala ut* och *skala i*, där hello antal virtuella datorer ändras beroende på hello arbetsbelastning.

Reprovisioning innebär att ta bort en befintlig virtuell dator och ersätta den med en ny. När du ökar eller minskar hello storleken på virtuella datorer i en Skaluppsättning i vissa fall du vill tooresize befintliga virtuella datorer och behålla dina data, medan i andra fall behöver du toodeploy nya virtuella datorer ny hello-storlek. Det här dokumentet beskriver båda fallen.

Lodrät skalning kan vara användbart när:

* En tjänst som bygger på virtuella datorer är outnyttjad (till exempel på helger). Minska hello VM-storlek kan minska kostnaderna för varje månad.
* Ökad VM-storlek toocope med större behov utan att skapa ytterligare virtuella datorer.

Du kan ställa in lodrät skalning toobe utlöses baserat på mått baserade aviseringar från din Skaluppsättning. När hello avisering aktiveras utlöses en webhook som utlösare för en runbook som kan skalas nivå ange uppåt eller nedåt. Lodrät skalning kan konfigureras genom att följa dessa steg:

1. Skapa ett Azure Automation-konto med Kör som-kapacitet.
2. Importera Lodrät skala för Azure Automation-runbooks för Skalningsuppsättningar i din prenumeration.
3. Lägg till en webhook tooyour runbook.
4. Lägga till en avisering tooyour VM Scale Set med hjälp av ett webhook-meddelande.

> [!NOTE]
> Lodrät autoskalning kan bara ske inom vissa intervall för VM-storlekar. Jämför hello specifikationer av varje storlek innan du bestämmer tooscale från en tooanother (högre nummer inte alltid anger större VM-storlek). Du kan välja tooscale mellan hello par med följande:
> 
> | VM-storlekar skalning par |  |
> | --- | --- |
> | Standard_A0 |Standard_A11 |
> | Standard_D1 |Standard_D14 |
> | Standard_DS1 |Standard_DS14 |
> | Standard_D1v2 |Standard_D15v2 |
> | Standard_G1 |Standard_G5 |
> | Standard_GS1 |Standard_GS5 |
> 
> 

## <a name="create-an-azure-automation-account-with-run-as-capability"></a>Skapa ett Azure Automation-konto med Kör som-kapacitet
hello måste du först toodo är att skapa ett Azure Automation-konto som ska vara värd för hello runbooks används tooscale hello VM Scale Set instanser. Nyligen [Azure Automation](https://azure.microsoft.com/services/automation/) introduceras hello ”kör som-konto” funktionen som gör att konfigurera hello tjänstens huvudnamn för att automatiskt köra hello runbooks på en användares vägnar mycket enkelt. Du kan läsa mer om detta i hello artikel nedan:

* [Autentisera runbooks med ett ”Kör som”-konto i Azure](../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Importera Lodrät skala för Azure Automation-runbooks till din prenumeration
Hej runbooks behövs toovertically skala din Skalningsuppsättningar har redan publicerats i hello Azure Automation Runbook-galleriet. tooimport dem till din prenumeration följer hello stegen i den här artikeln:

* [Azure Automation Runbook- och stänga](../automation/automation-runbook-gallery.md)

Välj alternativ för hello Bläddra galleriet hello Runbooks menyn:

![Runbooks toobe importeras][runbooks]

Hej runbooks som måste importeras toobe visas. Välj hello runbook baserat på om du vill lodräta skalning med eller utan reprovisioning:

![Runbooks-galleriet][gallery]

## <a name="add-a-webhook-tooyour-runbook"></a>Lägga till en webhook tooyour runbook
När du har importerat hello runbooks måste tooadd en webhook toohello runbook så att den kan aktiveras av en avisering från en Skaluppsättning. hello information om hur du skapar en webhook för din Runbook beskrivs i den här artikeln:

* [Azure Automation-webhooks](../automation/automation-webhooks.md)

> [!NOTE]
> Kontrollera att du kopierar hello webhook URI innan du stänger hello webhook dialogrutan som du behöver det i nästa avsnitt om hello.
> 
> 

## <a name="add-an-alert-tooyour-vm-scale-set"></a>Lägg till en avisering tooyour Skaluppsättning
Nedan finns ett PowerShell-skript som visar hur tooadd en avisering tooa Skaluppsättning. Referera toohello efter artikel tooget hello namnet på hello mått toofire hello avisering: [Azure-Monitor autoskalning vanliga mått](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).

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
> Är det rekommenderade tooconfigure en rimlig tidsfönster för hello avisering i ordning tooavoid utlösa lodräta skalning och alla associerade avbrott i tjänsten, för ofta. Överväg ett fönster med minst 20 – 30 minuter eller mer. Överväg att vågräta skalning om du behöver tooavoid avbrott.
> 
> 

Mer information om hur toocreate aviseringar finns toohello följande artiklar:

* [Azure-Monitor PowerShell Snabbstart-exempel](../monitoring-and-diagnostics/insights-powershell-samples.md)
* [Azure CLI för övervakaren plattformsoberoende Snabbstart-exempel](../monitoring-and-diagnostics/insights-cli-samples.md)

## <a name="summary"></a>Sammanfattning
Den här artikeln visade enkla lodräta skalning exempel. Du kan ansluta många olika händelser med en anpassad uppsättning åtgärder med byggblocken - Automation-konto, runbooks, webhooks, aviseringar.

[runbooks]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks.png
[gallery]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks-gallery.png
