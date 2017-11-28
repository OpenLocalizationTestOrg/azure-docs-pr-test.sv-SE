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
# <a name="how-toouse-automatic-scaling-and-virtual-machine-scale-sets"></a><span data-ttu-id="18b6d-103">Hur toouse automatisk skalning och Skalningsuppsättningar i virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="18b6d-103">How toouse automatic scaling and Virtual Machine Scale Sets</span></span>
<span data-ttu-id="18b6d-104">Automatisk skalning av virtuella datorer i en skaluppsättning är hello skapande eller borttagning av datorer i hello som behövs toomatch prestandakrav.</span><span class="sxs-lookup"><span data-stu-id="18b6d-104">Automatic scaling of virtual machines in a scale set is hello creation or deletion of machines in hello set as needed toomatch performance requirements.</span></span> <span data-ttu-id="18b6d-105">När hello mängd arbete växer, ett program kan kräva ytterligare resurser tooenable den tooeffectively utföra uppgifter.</span><span class="sxs-lookup"><span data-stu-id="18b6d-105">As hello volume of work grows, an application may require additional resources tooenable it tooeffectively perform tasks.</span></span>

<span data-ttu-id="18b6d-106">Automatisk skalning är en automatiserad process som underlättar hanteringskostnader.</span><span class="sxs-lookup"><span data-stu-id="18b6d-106">Automatic scaling is an automated process that helps ease management overhead.</span></span> <span data-ttu-id="18b6d-107">Genom att minska omkostnader som du inte behöver toocontinually övervaka systemets prestanda eller besluta hur toomanage resurser.</span><span class="sxs-lookup"><span data-stu-id="18b6d-107">By reducing overhead, you don't need toocontinually monitor system performance or decide how toomanage resources.</span></span> <span data-ttu-id="18b6d-108">Skalning är en elastisk process.</span><span class="sxs-lookup"><span data-stu-id="18b6d-108">Scaling is an elastic process.</span></span> <span data-ttu-id="18b6d-109">Fler resurser kan läggas till som hello belastningen ökar.</span><span class="sxs-lookup"><span data-stu-id="18b6d-109">More resources can be added as hello load increases.</span></span> <span data-ttu-id="18b6d-110">Och när begäran minskar resurser kan vara borttagen toominimize kostnader och underhålla prestandanivåer.</span><span class="sxs-lookup"><span data-stu-id="18b6d-110">And as demand decreases, resources can be removed toominimize costs and maintain performance levels.</span></span>

<span data-ttu-id="18b6d-111">Ställa in automatisk skalning på en skala som anges med hjälp av en Azure Resource Manager-mall, Azure PowerShell, Azure CLI eller hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="18b6d-111">Set up automatic scaling on a scale set by using an Azure Resource Manager template, Azure PowerShell, Azure CLI, or hello Azure portal.</span></span>

