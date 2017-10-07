---
title: aaaAutomatic skalning och den virtuella datorn skala anger | Microsoft Docs
description: "Lär dig mer om hur du använder diagnostik- och Autoskala resurser tooautomatically skala virtuella datorer i en skaluppsättning."
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d29a3385-179e-4331-a315-daa7ea5701df
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: adegeo
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 25f54b693e3c991577238242008c262023ed479c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-automatic-scaling-and-virtual-machine-scale-sets"></a>Hur toouse automatisk skalning och Skalningsuppsättningar i virtuella datorer
Automatisk skalning av virtuella datorer i en skaluppsättning är hello skapande eller borttagning av datorer i hello som behövs toomatch prestandakrav. När hello mängd arbete växer, ett program kan kräva ytterligare resurser tooenable den tooeffectively utföra uppgifter.

Automatisk skalning är en automatiserad process som underlättar hanteringskostnader. Genom att minska omkostnader som du inte behöver toocontinually övervaka systemets prestanda eller besluta hur toomanage resurser. Skalning är en elastisk process. Fler resurser kan läggas till som hello belastningen ökar. Och när begäran minskar resurser kan vara borttagen toominimize kostnader och underhålla prestandanivåer.

Ställa in automatisk skalning på en skala som anges med hjälp av en Azure Resource Manager-mall, Azure PowerShell, Azure CLI eller hello Azure-portalen.

## <a name="set-up-scaling-by-using-resource-manager-templates"></a>Ställ in skalning med hjälp av Resource Manager-mallar
I stället för att distribuera och hantera varje resurs i ditt program separat, använder du en mall som distribuerar alla resurser i en enda, samordnad åtgärd. I mallen för hello programresurser definieras och distribution parametrar har angetts för olika miljöer. hello mallen består av JSON och uttryck som du kan använda tooconstruct värden för din distribution. toolearn mer, titta på [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).

Ange hello kapacitet element i hello mallen:

```json
"sku": {
  "name": "Standard_A0",
  "tier": "Standard",
  "capacity": 3
},
```

Kapacitet identifierar hello antalet virtuella datorer i hello uppsättningen. Du kan ändra hello kapacitet manuellt genom att distribuera en mall med ett annat värde. Om du distribuerar en mall tooonly ändra hello kapacitet kan du inkludera endast hello SKU-element med hello uppdateras kapacitet.

hello kapacitet för din skaluppsättning kan automatiskt justeras med hjälp av en kombination av hello **autoscaleSettings** resurs och hello diagnostik-tillägget.

### <a name="configure-hello-azure-diagnostics-extension"></a>Konfigurera hello Azure Diagnostics-tillägget
Automatisk skalning kan endast göras om mått samling lyckas på varje virtuell dator i skaluppsättning för hello. hello Azure Diagnostics tillägget tillhandahåller hello övervakning och diagnostik funktioner som passar hello mått samling av hello Autoskala resurs. Du kan installera hello-tillägget som en del av hello Resource Manager-mall.

Det här exemplet visar hello variabler som används i hello mallen tooconfigure hello diagnostik tillägg:

```json
"diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'saa')]",
"accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/', 'Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
"wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
"wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Thread Count\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
"wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
"wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
"wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"
```

Parametrar anges när hello mallen distribueras. I det här exemplet hello namnet på hello storage-konto (var data lagras) och hello ange namn för hello skaluppsättning (där data samlas in). Även i det här Windows Server-exemplet samlas endast hello antal trådar prestandaräknaren in. Alla hello tillgängliga prestandaräknare i Windows eller Linux kan använda toocollect diagnostikinformation och kan tas med i hello tilläggets konfiguration.

Det här exemplet visar hello definition av hello tillägg i hello mallen:

```json
"extensionProfile": {
  "extensions": [
    {
      "name": "Microsoft.Insights.VMDiagnosticsSettings",
      "properties": {
        "publisher": "Microsoft.Azure.Diagnostics",
        "type": "IaaSDiagnostics",
        "typeHandlerVersion": "1.5",
        "autoUpgradeMinorVersion": true,
        "settings": {
          "xmlCfg": "[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
          "storageAccount": "[variables('diagnosticsStorageAccountName')]"
        },
        "protectedSettings": {
          "storageAccountName": "[variables('diagnosticsStorageAccountName')]",
          "storageAccountKey": "[listkeys(variables('accountid'), variables('apiVersion')).key1]",
          "storageAccountEndPoint": "https://core.windows.net"
        }
      }
    }
  ]
}
```

När hello diagnostik tillägget körs hello data lagras och som samlas in i en tabell i hello storage-konto som du anger. I hello **WADPerformanceCounters** tabell, som du kan hitta hello insamlade data:

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountBefore2.png)

