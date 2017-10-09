---
title: aaaLearn om den virtuella datorn ange mallar | Microsoft Docs
description: "Läs toocreate en minsta lönsam skala ange mall för virtuella datorer"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: negat
ms.openlocfilehash: b7a1cf6c03b22585e16db9c071d45795c8ae75df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="learn-about-virtual-machine-scale-set-templates"></a><span data-ttu-id="8695c-103">Lär dig mer om scale set-mallar för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="8695c-103">Learn about virtual machine scale set templates</span></span>
<span data-ttu-id="8695c-104">[Azure Resource Manager-mallar](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) är ett bra sätt toodeploy grupper av relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="8695c-104">[Azure Resource Manager templates](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) are a great way toodeploy groups of related resources.</span></span> <span data-ttu-id="8695c-105">Den här självstudiekursen serien visar hur toocreate en minsta lönsam skala ange mall och toomodify den här mallen toosuit olika scenarier.</span><span class="sxs-lookup"><span data-stu-id="8695c-105">This tutorial series shows how toocreate a minimum viable scale set template and how toomodify this template toosuit various scenarios.</span></span> <span data-ttu-id="8695c-106">Alla exempel kommer från den här [GitHub-lagringsplatsen](https://github.com/gatneil/mvss).</span><span class="sxs-lookup"><span data-stu-id="8695c-106">All examples come from this [GitHub repository](https://github.com/gatneil/mvss).</span></span> 

<span data-ttu-id="8695c-107">Den här mallen är avsedda toobe enkel.</span><span class="sxs-lookup"><span data-stu-id="8695c-107">This template is intended toobe simple.</span></span> <span data-ttu-id="8695c-108">Mer komplett exempel på skalan mallar finns i avsnittet hello [Azure Quickstart mallar GitHub-lagringsplatsen](https://github.com/Azure/azure-quickstart-templates) och Sök efter mappar som innehåller hello sträng `vmss`.</span><span class="sxs-lookup"><span data-stu-id="8695c-108">For more complete examples of scale set templates, see hello [Azure Quickstart Templates GitHub repository](https://github.com/Azure/azure-quickstart-templates) and search for folders that contain hello string `vmss`.</span></span>

<span data-ttu-id="8695c-109">Om du redan är bekant med att skapa mallar du kan hoppa över toohello ”nästa steg” avsnittet toosee hur toomodify den här mallen.</span><span class="sxs-lookup"><span data-stu-id="8695c-109">If you are already familiar with creating templates, you can skip toohello "Next steps" section toosee how toomodify this template.</span></span>

## <a name="review-hello-template"></a><span data-ttu-id="8695c-110">Granska hello mall</span><span class="sxs-lookup"><span data-stu-id="8695c-110">Review hello template</span></span>

<span data-ttu-id="8695c-111">Använda GitHub tooreview våra minsta lönsam skaluppsättning mallen [azuredeploy.json](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="8695c-111">Use GitHub tooreview our minimum viable scale set template, [azuredeploy.json](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json).</span></span>

<span data-ttu-id="8695c-112">I den här självstudiekursen kommer vi undersöka hello diff (`git diff master minimum-viable-scale-set`) toocreate hello lägsta lönsam skala ange mall bit för bit.</span><span class="sxs-lookup"><span data-stu-id="8695c-112">In this tutorial, we examine hello diff (`git diff master minimum-viable-scale-set`) toocreate hello minimum viable scale set template piece by piece.</span></span>

## <a name="define-schema-and-contentversion"></a><span data-ttu-id="8695c-113">Definiera $schema och contentVersion</span><span class="sxs-lookup"><span data-stu-id="8695c-113">Define $schema and contentVersion</span></span>
<span data-ttu-id="8695c-114">Först måste vi definiera `$schema` och `contentVersion` i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="8695c-114">First, we define `$schema` and `contentVersion` in hello template.</span></span> <span data-ttu-id="8695c-115">Hej `$schema` elementet definierar hello-versionen av hello mallen språket och används för syntaxmarkering för Visual Studio och liknande funktioner för verifiering.</span><span class="sxs-lookup"><span data-stu-id="8695c-115">hello `$schema` element defines hello version of hello template language and is used for Visual Studio syntax highlighting and similar validation features.</span></span> <span data-ttu-id="8695c-116">Hej `contentVersion` elementet används inte av Azure.</span><span class="sxs-lookup"><span data-stu-id="8695c-116">hello `contentVersion` element is not used by Azure.</span></span> <span data-ttu-id="8695c-117">I stället hjälper dig att hålla reda på hello versionen av principmallen.</span><span class="sxs-lookup"><span data-stu-id="8695c-117">Instead, it helps you keep track of hello template version.</span></span>

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
```
## <a name="define-parameters"></a><span data-ttu-id="8695c-118">Definiera parametrar</span><span class="sxs-lookup"><span data-stu-id="8695c-118">Define parameters</span></span>
<span data-ttu-id="8695c-119">Nu ska vi definierar två parametrar `adminUsername` och `adminPassword`.</span><span class="sxs-lookup"><span data-stu-id="8695c-119">Next, we define two parameters, `adminUsername` and `adminPassword`.</span></span> <span data-ttu-id="8695c-120">Parametrarna är värden som du anger vid hello tiden för distributionen.</span><span class="sxs-lookup"><span data-stu-id="8695c-120">Parameters are values you specify at hello time of deployment.</span></span> <span data-ttu-id="8695c-121">Hej `adminUsername` parametern är helt enkelt en `string` typ, men eftersom `adminPassword` är en hemlighet vi ge den typen `securestring`.</span><span class="sxs-lookup"><span data-stu-id="8695c-121">hello `adminUsername` parameter is simply a `string` type, but because `adminPassword` is a secret, we give it type `securestring`.</span></span> <span data-ttu-id="8695c-122">Dessa parametrar skickas senare i hello scale set-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="8695c-122">Later, these parameters are passed into hello scale set configuration.</span></span>

```json
  "parameters": {
    "adminUsername": {
      "type": "string"
    },
    "adminPassword": {
      "type": "securestring"
    }
  },
```
## <a name="define-variables"></a><span data-ttu-id="8695c-123">Definiera variabler</span><span class="sxs-lookup"><span data-stu-id="8695c-123">Define variables</span></span>
<span data-ttu-id="8695c-124">Resource Manager-mallar kan du definiera variabler toobe används senare i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="8695c-124">Resource Manager templates also let you define variables toobe used later in hello template.</span></span> <span data-ttu-id="8695c-125">Vårt exempel används inte några variabler, så vi har tomt hello JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="8695c-125">Our example doesn't use any variables, so we've left hello JSON object empty.</span></span>

```json
  "variables": {},
```

## <a name="define-resources"></a><span data-ttu-id="8695c-126">Definiera resurser</span><span class="sxs-lookup"><span data-stu-id="8695c-126">Define resources</span></span>
<span data-ttu-id="8695c-127">Nästa är hello resurser avsnitt i hello mall.</span><span class="sxs-lookup"><span data-stu-id="8695c-127">Next is hello resources section of hello template.</span></span> <span data-ttu-id="8695c-128">Här kan du definiera vad som faktiskt ska toodeploy.</span><span class="sxs-lookup"><span data-stu-id="8695c-128">Here, you define what you actually want toodeploy.</span></span> <span data-ttu-id="8695c-129">Till skillnad från `parameters` och `variables` (som är JSON-objekt), `resources` är en JSON-lista över JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="8695c-129">Unlike `parameters` and `variables` (which are JSON objects), `resources` is a JSON list of JSON objects.</span></span>

```json
   "resources": [
```

<span data-ttu-id="8695c-130">Alla resurser som kräver `type`, `name`, `apiVersion`, och `location` egenskaper.</span><span class="sxs-lookup"><span data-stu-id="8695c-130">All resources require `type`, `name`, `apiVersion`, and `location` properties.</span></span> <span data-ttu-id="8695c-131">Det här exemplet första resursen har typen `Microsft.Network/virtualNetwork`och namnet `myVnet`, och apiVersion `2016-03-30`.</span><span class="sxs-lookup"><span data-stu-id="8695c-131">This example's first resource has type `Microsft.Network/virtualNetwork`, name `myVnet`, and apiVersion `2016-03-30`.</span></span> <span data-ttu-id="8695c-132">(toofind hello senaste API-versionen för en resurstyp finns hello [Azure REST API-dokumentation](https://docs.microsoft.com/rest/api/).)</span><span class="sxs-lookup"><span data-stu-id="8695c-132">(toofind hello latest API version for a resource type, see hello [Azure REST API documentation](https://docs.microsoft.com/rest/api/).)</span></span>

```json
     {
       "type": "Microsoft.Network/virtualNetworks",
       "name": "myVnet",
       "apiVersion": "2016-12-01",
```

## <a name="specify-location"></a><span data-ttu-id="8695c-133">Ange plats</span><span class="sxs-lookup"><span data-stu-id="8695c-133">Specify location</span></span>
<span data-ttu-id="8695c-134">toospecify hello plats för hello virtuella nätverket, använder vi en [Resource Manager mallfunktionen](../azure-resource-manager/resource-group-template-functions.md).</span><span class="sxs-lookup"><span data-stu-id="8695c-134">toospecify hello location for hello virtual network, we use a [Resource Manager template function](../azure-resource-manager/resource-group-template-functions.md).</span></span> <span data-ttu-id="8695c-135">Den här funktionen måste stå inom citattecken och hakparenteser så här: `"[<template-function>]"`.</span><span class="sxs-lookup"><span data-stu-id="8695c-135">This function must be enclosed in quotes and square brackets like this: `"[<template-function>]"`.</span></span> <span data-ttu-id="8695c-136">I detta fall kan vi använda hello `resourceGroup` funktion.</span><span class="sxs-lookup"><span data-stu-id="8695c-136">In this case, we use hello `resourceGroup` function.</span></span> <span data-ttu-id="8695c-137">Det tar i inga argument och returnerar ett JSON-objekt med metadata om hello resursgruppen distributionen distribueras till.</span><span class="sxs-lookup"><span data-stu-id="8695c-137">It takes in no arguments and returns a JSON object with metadata about hello resource group this deployment is being deployed to.</span></span> <span data-ttu-id="8695c-138">hello resursgruppen har angetts av hello användare vid hello tiden för distributionen.</span><span class="sxs-lookup"><span data-stu-id="8695c-138">hello resource group is set by hello user at hello time of deployment.</span></span> <span data-ttu-id="8695c-139">Vi sedan index i den här JSON-objekt med `.location` tooget hello plats från hello JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="8695c-139">We then index into this JSON object with `.location` tooget hello location from hello JSON object.</span></span>

```json
       "location": "[resourceGroup().location]",
```

## <a name="specify-virtual-network-properties"></a><span data-ttu-id="8695c-140">Ange egenskaper för virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="8695c-140">Specify virtual network properties</span></span>
<span data-ttu-id="8695c-141">Varje Resource Manager-resurs har sin egen `properties` avsnittet för konfigurationer specifika toohello resurs.</span><span class="sxs-lookup"><span data-stu-id="8695c-141">Each Resource Manager resource has its own `properties` section for configurations specific toohello resource.</span></span> <span data-ttu-id="8695c-142">I det här fallet anger vi som hello det virtuella nätverket ska ha ett undernät med hello privat IP-adressintervall `10.0.0.0/16`.</span><span class="sxs-lookup"><span data-stu-id="8695c-142">In this case, we specify that hello virtual network should have one subnet using hello private IP address range `10.0.0.0/16`.</span></span> <span data-ttu-id="8695c-143">En skaluppsättning för är alltid finns i ett undernät.</span><span class="sxs-lookup"><span data-stu-id="8695c-143">A scale set is always contained within one subnet.</span></span> <span data-ttu-id="8695c-144">Det går inte att sträcka sig över undernät.</span><span class="sxs-lookup"><span data-stu-id="8695c-144">It cannot span subnets.</span></span>

```json
       "properties": {
         "addressSpace": {
           "addressPrefixes": [
             "10.0.0.0/16"
           ]
         },
         "subnets": [
           {
             "name": "mySubnet",
             "properties": {
               "addressPrefix": "10.0.0.0/16"
             }
           }
         ]
       }
     },
```

## <a name="add-dependson-list"></a><span data-ttu-id="8695c-145">Lägg till dependsOn-lista</span><span class="sxs-lookup"><span data-stu-id="8695c-145">Add dependsOn list</span></span>
<span data-ttu-id="8695c-146">Dessutom krävs toohello `type`, `name`, `apiVersion`, och `location` egenskaper för varje resurs kan ha en valfri `dependsOn` lista med strängar.</span><span class="sxs-lookup"><span data-stu-id="8695c-146">In addition toohello required `type`, `name`, `apiVersion`, and `location` properties, each resource can have an optional `dependsOn` list of strings.</span></span> <span data-ttu-id="8695c-147">Den här listan anger vilken andra resurser från den här distributionen måste avslutas innan du distribuerar den här resursen.</span><span class="sxs-lookup"><span data-stu-id="8695c-147">This list specifies which other resources from this deployment must finish before deploying this resource.</span></span>

<span data-ttu-id="8695c-148">I det här fallet finns bara ett element i listan hello hello virtuella nätverket från hello föregående exempel.</span><span class="sxs-lookup"><span data-stu-id="8695c-148">In this case, there is only one element in hello list, hello virtual network from hello previous example.</span></span> <span data-ttu-id="8695c-149">Vi kan ange detta beroende eftersom hello skaluppsättning behov hello nätverket tooexist innan du skapar virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="8695c-149">We specify this dependency because hello scale set needs hello network tooexist before creating any VMs.</span></span> <span data-ttu-id="8695c-150">Det här sättet hello skaluppsättning kan ge dessa privata IP-adresser för virtuella datorer från hello IP-adressintervall som tidigare angetts i hello-egenskaper.</span><span class="sxs-lookup"><span data-stu-id="8695c-150">This way, hello scale set can give these VMs private IP addresses from hello IP address range previously specified in hello network properties.</span></span> <span data-ttu-id="8695c-151">varje sträng i hello dependsOn listan hello format är `<type>/<name>`.</span><span class="sxs-lookup"><span data-stu-id="8695c-151">hello format of each string in hello dependsOn list is `<type>/<name>`.</span></span> <span data-ttu-id="8695c-152">Använd hello samma `type` och `name` använt tidigare i resursdefinitionen i hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="8695c-152">Use hello same `type` and `name` used previously in hello virtual network resource definition.</span></span>

```json
     {
       "type": "Microsoft.Compute/virtualMachineScaleSets",
       "name": "myScaleSet",
       "apiVersion": "2016-04-30-preview",
       "location": "[resourceGroup().location]",
       "dependsOn": [
         "Microsoft.Network/virtualNetworks/myVnet"
       ],
```
## <a name="specify-scale-set-properties"></a><span data-ttu-id="8695c-153">Ange egenskaper för skalan</span><span class="sxs-lookup"><span data-stu-id="8695c-153">Specify scale set properties</span></span>
<span data-ttu-id="8695c-154">Skaluppsättningar har många egenskaper för att anpassa hello virtuella datorer i hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="8695c-154">Scale sets have many properties for customizing hello VMs in hello scale set.</span></span> <span data-ttu-id="8695c-155">En fullständig lista över de här egenskaperna finns hello [skaluppsättning REST API-dokumentation](https://docs.microsoft.com/en-us/rest/api/virtualmachinescalesets/create-or-update-a-set).</span><span class="sxs-lookup"><span data-stu-id="8695c-155">For a full list of these properties, see hello [scale set REST API documentation](https://docs.microsoft.com/en-us/rest/api/virtualmachinescalesets/create-or-update-a-set).</span></span> <span data-ttu-id="8695c-156">För den här självstudiekursen kommer vi ange några vanliga egenskaper.</span><span class="sxs-lookup"><span data-stu-id="8695c-156">For this tutorial, we will set only a few commonly used properties.</span></span>
### <a name="supply-vm-size-and-capacity"></a><span data-ttu-id="8695c-157">Ange VM-storlek och kapacitet</span><span class="sxs-lookup"><span data-stu-id="8695c-157">Supply VM size and capacity</span></span>
<span data-ttu-id="8695c-158">Hej skaluppsättning behov tooknow vilka storleken på VM-toocreate (”sku namn”) och hur många sådana VMs toocreate (”artikelnummerkapaciteten”).</span><span class="sxs-lookup"><span data-stu-id="8695c-158">hello scale set needs tooknow what size of VM toocreate ("sku name") and how many such VMs toocreate ("sku capacity").</span></span> <span data-ttu-id="8695c-159">toosee vilka VM-storlekar är tillgängliga, se hello [storlekar på VM-dokumentationen](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-sizes).</span><span class="sxs-lookup"><span data-stu-id="8695c-159">toosee which VM sizes are available, see hello [VM Sizes documentation](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-sizes).</span></span>

```json
       "sku": {
         "name": "Standard_A1",
         "capacity": 2
       },
```

### <a name="choose-type-of-updates"></a><span data-ttu-id="8695c-160">Välj typ av uppdateringar</span><span class="sxs-lookup"><span data-stu-id="8695c-160">Choose type of updates</span></span>
<span data-ttu-id="8695c-161">Hej skaluppsättning måste också tooknow hur toohandle uppdaterar på hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="8695c-161">hello scale set also needs tooknow how toohandle updates on hello scale set.</span></span> <span data-ttu-id="8695c-162">Det finns två alternativ `Manual` och `Automatic`.</span><span class="sxs-lookup"><span data-stu-id="8695c-162">Currently, there are two options, `Manual` and `Automatic`.</span></span> <span data-ttu-id="8695c-163">Mer information om hello skillnaderna mellan hello två dokumentationen hello på [hur tooupgrade en skaluppsättning](./virtual-machine-scale-sets-upgrade-scale-set.md).</span><span class="sxs-lookup"><span data-stu-id="8695c-163">For more information on hello differences between hello two, see hello documentation on [how tooupgrade a scale set](./virtual-machine-scale-sets-upgrade-scale-set.md).</span></span>

```json
       "properties": {
         "upgradePolicy": {
           "mode": "Manual"
         },
```

### <a name="choose-vm-operating-system"></a><span data-ttu-id="8695c-164">Välj operativsystem för VM</span><span class="sxs-lookup"><span data-stu-id="8695c-164">Choose VM operating system</span></span>
<span data-ttu-id="8695c-165">Hej skaluppsättning behov tooknow vilka operativsystem tooput på hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="8695c-165">hello scale set needs tooknow what operating system tooput on hello VMs.</span></span> <span data-ttu-id="8695c-166">Här kan skapa vi hello virtuella datorer med ett fullständigt korrigeringsfil Ubuntu 16.04 LTS avbildning.</span><span class="sxs-lookup"><span data-stu-id="8695c-166">Here, we create hello VMs with a fully patched Ubuntu 16.04-LTS image.</span></span>

```json
         "virtualMachineProfile": {
           "storageProfile": {
             "imageReference": {
               "publisher": "Canonical",
               "offer": "UbuntuServer",
               "sku": "16.04-LTS",
               "version": "latest"
             }
           },
```

### <a name="specify-computernameprefix"></a><span data-ttu-id="8695c-167">Ange computerNamePrefix</span><span class="sxs-lookup"><span data-stu-id="8695c-167">Specify computerNamePrefix</span></span>
<span data-ttu-id="8695c-168">Hej skaluppsättning distribuerar flera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="8695c-168">hello scale set deploys multiple VMs.</span></span> <span data-ttu-id="8695c-169">I stället för att ange namn på varje virtuell dator kan vi ange `computerNamePrefix`.</span><span class="sxs-lookup"><span data-stu-id="8695c-169">Instead of specifying each VM name, we specify `computerNamePrefix`.</span></span> <span data-ttu-id="8695c-170">hello skaluppsättning lägger till ett index toohello prefix för varje virtuell dator så att VM-namn har hello formuläret `<computerNamePrefix>_<auto-generated-index>`.</span><span class="sxs-lookup"><span data-stu-id="8695c-170">hello scale set appends an index toohello prefix for each VM, so VM names have hello form `<computerNamePrefix>_<auto-generated-index>`.</span></span>

<span data-ttu-id="8695c-171">I följande fragment hello, använda vi hello parametrar från innan tooset Hej administratörsanvändarnamn och lösenord för alla virtuella datorer i hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="8695c-171">In hello following snippet, we use hello parameters from before tooset hello administrator username and password for all VMs in hello scale set.</span></span> <span data-ttu-id="8695c-172">Vi kan göra detta med hello `parameters` mallfunktionen.</span><span class="sxs-lookup"><span data-stu-id="8695c-172">We do this with hello `parameters` template function.</span></span> <span data-ttu-id="8695c-173">Den här funktionen använder en sträng som anger vilka parametern toorefer tooand matar ut hello värde för parametern.</span><span class="sxs-lookup"><span data-stu-id="8695c-173">This function takes in a string that specifies which parameter toorefer tooand outputs hello value for that parameter.</span></span>

```json
           "osProfile": {
             "computerNamePrefix": "vm",
             "adminUsername": "[parameters('adminUsername')]",
             "adminPassword": "[parameters('adminPassword')]"
           },
```

### <a name="specify-vm-network-configuration"></a><span data-ttu-id="8695c-174">Ange konfiguration för VM-nätverk</span><span class="sxs-lookup"><span data-stu-id="8695c-174">Specify VM network configuration</span></span>
<span data-ttu-id="8695c-175">Slutligen måste toospecify hello nätverkskonfigurationen för hello virtuella datorer i hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="8695c-175">Finally, we need toospecify hello network configuration for hello VMs in hello scale set.</span></span> <span data-ttu-id="8695c-176">I det här fallet behöver vi bara toospecify hello-ID för hello undernät som skapats tidigare.</span><span class="sxs-lookup"><span data-stu-id="8695c-176">In this case, we only need toospecify hello ID of hello subnet created earlier.</span></span> <span data-ttu-id="8695c-177">Detta visar hello skaluppsättning tooput hello nätverksgränssnitt i det här undernätet.</span><span class="sxs-lookup"><span data-stu-id="8695c-177">This tells hello scale set tooput hello network interfaces in this subnet.</span></span>

<span data-ttu-id="8695c-178">Du kan hämta hello ID av hello virtuella nätverk som innehåller hello undernät med hello `resourceId` mallfunktionen.</span><span class="sxs-lookup"><span data-stu-id="8695c-178">You can get hello ID of hello virtual network containing hello subnet by using hello `resourceId` template function.</span></span> <span data-ttu-id="8695c-179">Den här funktionen använder hello typ och namn på en resurs och returnerar hello fullständigt kvalificerade identifieraren för den här resursen.</span><span class="sxs-lookup"><span data-stu-id="8695c-179">This function takes in hello type and name of a resource and returns hello fully qualified identifier of that resource.</span></span> <span data-ttu-id="8695c-180">Detta ID har hello formuläret:`/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/<resourceProviderNamespace>/<resourceType>/<resourceName>`</span><span class="sxs-lookup"><span data-stu-id="8695c-180">This ID has hello form: `/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/<resourceProviderNamespace>/<resourceType>/<resourceName>`</span></span>

<span data-ttu-id="8695c-181">Dock är hello identifierare för hello virtuellt nätverk inte tillräckligt.</span><span class="sxs-lookup"><span data-stu-id="8695c-181">However, hello identifier of hello virtual network is not enough.</span></span> <span data-ttu-id="8695c-182">Du måste ange hello specifika undernät som hello skaluppsättning för virtuella datorer måste vara i.</span><span class="sxs-lookup"><span data-stu-id="8695c-182">You must specify hello specific subnet that hello scale set VMs should be in.</span></span> <span data-ttu-id="8695c-183">toodo, sammanfoga `/subnets/mySubnet` toohello ID för hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="8695c-183">toodo this, concatenate `/subnets/mySubnet` toohello ID of hello virtual network.</span></span> <span data-ttu-id="8695c-184">hello resultatet är hello fullständigt kvalificerade ID hello undernät.</span><span class="sxs-lookup"><span data-stu-id="8695c-184">hello result is hello fully qualified ID of hello subnet.</span></span> <span data-ttu-id="8695c-185">Gör den här sammanfogning med hello `concat` funktion, vilket tar i en serie med strängar och returnerar sina sammanfogning.</span><span class="sxs-lookup"><span data-stu-id="8695c-185">Do this concatenation with hello `concat` function, which takes in a series of strings and returns their concatenation.</span></span>

```json
           "networkProfile": {
             "networkInterfaceConfigurations": [
               {
                 "name": "myNic",
                 "properties": {
                   "primary": "true",
                   "ipConfigurations": [
                     {
                       "name": "myIpConfig",
                       "properties": {
                         "subnet": {
                           "id": "[concat(resourceId('Microsoft.Network/virtualNetworks', 'myVnet'), '/subnets/mySubnet')]"
                         }
                       }
                     }
                   ]
                 }
               }
             ]
           }
         }
       }
     }
   ]
}

```

## <a name="next-steps"></a><span data-ttu-id="8695c-186">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8695c-186">Next steps</span></span>

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