## <a name="set-up-scaling-by-using-resource-manager-templates"></a><span data-ttu-id="18b6d-112">Ställ in skalning med hjälp av Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="18b6d-112">Set up scaling by using Resource Manager templates</span></span>
<span data-ttu-id="18b6d-113">I stället för att distribuera och hantera varje resurs i ditt program separat, använder du en mall som distribuerar alla resurser i en enda, samordnad åtgärd.</span><span class="sxs-lookup"><span data-stu-id="18b6d-113">Rather than deploying and managing each resource of your application separately, use a template that deploys all resources in a single, coordinated operation.</span></span> <span data-ttu-id="18b6d-114">I mallen för hello programresurser definieras och distribution parametrar har angetts för olika miljöer.</span><span class="sxs-lookup"><span data-stu-id="18b6d-114">In hello template, application resources are defined and deployment parameters are specified for different environments.</span></span> <span data-ttu-id="18b6d-115">hello mallen består av JSON och uttryck som du kan använda tooconstruct värden för din distribution.</span><span class="sxs-lookup"><span data-stu-id="18b6d-115">hello template consists of JSON and expressions that you can use tooconstruct values for your deployment.</span></span> <span data-ttu-id="18b6d-116">toolearn mer, titta på [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="18b6d-116">toolearn more, look at [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="18b6d-117">Ange hello kapacitet element i hello mallen:</span><span class="sxs-lookup"><span data-stu-id="18b6d-117">In hello template, you specify hello capacity element:</span></span>

```json
"sku": {
  "name": "Standard_A0",
  "tier": "Standard",
  "capacity": 3
},
```

<span data-ttu-id="18b6d-118">Kapacitet identifierar hello antalet virtuella datorer i hello uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="18b6d-118">Capacity identifies hello number of virtual machines in hello set.</span></span> <span data-ttu-id="18b6d-119">Du kan ändra hello kapacitet manuellt genom att distribuera en mall med ett annat värde.</span><span class="sxs-lookup"><span data-stu-id="18b6d-119">You can manually change hello capacity by deploying a template with a different value.</span></span> <span data-ttu-id="18b6d-120">Om du distribuerar en mall tooonly ändra hello kapacitet kan du inkludera endast hello SKU-element med hello uppdateras kapacitet.</span><span class="sxs-lookup"><span data-stu-id="18b6d-120">If you are deploying a template tooonly change hello capacity, you can include only hello SKU element with hello updated capacity.</span></span>

<span data-ttu-id="18b6d-121">hello kapacitet för din skaluppsättning kan automatiskt justeras med hjälp av en kombination av hello **autoscaleSettings** resurs och hello diagnostik-tillägget.</span><span class="sxs-lookup"><span data-stu-id="18b6d-121">hello capacity of your scale set can be automatically adjusted by using a combination of hello **autoscaleSettings** resource and hello diagnostics extension.</span></span>

### <a name="configure-hello-azure-diagnostics-extension"></a><span data-ttu-id="18b6d-122">Konfigurera hello Azure Diagnostics-tillägget</span><span class="sxs-lookup"><span data-stu-id="18b6d-122">Configure hello Azure Diagnostics extension</span></span>
<span data-ttu-id="18b6d-123">Automatisk skalning kan endast göras om mått samling lyckas på varje virtuell dator i skaluppsättning för hello.</span><span class="sxs-lookup"><span data-stu-id="18b6d-123">Automatic scaling can only be done if metrics collection is successful on each virtual machine in hello scale set.</span></span> <span data-ttu-id="18b6d-124">hello Azure Diagnostics tillägget tillhandahåller hello övervakning och diagnostik funktioner som passar hello mått samling av hello Autoskala resurs.</span><span class="sxs-lookup"><span data-stu-id="18b6d-124">hello Azure Diagnostics Extension provides hello monitoring and diagnostics capabilities that meets hello metrics collection needs of hello autoscale resource.</span></span> <span data-ttu-id="18b6d-125">Du kan installera hello-tillägget som en del av hello Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="18b6d-125">You can install hello extension as part of hello Resource Manager template.</span></span>

<span data-ttu-id="18b6d-126">Det här exemplet visar hello variabler som används i hello mallen tooconfigure hello diagnostik tillägg:</span><span class="sxs-lookup"><span data-stu-id="18b6d-126">This example shows hello variables that are used in hello template tooconfigure hello diagnostics extension:</span></span>

```json
"diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'saa')]",
"accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/', 'Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
"wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
"wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Thread Count\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
"wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
"wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
"wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"
```

<span data-ttu-id="18b6d-127">Parametrar anges när hello mallen distribueras.</span><span class="sxs-lookup"><span data-stu-id="18b6d-127">Parameters are provided when hello template is deployed.</span></span> <span data-ttu-id="18b6d-128">I det här exemplet hello namnet på hello storage-konto (var data lagras) och hello ange namn för hello skaluppsättning (där data samlas in).</span><span class="sxs-lookup"><span data-stu-id="18b6d-128">In this example, hello name of hello storage account (where data is stored) and hello name of hello scale set (where data is collected) are provided.</span></span> <span data-ttu-id="18b6d-129">Även i det här Windows Server-exemplet samlas endast hello antal trådar prestandaräknaren in.</span><span class="sxs-lookup"><span data-stu-id="18b6d-129">Also in this Windows Server example, only hello Thread Count performance counter is collected.</span></span> <span data-ttu-id="18b6d-130">Alla hello tillgängliga prestandaräknare i Windows eller Linux kan använda toocollect diagnostikinformation och kan tas med i hello tilläggets konfiguration.</span><span class="sxs-lookup"><span data-stu-id="18b6d-130">All hello available performance counters in Windows or Linux can be used toocollect diagnostics information and can be included in hello extension configuration.</span></span>

<span data-ttu-id="18b6d-131">Det här exemplet visar hello definition av hello tillägg i hello mallen:</span><span class="sxs-lookup"><span data-stu-id="18b6d-131">This example shows hello definition of hello extension in hello template:</span></span>

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

<span data-ttu-id="18b6d-132">När hello diagnostik tillägget körs hello data lagras och som samlas in i en tabell i hello storage-konto som du anger.</span><span class="sxs-lookup"><span data-stu-id="18b6d-132">When hello diagnostics extension runs, hello data is stored and collected in a table, in hello storage account that you specify.</span></span> <span data-ttu-id="18b6d-133">I hello **WADPerformanceCounters** tabell, som du kan hitta hello insamlade data:</span><span class="sxs-lookup"><span data-stu-id="18b6d-133">In hello **WADPerformanceCounters** table, you can find hello collected data:</span></span>

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountBefore2.png)

### <a name="configure-hello-autoscalesettings-resource"></a><span data-ttu-id="18b6d-134">Konfigurera hello autoScaleSettings resurs</span><span class="sxs-lookup"><span data-stu-id="18b6d-134">Configure hello autoScaleSettings resource</span></span>
<span data-ttu-id="18b6d-135">Hej autoscaleSettings resurs använder hello information från hello diagnostik tillägget toodecide om tooincrease eller minska hello antal virtuella datorer i hello skala.</span><span class="sxs-lookup"><span data-stu-id="18b6d-135">hello autoscaleSettings resource uses hello information from hello diagnostics extension toodecide whether tooincrease or decrease hello number of virtual machines in hello scale set.</span></span>

<span data-ttu-id="18b6d-136">Det här exemplet visar hello konfigurationen av hello resurs i hello mallen:</span><span class="sxs-lookup"><span data-stu-id="18b6d-136">This example shows hello configuration of hello resource in hello template:</span></span>

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

<span data-ttu-id="18b6d-137">Hello-exemplet ovan skapas två regler för att definiera hello automatisk skalning åtgärder.</span><span class="sxs-lookup"><span data-stu-id="18b6d-137">In hello example above, two rules are created for defining hello automatic scaling actions.</span></span> <span data-ttu-id="18b6d-138">hello första regeln definierar hello skalbar åtgärd och hello andra regel definierar hello skala i åtgärden.</span><span class="sxs-lookup"><span data-stu-id="18b6d-138">hello first rule defines hello scale-out action and hello second rule defines hello scale-in action.</span></span> <span data-ttu-id="18b6d-139">Dessa värden finns i hello regler:</span><span class="sxs-lookup"><span data-stu-id="18b6d-139">These values are provided in hello rules:</span></span>

| <span data-ttu-id="18b6d-140">Regel</span><span class="sxs-lookup"><span data-stu-id="18b6d-140">Rule</span></span> | <span data-ttu-id="18b6d-141">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="18b6d-141">Description</span></span> |
| ---- | ----------- |
| <span data-ttu-id="18b6d-142">metricName</span><span class="sxs-lookup"><span data-stu-id="18b6d-142">metricName</span></span>        | <span data-ttu-id="18b6d-143">Det här värdet är hello samma som hello prestandaräknare som du definierat i hello wadperfcounter variabel för hello diagnostik tillägg.</span><span class="sxs-lookup"><span data-stu-id="18b6d-143">This value is hello same as hello performance counter that you defined in hello wadperfcounter variable for hello diagnostics extension.</span></span> <span data-ttu-id="18b6d-144">I hello-exemplet ovan används hello antal trådar räknare.</span><span class="sxs-lookup"><span data-stu-id="18b6d-144">In hello example above, hello Thread Count counter is used.</span></span>    |
| <span data-ttu-id="18b6d-145">metricResourceUri</span><span class="sxs-lookup"><span data-stu-id="18b6d-145">metricResourceUri</span></span> | <span data-ttu-id="18b6d-146">Det här värdet är hello resursidentifieraren för hello skaluppsättning för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="18b6d-146">This value is hello resource identifier of hello virtual machine scale set.</span></span> <span data-ttu-id="18b6d-147">Den här identifieraren innehåller hello namnet på hello resursgrupp, hello namnet på hello resursprovidern och hello namnet på hello scale set tooscale.</span><span class="sxs-lookup"><span data-stu-id="18b6d-147">This identifier contains hello name of hello resource group, hello name of hello resource provider, and hello name of hello scale set tooscale.</span></span> |
| <span data-ttu-id="18b6d-148">Tidskorn</span><span class="sxs-lookup"><span data-stu-id="18b6d-148">timeGrain</span></span>         | <span data-ttu-id="18b6d-149">Det här värdet är hello Granulariteten för hello mått som har samlats in.</span><span class="sxs-lookup"><span data-stu-id="18b6d-149">This value is hello granularity of hello metrics that are collected.</span></span> <span data-ttu-id="18b6d-150">I föregående exempel hello, samlas data på ett intervall på en minut.</span><span class="sxs-lookup"><span data-stu-id="18b6d-150">In hello preceding example, data is collected on an interval of one minute.</span></span> <span data-ttu-id="18b6d-151">Det här värdet används med värdet timeWindow.</span><span class="sxs-lookup"><span data-stu-id="18b6d-151">This value is used with timeWindow.</span></span> |
| <span data-ttu-id="18b6d-152">statistik</span><span class="sxs-lookup"><span data-stu-id="18b6d-152">statistic</span></span>         | <span data-ttu-id="18b6d-153">Det här värdet avgör hur hello är kombinerade tooaccommodate hello autoskalning åtgärd.</span><span class="sxs-lookup"><span data-stu-id="18b6d-153">This value determines how hello metrics are combined tooaccommodate hello automatic scaling action.</span></span> <span data-ttu-id="18b6d-154">hello möjliga värden är: medel, Min, Max.</span><span class="sxs-lookup"><span data-stu-id="18b6d-154">hello possible values are: Average, Min, Max.</span></span> |
| <span data-ttu-id="18b6d-155">värdet timeWindow</span><span class="sxs-lookup"><span data-stu-id="18b6d-155">timeWindow</span></span>        | <span data-ttu-id="18b6d-156">Det här värdet är hello tidsintervall som samlas in instansdata.</span><span class="sxs-lookup"><span data-stu-id="18b6d-156">This value is hello range of time in which instance data is collected.</span></span> <span data-ttu-id="18b6d-157">Det måste vara mellan 5 minuter och 12 timmar.</span><span class="sxs-lookup"><span data-stu-id="18b6d-157">It must be between 5 minutes and 12 hours.</span></span> |
| <span data-ttu-id="18b6d-158">timeAggregation</span><span class="sxs-lookup"><span data-stu-id="18b6d-158">timeAggregation</span></span>   | <span data-ttu-id="18b6d-159">Det här värdet avgör hur hello data som samlas in ska kombineras med tiden.</span><span class="sxs-lookup"><span data-stu-id="18b6d-159">This value determines how hello data that is collected should be combined over time.</span></span> <span data-ttu-id="18b6d-160">hello standardvärdet är medelvärdet.</span><span class="sxs-lookup"><span data-stu-id="18b6d-160">hello default value is Average.</span></span> <span data-ttu-id="18b6d-161">hello möjliga värden är:, lägsta, högsta, senaste, totalt antal, räknas.</span><span class="sxs-lookup"><span data-stu-id="18b6d-161">hello possible values are: Average, Minimum, Maximum, Last, Total, Count.</span></span> |
| <span data-ttu-id="18b6d-162">Operatorn</span><span class="sxs-lookup"><span data-stu-id="18b6d-162">operator</span></span>          | <span data-ttu-id="18b6d-163">Det här värdet är hello operator som används toocompare hello mått data och hello tröskelvärdet.</span><span class="sxs-lookup"><span data-stu-id="18b6d-163">This value is hello operator that is used toocompare hello metric data and hello threshold.</span></span> <span data-ttu-id="18b6d-164">hello möjliga värden är: är lika med, NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.</span><span class="sxs-lookup"><span data-stu-id="18b6d-164">hello possible values are: Equals, NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.</span></span> |
| <span data-ttu-id="18b6d-165">Tröskelvärde</span><span class="sxs-lookup"><span data-stu-id="18b6d-165">threshold</span></span>         | <span data-ttu-id="18b6d-166">Det här värdet är hello-värde som utlöser hello skalningsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="18b6d-166">This value is hello value that triggers hello scale action.</span></span> <span data-ttu-id="18b6d-167">Vara säker på att tooprovide tillräckligt skillnad mellan hello tröskelvärden för hello **skalbar** och **skala i** åtgärder.</span><span class="sxs-lookup"><span data-stu-id="18b6d-167">Be sure tooprovide a sufficient difference between hello threshold values for hello **scale-out** and **scale-in** actions.</span></span> <span data-ttu-id="18b6d-168">Om du ställer in hello samma värden för båda åtgärderna förutse hello system konstant ändras, vilket förhindrar att implementera en skalning åtgärd.</span><span class="sxs-lookup"><span data-stu-id="18b6d-168">If you set hello same values for both actions, hello system anticipates constant change, which prevents it from implementing a scaling action.</span></span> <span data-ttu-id="18b6d-169">Till exempel fungerar ange båda too600 trådar i föregående exempel hello inte.</span><span class="sxs-lookup"><span data-stu-id="18b6d-169">For example, setting both too600 threads in hello preceding example doesn't work.</span></span> |
| <span data-ttu-id="18b6d-170">Riktning</span><span class="sxs-lookup"><span data-stu-id="18b6d-170">direction</span></span>         | <span data-ttu-id="18b6d-171">Det här värdet fastställer hello vad som händer när hello tröskelvärde uppnås.</span><span class="sxs-lookup"><span data-stu-id="18b6d-171">This value determines hello action that is taken when hello threshold value is achieved.</span></span> <span data-ttu-id="18b6d-172">hello möjliga värden är ökning eller minskning.</span><span class="sxs-lookup"><span data-stu-id="18b6d-172">hello possible values are Increase or Decrease.</span></span> |
| <span data-ttu-id="18b6d-173">typ</span><span class="sxs-lookup"><span data-stu-id="18b6d-173">type</span></span>              | <span data-ttu-id="18b6d-174">Det här värdet är hello typ av åtgärd som ska ske och tooChangeCount måste anges.</span><span class="sxs-lookup"><span data-stu-id="18b6d-174">This value is hello type of action that should occur and must be set tooChangeCount.</span></span> |
| <span data-ttu-id="18b6d-175">värde</span><span class="sxs-lookup"><span data-stu-id="18b6d-175">value</span></span>             | <span data-ttu-id="18b6d-176">Det här värdet är hello antalet virtuella datorer som har lagts till tooor tas bort från hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="18b6d-176">This value is hello number of virtual machines that are added tooor removed from hello scale set.</span></span> <span data-ttu-id="18b6d-177">Det här värdet måste vara 1 eller högre.</span><span class="sxs-lookup"><span data-stu-id="18b6d-177">This value must be 1 or greater.</span></span> |
| <span data-ttu-id="18b6d-178">cooldown</span><span class="sxs-lookup"><span data-stu-id="18b6d-178">cooldown</span></span>          | <span data-ttu-id="18b6d-179">Det här värdet är hello mängden tid toowait sedan hello senaste skalning åtgärd innan hello nästa åtgärd sker.</span><span class="sxs-lookup"><span data-stu-id="18b6d-179">This value is hello amount of time toowait since hello last scaling action before hello next action occurs.</span></span> <span data-ttu-id="18b6d-180">Det här värdet måste vara mellan en minut och en vecka.</span><span class="sxs-lookup"><span data-stu-id="18b6d-180">This value must be between one minute and one week.</span></span> |

<span data-ttu-id="18b6d-181">Om du använder kan vissa hello element i mallkonfigurationen hello används på olika sätt beroende på hello prestandaräknaren.</span><span class="sxs-lookup"><span data-stu-id="18b6d-181">Depending on hello performance counter, you are using, some of hello elements in hello template configuration are used differently.</span></span> <span data-ttu-id="18b6d-182">I föregående exempel hello, hello prestandaräknaren är antal trådar, hello tröskelvärdet är 650 för en skalbar åtgärd och hello tröskelvärdet är 550 för hello skala i åtgärden.</span><span class="sxs-lookup"><span data-stu-id="18b6d-182">In hello preceding example, hello performance counter is Thread Count, hello threshold value is 650 for a scale-out action, and hello threshold value is 550 for hello scale-in action.</span></span> <span data-ttu-id="18b6d-183">Om du använder en räknare, till exempel % processortid anges hello tröskelvärdet toohello procentandelen av CPU-kapaciteten som anger en skalning åtgärd.</span><span class="sxs-lookup"><span data-stu-id="18b6d-183">If you use a counter such as %Processor Time, hello threshold value is set toohello percentage of CPU usage that determines a scaling action.</span></span>

<span data-ttu-id="18b6d-184">När en skalning åtgärd utlöses, till exempel en hög belastning ökas hello kapacitet för hello mängd baserat på hello värdet i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="18b6d-184">When a scaling action is triggered, such as a high load, hello capacity of hello set is increased based on hello value in hello template.</span></span> <span data-ttu-id="18b6d-185">Till exempel anger där hello kapacitet är too3 och hello skalningsåtgärdsvärde har angetts too1 i en skala:</span><span class="sxs-lookup"><span data-stu-id="18b6d-185">For example, in a scale set where hello capacity is set too3 and hello scale action value is set too1:</span></span>

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerBefore.png)