### <a name="configure-hello-autoscalesettings-resource"></a>Konfigurera hello autoScaleSettings resurs
Hej autoscaleSettings resurs använder hello information från hello diagnostik tillägget toodecide om tooincrease eller minska hello antal virtuella datorer i hello skala.

Det här exemplet visar hello konfigurationen av hello resurs i hello mallen:

```json
{
  "type": "Microsoft.Insights/autoscaleSettings",
  "apiVersion": "2015-04-01",
  "name": "[concat(parameters('resourcePrefix'),'as1')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
  ],
  "properties": {
    "enabled": true,
    "name": "[concat(parameters('resourcePrefix'),'as1')]",
    "profiles": [
      {
        "name": "Profile1",
        "capacity": {
          "minimum": "1",
          "maximum": "10",
          "default": "1"
        },
        "rules": [
          {
            "metricTrigger": {
              "metricName": "\\Processor(_Total)\\Thread Count",
              "metricNamespace": "",
              "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
              "timeGrain": "PT1M",
              "statistic": "Average",
              "timeWindow": "PT5M",
              "timeAggregation": "Average",
              "operator": "GreaterThan",
              "threshold": 650
            },
            "scaleAction": {
              "direction": "Increase",
              "type": "ChangeCount",
              "value": "1",
              "cooldown": "PT5M"
            }
          },
          {
            "metricTrigger": {
              "metricName": "\\Processor(_Total)\\Thread Count",
              "metricNamespace": "",
              "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
              "timeGrain": "PT1M",
              "statistic": "Average",
              "timeWindow": "PT5M",
              "timeAggregation": "Average",
              "operator": "LessThan",
              "threshold": 550
            },
            "scaleAction": {
              "direction": "Decrease",
              "type": "ChangeCount",
              "value": "1",
              "cooldown": "PT5M"
            }
          }
        ]
      }
    ],
    "targetResourceUri": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]"
  }
}
```

Hello-exemplet ovan skapas två regler för att definiera hello automatisk skalning åtgärder. hello första regeln definierar hello skalbar åtgärd och hello andra regel definierar hello skala i åtgärden. Dessa värden finns i hello regler:

| Regel | Beskrivning |
| ---- | ----------- |
| metricName        | Det här värdet är hello samma som hello prestandaräknare som du definierat i hello wadperfcounter variabel för hello diagnostik tillägg. I hello-exemplet ovan används hello antal trådar räknare.    |
| metricResourceUri | Det här värdet är hello resursidentifieraren för hello skaluppsättning för virtuell dator. Den här identifieraren innehåller hello namnet på hello resursgrupp, hello namnet på hello resursprovidern och hello namnet på hello scale set tooscale. |
| Tidskorn         | Det här värdet är hello Granulariteten för hello mått som har samlats in. I föregående exempel hello, samlas data på ett intervall på en minut. Det här värdet används med värdet timeWindow. |
| statistik         | Det här värdet avgör hur hello är kombinerade tooaccommodate hello autoskalning åtgärd. hello möjliga värden är: medel, Min, Max. |
| värdet timeWindow        | Det här värdet är hello tidsintervall som samlas in instansdata. Det måste vara mellan 5 minuter och 12 timmar. |
| timeAggregation   | Det här värdet avgör hur hello data som samlas in ska kombineras med tiden. hello standardvärdet är medelvärdet. hello möjliga värden är:, lägsta, högsta, senaste, totalt antal, räknas. |
| Operatorn          | Det här värdet är hello operator som används toocompare hello mått data och hello tröskelvärdet. hello möjliga värden är: är lika med, NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual. |
| Tröskelvärde         | Det här värdet är hello-värde som utlöser hello skalningsåtgärd. Vara säker på att tooprovide tillräckligt skillnad mellan hello tröskelvärden för hello **skalbar** och **skala i** åtgärder. Om du ställer in hello samma värden för båda åtgärderna förutse hello system konstant ändras, vilket förhindrar att implementera en skalning åtgärd. Till exempel fungerar ange båda too600 trådar i föregående exempel hello inte. |
| Riktning         | Det här värdet fastställer hello vad som händer när hello tröskelvärde uppnås. hello möjliga värden är ökning eller minskning. |
| typ              | Det här värdet är hello typ av åtgärd som ska ske och tooChangeCount måste anges. |
| värde             | Det här värdet är hello antalet virtuella datorer som har lagts till tooor tas bort från hello skaluppsättning. Det här värdet måste vara 1 eller högre. |
| cooldown          | Det här värdet är hello mängden tid toowait sedan hello senaste skalning åtgärd innan hello nästa åtgärd sker. Det här värdet måste vara mellan en minut och en vecka. |

