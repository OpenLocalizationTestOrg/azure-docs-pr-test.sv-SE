---
title: "Lär dig mer om mallar för virtuella datorer scale set | Microsoft Docs"
description: "Lär dig hur du skapar en lägsta lönsam skala set mall för virtuella datorer"
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
ms.openlocfilehash: 65f02c4675eb752dcc82e9a1d1c7f6c2c193fc32
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="learn-about-virtual-machine-scale-set-templates"></a><span data-ttu-id="28089-103">Lär dig mer om scale set-mallar för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="28089-103">Learn about virtual machine scale set templates</span></span>
<span data-ttu-id="28089-104">[Azure Resource Manager-mallar](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) är ett bra sätt att distribuera grupper av relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="28089-104">[Azure Resource Manager templates](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) are a great way to deploy groups of related resources.</span></span> <span data-ttu-id="28089-105">Den här självstudiekursen serien visar hur du skapar en mall för lägsta lönsam skala och hur du ändrar den här mallen så att den passar olika scenarier.</span><span class="sxs-lookup"><span data-stu-id="28089-105">This tutorial series shows how to create a minimum viable scale set template and how to modify this template to suit various scenarios.</span></span> <span data-ttu-id="28089-106">Alla exempel kommer från den här [GitHub-lagringsplatsen](https://github.com/gatneil/mvss).</span><span class="sxs-lookup"><span data-stu-id="28089-106">All examples come from this [GitHub repository](https://github.com/gatneil/mvss).</span></span> 

<span data-ttu-id="28089-107">Den här mallen är avsedd att vara enkla.</span><span class="sxs-lookup"><span data-stu-id="28089-107">This template is intended to be simple.</span></span> <span data-ttu-id="28089-108">Mer komplett exempel på skalan mallar finns i avsnittet den [Azure Quickstart mallar GitHub-lagringsplatsen](https://github.com/Azure/azure-quickstart-templates) och Sök efter mappar som innehåller strängen `vmss`.</span><span class="sxs-lookup"><span data-stu-id="28089-108">For more complete examples of scale set templates, see the [Azure Quickstart Templates GitHub repository](https://github.com/Azure/azure-quickstart-templates) and search for folders that contain the string `vmss`.</span></span>

<span data-ttu-id="28089-109">Om du redan är bekant med att skapa mallar som du kan hoppa till avsnittet ”nästa steg” om du vill se hur du ändrar den här mallen.</span><span class="sxs-lookup"><span data-stu-id="28089-109">If you are already familiar with creating templates, you can skip to the "Next steps" section to see how to modify this template.</span></span>

## <a name="review-the-template"></a><span data-ttu-id="28089-110">Granska mallen</span><span class="sxs-lookup"><span data-stu-id="28089-110">Review the template</span></span>

<span data-ttu-id="28089-111">Använda GitHub för att granska mall våra lägsta lönsam skala, [azuredeploy.json](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="28089-111">Use GitHub to review our minimum viable scale set template, [azuredeploy.json](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json).</span></span>

<span data-ttu-id="28089-112">I den här självstudiekursen kommer vi undersöka diff (`git diff master minimum-viable-scale-set`) att skapa den lägsta lönsam skalan ange mall bit för bit.</span><span class="sxs-lookup"><span data-stu-id="28089-112">In this tutorial, we examine the diff (`git diff master minimum-viable-scale-set`) to create the minimum viable scale set template piece by piece.</span></span>

## <a name="define-schema-and-contentversion"></a><span data-ttu-id="28089-113">Definiera $schema och contentVersion</span><span class="sxs-lookup"><span data-stu-id="28089-113">Define $schema and contentVersion</span></span>
<span data-ttu-id="28089-114">Först måste vi definiera `$schema` och `contentVersion` i mallen.</span><span class="sxs-lookup"><span data-stu-id="28089-114">First, we define `$schema` and `contentVersion` in the template.</span></span> <span data-ttu-id="28089-115">Den `$schema` elementet definierar versionen av mall-språket och används för syntaxmarkering för Visual Studio och liknande funktioner för verifiering.</span><span class="sxs-lookup"><span data-stu-id="28089-115">The `$schema` element defines the version of the template language and is used for Visual Studio syntax highlighting and similar validation features.</span></span> <span data-ttu-id="28089-116">Den `contentVersion` elementet används inte av Azure.</span><span class="sxs-lookup"><span data-stu-id="28089-116">The `contentVersion` element is not used by Azure.</span></span> <span data-ttu-id="28089-117">I stället hjälper dig att hålla reda på versionen av principmallen.</span><span class="sxs-lookup"><span data-stu-id="28089-117">Instead, it helps you keep track of the template version.</span></span>

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
```
## <a name="define-parameters"></a><span data-ttu-id="28089-118">Definiera parametrar</span><span class="sxs-lookup"><span data-stu-id="28089-118">Define parameters</span></span>
<span data-ttu-id="28089-119">Nu ska vi definierar två parametrar `adminUsername` och `adminPassword`.</span><span class="sxs-lookup"><span data-stu-id="28089-119">Next, we define two parameters, `adminUsername` and `adminPassword`.</span></span> <span data-ttu-id="28089-120">Parametrarna är värden som du anger vid tidpunkten för distribution.</span><span class="sxs-lookup"><span data-stu-id="28089-120">Parameters are values you specify at the time of deployment.</span></span> <span data-ttu-id="28089-121">Den `adminUsername` parametern är helt enkelt en `string` typ, men eftersom `adminPassword` är en hemlighet vi ge den typen `securestring`.</span><span class="sxs-lookup"><span data-stu-id="28089-121">The `adminUsername` parameter is simply a `string` type, but because `adminPassword` is a secret, we give it type `securestring`.</span></span> <span data-ttu-id="28089-122">Dessa parametrar skickas senare i scale set-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="28089-122">Later, these parameters are passed into the scale set configuration.</span></span>

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
## <a name="define-variables"></a><span data-ttu-id="28089-123">Definiera variabler</span><span class="sxs-lookup"><span data-stu-id="28089-123">Define variables</span></span>
<span data-ttu-id="28089-124">Resource Manager-mallar kan du definiera variabler som ska användas senare i mallen.</span><span class="sxs-lookup"><span data-stu-id="28089-124">Resource Manager templates also let you define variables to be used later in the template.</span></span> <span data-ttu-id="28089-125">Vårt exempel används inte några variabler, så vi har tomt JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="28089-125">Our example doesn't use any variables, so we've left the JSON object empty.</span></span>

```json
  "variables": {},
```

## <a name="define-resources"></a><span data-ttu-id="28089-126">Definiera resurser</span><span class="sxs-lookup"><span data-stu-id="28089-126">Define resources</span></span>
<span data-ttu-id="28089-127">Nästa är avsnittet resurser i mallen.</span><span class="sxs-lookup"><span data-stu-id="28089-127">Next is the resources section of the template.</span></span> <span data-ttu-id="28089-128">Här kan definiera du vad du vill distribuera.</span><span class="sxs-lookup"><span data-stu-id="28089-128">Here, you define what you actually want to deploy.</span></span> <span data-ttu-id="28089-129">Till skillnad från `parameters` och `variables` (som är JSON-objekt), `resources` är en JSON-lista över JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="28089-129">Unlike `parameters` and `variables` (which are JSON objects), `resources` is a JSON list of JSON objects.</span></span>

```json
   "resources": [
```

<span data-ttu-id="28089-130">Alla resurser som kräver `type`, `name`, `apiVersion`, och `location` egenskaper.</span><span class="sxs-lookup"><span data-stu-id="28089-130">All resources require `type`, `name`, `apiVersion`, and `location` properties.</span></span> <span data-ttu-id="28089-131">Det här exemplet första resursen har typen `Microsft.Network/virtualNetwork`och namnet `myVnet`, och apiVersion `2016-03-30`.</span><span class="sxs-lookup"><span data-stu-id="28089-131">This example's first resource has type `Microsft.Network/virtualNetwork`, name `myVnet`, and apiVersion `2016-03-30`.</span></span> <span data-ttu-id="28089-132">(Du hittar den senaste API-versionen för en resurstyp i [Azure REST API-dokumentation](https://docs.microsoft.com/rest/api/).)</span><span class="sxs-lookup"><span data-stu-id="28089-132">(To find the latest API version for a resource type, see the [Azure REST API documentation](https://docs.microsoft.com/rest/api/).)</span></span>

```json
     {
       "type": "Microsoft.Network/virtualNetworks",
       "name": "myVnet",
       "apiVersion": "2016-12-01",
```

## <a name="specify-location"></a><span data-ttu-id="28089-133">Ange plats</span><span class="sxs-lookup"><span data-stu-id="28089-133">Specify location</span></span>
<span data-ttu-id="28089-134">Ange platsen för det virtuella nätverket som vi använder en [Resource Manager mallfunktionen](../azure-resource-manager/resource-group-template-functions.md).</span><span class="sxs-lookup"><span data-stu-id="28089-134">To specify the location for the virtual network, we use a [Resource Manager template function](../azure-resource-manager/resource-group-template-functions.md).</span></span> <span data-ttu-id="28089-135">Den här funktionen måste stå inom citattecken och hakparenteser så här: `"[<template-function>]"`.</span><span class="sxs-lookup"><span data-stu-id="28089-135">This function must be enclosed in quotes and square brackets like this: `"[<template-function>]"`.</span></span> <span data-ttu-id="28089-136">I det här fallet används den `resourceGroup` funktion.</span><span class="sxs-lookup"><span data-stu-id="28089-136">In this case, we use the `resourceGroup` function.</span></span> <span data-ttu-id="28089-137">Det tar i inga argument och returnerar ett JSON-objekt med metadata om den här distributionen distribueras till resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="28089-137">It takes in no arguments and returns a JSON object with metadata about the resource group this deployment is being deployed to.</span></span> <span data-ttu-id="28089-138">Resursgruppen har angetts av användaren vid tidpunkten för distribution.</span><span class="sxs-lookup"><span data-stu-id="28089-138">The resource group is set by the user at the time of deployment.</span></span> <span data-ttu-id="28089-139">Vi sedan index i den här JSON-objekt med `.location` att hämta platsen från JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="28089-139">We then index into this JSON object with `.location` to get the location from the JSON object.</span></span>

```json
       "location": "[resourceGroup().location]",
```

## <a name="specify-virtual-network-properties"></a><span data-ttu-id="28089-140">Ange egenskaper för virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="28089-140">Specify virtual network properties</span></span>
<span data-ttu-id="28089-141">Varje Resource Manager-resurs har sin egen `properties` avsnittet för konfigurationer som är specifika för resursen.</span><span class="sxs-lookup"><span data-stu-id="28089-141">Each Resource Manager resource has its own `properties` section for configurations specific to the resource.</span></span> <span data-ttu-id="28089-142">I det här fallet vi anger att det virtuella nätverket ska ha ett undernät med privata IP-adressintervallet `10.0.0.0/16`.</span><span class="sxs-lookup"><span data-stu-id="28089-142">In this case, we specify that the virtual network should have one subnet using the private IP address range `10.0.0.0/16`.</span></span> <span data-ttu-id="28089-143">En skaluppsättning för är alltid finns i ett undernät.</span><span class="sxs-lookup"><span data-stu-id="28089-143">A scale set is always contained within one subnet.</span></span> <span data-ttu-id="28089-144">Det går inte att sträcka sig över undernät.</span><span class="sxs-lookup"><span data-stu-id="28089-144">It cannot span subnets.</span></span>

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

## <a name="add-dependson-list"></a><span data-ttu-id="28089-145">Lägg till dependsOn-lista</span><span class="sxs-lookup"><span data-stu-id="28089-145">Add dependsOn list</span></span>
<span data-ttu-id="28089-146">Förutom de nödvändiga `type`, `name`, `apiVersion`, och `location` egenskaper för varje resurs kan ha en valfri `dependsOn` lista med strängar.</span><span class="sxs-lookup"><span data-stu-id="28089-146">In addition to the required `type`, `name`, `apiVersion`, and `location` properties, each resource can have an optional `dependsOn` list of strings.</span></span> <span data-ttu-id="28089-147">Den här listan anger vilken andra resurser från den här distributionen måste avslutas innan du distribuerar den här resursen.</span><span class="sxs-lookup"><span data-stu-id="28089-147">This list specifies which other resources from this deployment must finish before deploying this resource.</span></span>

<span data-ttu-id="28089-148">I det här fallet finns bara ett element i listan över det virtuella nätverket från föregående exempel.</span><span class="sxs-lookup"><span data-stu-id="28089-148">In this case, there is only one element in the list, the virtual network from the previous example.</span></span> <span data-ttu-id="28089-149">Vi kan ange detta beroende eftersom skalan behöver ange nätverket måste finnas innan du skapar virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="28089-149">We specify this dependency because the scale set needs the network to exist before creating any VMs.</span></span> <span data-ttu-id="28089-150">På så sätt kan skaluppsättning kan ge dessa privata IP-adresser för virtuella datorer från IP-adressintervall som tidigare har angetts i nätverksegenskaperna för.</span><span class="sxs-lookup"><span data-stu-id="28089-150">This way, the scale set can give these VMs private IP addresses from the IP address range previously specified in the network properties.</span></span> <span data-ttu-id="28089-151">Formatet för varje sträng i listan dependsOn är `<type>/<name>`.</span><span class="sxs-lookup"><span data-stu-id="28089-151">The format of each string in the dependsOn list is `<type>/<name>`.</span></span> <span data-ttu-id="28089-152">Använda samma `type` och `name` använt tidigare i resursdefinitionen virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="28089-152">Use the same `type` and `name` used previously in the virtual network resource definition.</span></span>

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
## <a name="specify-scale-set-properties"></a><span data-ttu-id="28089-153">Ange egenskaper för skalan</span><span class="sxs-lookup"><span data-stu-id="28089-153">Specify scale set properties</span></span>
<span data-ttu-id="28089-154">Skaluppsättningar har många egenskaper för att anpassa de virtuella datorerna i skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="28089-154">Scale sets have many properties for customizing the VMs in the scale set.</span></span> <span data-ttu-id="28089-155">En fullständig lista över dessa egenskaper finns i [skaluppsättning REST API-dokumentation](https://docs.microsoft.com/en-us/rest/api/virtualmachinescalesets/create-or-update-a-set).</span><span class="sxs-lookup"><span data-stu-id="28089-155">For a full list of these properties, see the [scale set REST API documentation](https://docs.microsoft.com/en-us/rest/api/virtualmachinescalesets/create-or-update-a-set).</span></span> <span data-ttu-id="28089-156">För den här självstudiekursen kommer vi ange några vanliga egenskaper.</span><span class="sxs-lookup"><span data-stu-id="28089-156">For this tutorial, we will set only a few commonly used properties.</span></span>
### <a name="supply-vm-size-and-capacity"></a><span data-ttu-id="28089-157">Ange VM-storlek och kapacitet</span><span class="sxs-lookup"><span data-stu-id="28089-157">Supply VM size and capacity</span></span>
<span data-ttu-id="28089-158">Skaluppsättning måste du ange vilken storlek för den virtuella datorn för att skapa (”sku namn”) och hur många sådana virtuella datorer att skapa (”artikelnummerkapaciteten”).</span><span class="sxs-lookup"><span data-stu-id="28089-158">The scale set needs to know what size of VM to create ("sku name") and how many such VMs to create ("sku capacity").</span></span> <span data-ttu-id="28089-159">Vilka VM-storlekar är tillgängliga finns i [storlekar på VM-dokumentationen](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-sizes).</span><span class="sxs-lookup"><span data-stu-id="28089-159">To see which VM sizes are available, see the [VM Sizes documentation](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-sizes).</span></span>

```json
       "sku": {
         "name": "Standard_A1",
         "capacity": 2
       },
```

### <a name="choose-type-of-updates"></a><span data-ttu-id="28089-160">Välj typ av uppdateringar</span><span class="sxs-lookup"><span data-stu-id="28089-160">Choose type of updates</span></span>
<span data-ttu-id="28089-161">Skalan också behöver veta hur du hanterar uppdateringar på skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="28089-161">The scale set also needs to know how to handle updates on the scale set.</span></span> <span data-ttu-id="28089-162">Det finns två alternativ `Manual` och `Automatic`.</span><span class="sxs-lookup"><span data-stu-id="28089-162">Currently, there are two options, `Manual` and `Automatic`.</span></span> <span data-ttu-id="28089-163">Mer information om skillnaderna mellan två finns i dokumentationen på [hur du uppgraderar en skalningsuppsättning](./virtual-machine-scale-sets-upgrade-scale-set.md).</span><span class="sxs-lookup"><span data-stu-id="28089-163">For more information on the differences between the two, see the documentation on [how to upgrade a scale set](./virtual-machine-scale-sets-upgrade-scale-set.md).</span></span>

```json
       "properties": {
         "upgradePolicy": {
           "mode": "Manual"
         },
```

### <a name="choose-vm-operating-system"></a><span data-ttu-id="28089-164">Välj operativsystem för VM</span><span class="sxs-lookup"><span data-stu-id="28089-164">Choose VM operating system</span></span>
<span data-ttu-id="28089-165">Skaluppsättning måste du ange vilket operativsystem du vill publicera på de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="28089-165">The scale set needs to know what operating system to put on the VMs.</span></span> <span data-ttu-id="28089-166">Här kan skapa vi de virtuella datorerna med en fullständigt korrigeringsfil Ubuntu 16.04 LTS avbildning.</span><span class="sxs-lookup"><span data-stu-id="28089-166">Here, we create the VMs with a fully patched Ubuntu 16.04-LTS image.</span></span>

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

### <a name="specify-computernameprefix"></a><span data-ttu-id="28089-167">Ange computerNamePrefix</span><span class="sxs-lookup"><span data-stu-id="28089-167">Specify computerNamePrefix</span></span>
<span data-ttu-id="28089-168">Skaluppsättning distribuerar flera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="28089-168">The scale set deploys multiple VMs.</span></span> <span data-ttu-id="28089-169">I stället för att ange namn på varje virtuell dator kan vi ange `computerNamePrefix`.</span><span class="sxs-lookup"><span data-stu-id="28089-169">Instead of specifying each VM name, we specify `computerNamePrefix`.</span></span> <span data-ttu-id="28089-170">Skaluppsättning lägger till ett index till prefix för varje virtuell dator så att VM-namn har formatet `<computerNamePrefix>_<auto-generated-index>`.</span><span class="sxs-lookup"><span data-stu-id="28089-170">The scale set appends an index to the prefix for each VM, so VM names have the form `<computerNamePrefix>_<auto-generated-index>`.</span></span>

<span data-ttu-id="28089-171">I följande fragment använder vi parametrar från innan ange administratörsanvändarnamn och lösenord för alla virtuella datorer i skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="28089-171">In the following snippet, we use the parameters from before to set the administrator username and password for all VMs in the scale set.</span></span> <span data-ttu-id="28089-172">Vi göra detta med den `parameters` mallfunktionen.</span><span class="sxs-lookup"><span data-stu-id="28089-172">We do this with the `parameters` template function.</span></span> <span data-ttu-id="28089-173">Den här funktionen använder en sträng som anger vilken parameter att referera till och anger värdet för parametern.</span><span class="sxs-lookup"><span data-stu-id="28089-173">This function takes in a string that specifies which parameter to refer to and outputs the value for that parameter.</span></span>

```json
           "osProfile": {
             "computerNamePrefix": "vm",
             "adminUsername": "[parameters('adminUsername')]",
             "adminPassword": "[parameters('adminPassword')]"
           },
```

### <a name="specify-vm-network-configuration"></a><span data-ttu-id="28089-174">Ange konfiguration för VM-nätverk</span><span class="sxs-lookup"><span data-stu-id="28089-174">Specify VM network configuration</span></span>
<span data-ttu-id="28089-175">Slutligen behöver vi ange nätverkskonfigurationen för de virtuella datorerna i skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="28089-175">Finally, we need to specify the network configuration for the VMs in the scale set.</span></span> <span data-ttu-id="28089-176">I det här fallet behöver vi bara ange ID för det undernät som skapats tidigare.</span><span class="sxs-lookup"><span data-stu-id="28089-176">In this case, we only need to specify the ID of the subnet created earlier.</span></span> <span data-ttu-id="28089-177">Detta visar skaluppsättningen att placera nätverksgränssnitten i det här undernätet.</span><span class="sxs-lookup"><span data-stu-id="28089-177">This tells the scale set to put the network interfaces in this subnet.</span></span>

<span data-ttu-id="28089-178">Du kan hämta ID för det virtuella nätverket med undernätet med hjälp av den `resourceId` mallfunktionen.</span><span class="sxs-lookup"><span data-stu-id="28089-178">You can get the ID of the virtual network containing the subnet by using the `resourceId` template function.</span></span> <span data-ttu-id="28089-179">Den här funktionen tar i typ och namn på en resurs och returnerar fullständigt kvalificerade identifieraren för den här resursen.</span><span class="sxs-lookup"><span data-stu-id="28089-179">This function takes in the type and name of a resource and returns the fully qualified identifier of that resource.</span></span> <span data-ttu-id="28089-180">Detta ID har formatet:`/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/<resourceProviderNamespace>/<resourceType>/<resourceName>`</span><span class="sxs-lookup"><span data-stu-id="28089-180">This ID has the form: `/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/<resourceProviderNamespace>/<resourceType>/<resourceName>`</span></span>

<span data-ttu-id="28089-181">Identifierare för det virtuella nätverket är dock inte tillräckligt med.</span><span class="sxs-lookup"><span data-stu-id="28089-181">However, the identifier of the virtual network is not enough.</span></span> <span data-ttu-id="28089-182">Du måste ange specifika undernät som den skaluppsättning för virtuella datorer måste vara i.</span><span class="sxs-lookup"><span data-stu-id="28089-182">You must specify the specific subnet that the scale set VMs should be in.</span></span> <span data-ttu-id="28089-183">Om du vill göra detta, sammanfoga `/subnets/mySubnet` -ID: t för det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="28089-183">To do this, concatenate `/subnets/mySubnet` to the ID of the virtual network.</span></span> <span data-ttu-id="28089-184">Resultatet är det fullständigt kvalificerade ID till undernätet.</span><span class="sxs-lookup"><span data-stu-id="28089-184">The result is the fully qualified ID of the subnet.</span></span> <span data-ttu-id="28089-185">Gör den här sammanfogning med den `concat` funktion, vilket tar i en serie med strängar och returnerar sina sammanfogning.</span><span class="sxs-lookup"><span data-stu-id="28089-185">Do this concatenation with the `concat` function, which takes in a series of strings and returns their concatenation.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="28089-186">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="28089-186">Next steps</span></span>

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