<span data-ttu-id="18b6d-186">När hello aktuella belastningen orsaker hello genomsnittlig tråd antal toogo över hello tröskeln på 650:</span><span class="sxs-lookup"><span data-stu-id="18b6d-186">When hello current load causes hello average thread count toogo above hello threshold of 650:</span></span>

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountAfter.png)

<span data-ttu-id="18b6d-187">En **skalbar** åtgärden utlöses att orsaker hello kapacitet hello set toobe ökat med ett:</span><span class="sxs-lookup"><span data-stu-id="18b6d-187">A **scale-out** action is triggered that causes hello capacity of hello set toobe increased by one:</span></span>

```json
"sku": {
  "name": "Standard_A0",
  "tier": "Standard",
  "capacity": 4
},
```

<span data-ttu-id="18b6d-188">hello resultatet är en virtuell dator läggs toohello skaluppsättning:</span><span class="sxs-lookup"><span data-stu-id="18b6d-188">hello result is a virtual machine is added toohello scale set:</span></span>

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerAfter.png)

<span data-ttu-id="18b6d-189">När en cooldown period på fem minuter om hello Genomsnittligt antal trådar på hello datorer ligger över 600, läggs en annan dator toohello uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="18b6d-189">After a cooldown period of five minutes, if hello average number of threads on hello machines stays over 600, another machine is added toohello set.</span></span> <span data-ttu-id="18b6d-190">Om hello genomsnittlig trådar förblir nedan 550, minskas hello kapacitet för hello skaluppsättning av en och en dator tas bort från hello mängd.</span><span class="sxs-lookup"><span data-stu-id="18b6d-190">If hello average thread count stays below 550, hello capacity of hello scale set is reduced by one and a machine is removed from hello set.</span></span>