Om du använder kan vissa hello element i mallkonfigurationen hello används på olika sätt beroende på hello prestandaräknaren. I föregående exempel hello, hello prestandaräknaren är antal trådar, hello tröskelvärdet är 650 för en skalbar åtgärd och hello tröskelvärdet är 550 för hello skala i åtgärden. Om du använder en räknare, till exempel % processortid anges hello tröskelvärdet toohello procentandelen av CPU-kapaciteten som anger en skalning åtgärd.

När en skalning åtgärd utlöses, till exempel en hög belastning ökas hello kapacitet för hello mängd baserat på hello värdet i hello mallen. Till exempel anger där hello kapacitet är too3 och hello skalningsåtgärdsvärde har angetts too1 i en skala:

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerBefore.png)

När hello aktuella belastningen orsaker hello genomsnittlig tråd antal toogo över hello tröskeln på 650:

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountAfter.png)

En **skalbar** åtgärden utlöses att orsaker hello kapacitet hello set toobe ökat med ett:

```json
"sku": {
  "name": "Standard_A0",
  "tier": "Standard",
  "capacity": 4
},
```

hello resultatet är en virtuell dator läggs toohello skaluppsättning:

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerAfter.png)

När en cooldown period på fem minuter om hello Genomsnittligt antal trådar på hello datorer ligger över 600, läggs en annan dator toohello uppsättningen. Om hello genomsnittlig trådar förblir nedan 550, minskas hello kapacitet för hello skaluppsättning av en och en dator tas bort från hello mängd.

## <a name="set-up-scaling-using-azure-powershell"></a>Ställ in skalning med Azure PowerShell

toosee exempel på användning av PowerShell tooset in autoskalningen, titta på [Azure-Monitor PowerShell snabb start exempel](../monitoring-and-diagnostics/insights-powershell-samples.md).

## <a name="set-up-scaling-using-azure-cli"></a>Ställ in skalning med Azure CLI

toosee exempel på användning av Azure CLI tooset in autoskalningen, titta på [Azure-Monitor plattformsoberoende CLI snabb start exempel](../monitoring-and-diagnostics/insights-cli-samples.md).

## <a name="set-up-scaling-using-hello-azure-portal"></a>Ställ in skalning med hello Azure-portalen

toosee ett exempel på hur hello Azure portal tooset in autoskalningen, titta på [skapa en Skalningsuppsättning virtuell dator med hjälp av hello Azure-portalen](virtual-machine-scale-sets-portal-create.md).

## <a name="investigate-scaling-actions"></a>Undersök skalning åtgärder

* **Azure Portal**  
För närvarande kan du få en begränsad mängd information med hjälp av hello-portalen.

* **Azure Resource Explorer**  
Det här verktyget är hello bästa för att utforska hello aktuell status för din skaluppsättning. Följ den här sökvägen och du bör se hello instansvyn för hello skala ange som du skapade:  
**Prenumerationer > {din prenumeration} > resursgrupper > {din resursgrupp} > providers > Microsoft.Compute > virtualMachineScaleSets > {skaluppsättning} > virtuella datorer**

* **Azure PowerShell**  
Använd det här kommandot tooget vissa information:

  ```powershell
  Get-AzureRmResource -name vmsstest1 -ResourceGroupName vmsstestrg1 -ResourceType Microsoft.Compute/virtualMachineScaleSets -ApiVersion 2015-06-15
  Get-Autoscalesetting -ResourceGroup rainvmss -DetailedOutput
  ```

* Ansluta toohello jumpbox virtuella datorn precis som du skulle någon annan dator och sedan kan du fjärransluta till hello virtuella datorer i hello scale set toomonitor enskilda processer.

## <a name="next-steps"></a>Nästa steg
* Titta på [skala automatiskt datorer i en virtuell dator Skaluppsättning](virtual-machine-scale-sets-windows-autoscale.md) toosee ett exempel på hur toocreate en skaluppsättning med automatisk skalning konfigurerats.

* Hitta exempel på Azure-Monitor övervakningsfunktionerna i [Azure-Monitor PowerShell snabb start prover](../monitoring-and-diagnostics/insights-powershell-samples.md)

* Lär dig mer om meddelandefunktioner i [använda automatiska åtgärder toosend e-post och webhook aviseringar i Azure-Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md).

* Mer information om hur för[Använd granskningsloggar toosend e-post och webhook aviseringar i Azure-Monitor](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)

* Lär dig mer om [avancerade scenarier för Autoskala](virtual-machine-scale-sets-advanced-autoscale.md).