## <a name="set-up-scaling-using-azure-powershell"></a><span data-ttu-id="18b6d-191">Ställ in skalning med Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="18b6d-191">Set up scaling using Azure PowerShell</span></span>

<span data-ttu-id="18b6d-192">toosee exempel på användning av PowerShell tooset in autoskalningen, titta på [Azure-Monitor PowerShell snabb start exempel](../monitoring-and-diagnostics/insights-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="18b6d-192">toosee examples of using PowerShell tooset up autoscaling, look at [Azure Monitor PowerShell quick start samples](../monitoring-and-diagnostics/insights-powershell-samples.md).</span></span>

## <a name="set-up-scaling-using-azure-cli"></a><span data-ttu-id="18b6d-193">Ställ in skalning med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="18b6d-193">Set up scaling using Azure CLI</span></span>

<span data-ttu-id="18b6d-194">toosee exempel på användning av Azure CLI tooset in autoskalningen, titta på [Azure-Monitor plattformsoberoende CLI snabb start exempel](../monitoring-and-diagnostics/insights-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="18b6d-194">toosee examples of using Azure CLI tooset up autoscaling, look at [Azure Monitor Cross-platform CLI quick start samples](../monitoring-and-diagnostics/insights-cli-samples.md).</span></span>

## <a name="set-up-scaling-using-hello-azure-portal"></a><span data-ttu-id="18b6d-195">Ställ in skalning med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="18b6d-195">Set up scaling using hello Azure portal</span></span>

<span data-ttu-id="18b6d-196">toosee ett exempel på hur hello Azure portal tooset in autoskalningen, titta på [skapa en Skalningsuppsättning virtuell dator med hjälp av hello Azure-portalen](virtual-machine-scale-sets-portal-create.md).</span><span class="sxs-lookup"><span data-stu-id="18b6d-196">toosee an example of using hello Azure portal tooset up autoscaling, look at [Create a Virtual Machine Scale Set using hello Azure portal](virtual-machine-scale-sets-portal-create.md).</span></span>

## <a name="investigate-scaling-actions"></a><span data-ttu-id="18b6d-197">Undersök skalning åtgärder</span><span class="sxs-lookup"><span data-stu-id="18b6d-197">Investigate scaling actions</span></span>

* <span data-ttu-id="18b6d-198">**Azure Portal**</span><span class="sxs-lookup"><span data-stu-id="18b6d-198">**Azure portal**</span></span>  
<span data-ttu-id="18b6d-199">För närvarande kan du få en begränsad mängd information med hjälp av hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="18b6d-199">You can currently get a limited amount of information using hello portal.</span></span>

* <span data-ttu-id="18b6d-200">**Azure Resource Explorer**</span><span class="sxs-lookup"><span data-stu-id="18b6d-200">**Azure Resource Explorer**</span></span>  
<span data-ttu-id="18b6d-201">Det här verktyget är hello bästa för att utforska hello aktuell status för din skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="18b6d-201">This tool is hello best for exploring hello current state of your scale set.</span></span> <span data-ttu-id="18b6d-202">Följ den här sökvägen och du bör se hello instansvyn för hello skala ange som du skapade:</span><span class="sxs-lookup"><span data-stu-id="18b6d-202">Follow this path and you should see hello instance view of hello scale set that you created:</span></span>  
<span data-ttu-id="18b6d-203">**Prenumerationer > {din prenumeration} > resursgrupper > {din resursgrupp} > providers > Microsoft.Compute > virtualMachineScaleSets > {skaluppsättning} > virtuella datorer**</span><span class="sxs-lookup"><span data-stu-id="18b6d-203">**Subscriptions > {your subscription} > resourceGroups > {your resource group} > providers > Microsoft.Compute > virtualMachineScaleSets > {your scale set} > virtualMachines**</span></span>

* <span data-ttu-id="18b6d-204">**Azure PowerShell**</span><span class="sxs-lookup"><span data-stu-id="18b6d-204">**Azure PowerShell**</span></span>  
<span data-ttu-id="18b6d-205">Använd det här kommandot tooget vissa information:</span><span class="sxs-lookup"><span data-stu-id="18b6d-205">Use this command tooget some information:</span></span>

  ```powershell
  Get-AzureRmResource -name vmsstest1 -ResourceGroupName vmsstestrg1 -ResourceType Microsoft.Compute/virtualMachineScaleSets -ApiVersion 2015-06-15
  Get-Autoscalesetting -ResourceGroup rainvmss -DetailedOutput
  ```

* <span data-ttu-id="18b6d-206">Ansluta toohello jumpbox virtuella datorn precis som du skulle någon annan dator och sedan kan du fjärransluta till hello virtuella datorer i hello scale set toomonitor enskilda processer.</span><span class="sxs-lookup"><span data-stu-id="18b6d-206">Connect toohello jumpbox virtual machine just like you would any other machine and then you can remotely access hello virtual machines in hello scale set toomonitor individual processes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="18b6d-207">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="18b6d-207">Next Steps</span></span>
* <span data-ttu-id="18b6d-208">Titta på [skala automatiskt datorer i en virtuell dator Skaluppsättning](virtual-machine-scale-sets-windows-autoscale.md) toosee ett exempel på hur toocreate en skaluppsättning med automatisk skalning konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="18b6d-208">Look at [Automatically scale machines in a Virtual Machine Scale Set](virtual-machine-scale-sets-windows-autoscale.md) toosee an example of how toocreate a scale set with automatic scaling configured.</span></span>

* <span data-ttu-id="18b6d-209">Hitta exempel på Azure-Monitor övervakningsfunktionerna i [Azure-Monitor PowerShell snabb start prover](../monitoring-and-diagnostics/insights-powershell-samples.md)</span><span class="sxs-lookup"><span data-stu-id="18b6d-209">Find examples of Azure Monitor monitoring features in [Azure Monitor PowerShell quick start samples](../monitoring-and-diagnostics/insights-powershell-samples.md)</span></span>

* <span data-ttu-id="18b6d-210">Lär dig mer om meddelandefunktioner i [använda automatiska åtgärder toosend e-post och webhook aviseringar i Azure-Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md).</span><span class="sxs-lookup"><span data-stu-id="18b6d-210">Learn about notification features in [Use autoscale actions toosend email and webhook alert notifications in Azure Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md).</span></span>

* <span data-ttu-id="18b6d-211">Mer information om hur för[Använd granskningsloggar toosend e-post och webhook aviseringar i Azure-Monitor](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)</span><span class="sxs-lookup"><span data-stu-id="18b6d-211">Learn about how too[Use audit logs toosend email and webhook alert notifications in Azure Monitor](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)</span></span>

* <span data-ttu-id="18b6d-212">Lär dig mer om [avancerade scenarier för Autoskala](virtual-machine-scale-sets-advanced-autoscale.md).</span><span class="sxs-lookup"><span data-stu-id="18b6d-212">Learn about [advanced autoscale scenarios](virtual-machine-scale-sets-advanced-autoscale.md).</span></span>
