---
title: "aaaOMS tillägg för Azure virtuell dator för Linux | Microsoft Docs"
description: "Distribuera hello OMS-agent på Linux-dator som använder ett tillägg för virtuell dator."
services: virtual-machines-linux
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c7bbf210-7d71-4a37-ba47-9c74567a9ea6
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: nepeters
ms.openlocfilehash: 0fc8003d1fae6c043eef18ae78d12f9304b70832
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="oms-virtual-machine-extension-for-linux"></a><span data-ttu-id="e9d2f-103">OMS tillägg för virtuell dator för Linux</span><span class="sxs-lookup"><span data-stu-id="e9d2f-103">OMS virtual machine extension for Linux</span></span>

## <a name="overview"></a><span data-ttu-id="e9d2f-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="e9d2f-104">Overview</span></span>

<span data-ttu-id="e9d2f-105">Operations Management Suite (OMS) ger funktioner för övervakning, aviseringar, och reparationen i molnet och lokala resurser.</span><span class="sxs-lookup"><span data-stu-id="e9d2f-105">Operations Management Suite (OMS) provides monitoring, alerting, and alert remediation capabilities across cloud and on-premises assets.</span></span> <span data-ttu-id="e9d2f-106">hello tillägg för virtuell dator i OMS-Agent för Linux publiceras och stöds av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="e9d2f-106">hello OMS Agent virtual machine extension for Linux is published and supported by Microsoft.</span></span> <span data-ttu-id="e9d2f-107">hello-tillägget hello OMS-agent installeras på virtuella Azure-datorer och registrerar virtuella datorer i en befintlig OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="e9d2f-107">hello extension installs hello OMS agent on Azure virtual machines, and enrolls virtual machines into an existing OMS workspace.</span></span> <span data-ttu-id="e9d2f-108">Det här dokumentet information hello stöds plattformar, konfigurationer och distributionsalternativ för hello OMS-tillägg för virtuell dator för Linux.</span><span class="sxs-lookup"><span data-stu-id="e9d2f-108">This document details hello supported platforms, configurations, and deployment options for hello OMS virtual machine extension for Linux.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e9d2f-109">Krav</span><span class="sxs-lookup"><span data-stu-id="e9d2f-109">Prerequisites</span></span>

### <a name="operating-system"></a><span data-ttu-id="e9d2f-110">Operativsystem</span><span class="sxs-lookup"><span data-stu-id="e9d2f-110">Operating system</span></span>

<span data-ttu-id="e9d2f-111">hello tillägget OMS-Agent kan köras mot dessa Linux-distributioner.</span><span class="sxs-lookup"><span data-stu-id="e9d2f-111">hello OMS Agent extension can be run against these Linux distributions.</span></span>

| <span data-ttu-id="e9d2f-112">Distribution</span><span class="sxs-lookup"><span data-stu-id="e9d2f-112">Distribution</span></span> | <span data-ttu-id="e9d2f-113">Version</span><span class="sxs-lookup"><span data-stu-id="e9d2f-113">Version</span></span> |
|---|---|
| <span data-ttu-id="e9d2f-114">CentOS Linux</span><span class="sxs-lookup"><span data-stu-id="e9d2f-114">CentOS Linux</span></span> | <span data-ttu-id="e9d2f-115">5, 6 och 7</span><span class="sxs-lookup"><span data-stu-id="e9d2f-115">5, 6, and 7</span></span> |
| <span data-ttu-id="e9d2f-116">Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="e9d2f-116">Oracle Linux</span></span> | <span data-ttu-id="e9d2f-117">5, 6 och 7</span><span class="sxs-lookup"><span data-stu-id="e9d2f-117">5, 6, and 7</span></span> |
| <span data-ttu-id="e9d2f-118">Red Hat Enterprise Linux Server</span><span class="sxs-lookup"><span data-stu-id="e9d2f-118">Red Hat Enterprise Linux Server</span></span> | <span data-ttu-id="e9d2f-119">5, 6 och 7</span><span class="sxs-lookup"><span data-stu-id="e9d2f-119">5, 6 and 7</span></span> |
| <span data-ttu-id="e9d2f-120">Debian GNU/Linux</span><span class="sxs-lookup"><span data-stu-id="e9d2f-120">Debian GNU/Linux</span></span> | <span data-ttu-id="e9d2f-121">6, 7 och 8</span><span class="sxs-lookup"><span data-stu-id="e9d2f-121">6, 7, and 8</span></span> |
| <span data-ttu-id="e9d2f-122">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="e9d2f-122">Ubuntu</span></span> | <span data-ttu-id="e9d2f-123">12.04 LTS, 14.04 LTS, 15.04, 15.10, 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="e9d2f-123">12.04 LTS, 14.04 LTS, 15.04, 15.10, 16.04 LTS</span></span> |
| <span data-ttu-id="e9d2f-124">SUSE Linux Enterprise Server</span><span class="sxs-lookup"><span data-stu-id="e9d2f-124">SUSE Linux Enterprise Server</span></span> | <span data-ttu-id="e9d2f-125">11 och 12</span><span class="sxs-lookup"><span data-stu-id="e9d2f-125">11 and 12</span></span> |

### <a name="internet-connectivity"></a><span data-ttu-id="e9d2f-126">Internetanslutning</span><span class="sxs-lookup"><span data-stu-id="e9d2f-126">Internet connectivity</span></span>

<span data-ttu-id="e9d2f-127">hello tillägget OMS-Agent för Linux kräver att hello mål den virtuella datorn är ansluten toohello internet.</span><span class="sxs-lookup"><span data-stu-id="e9d2f-127">hello OMS Agent extension for Linux requires that hello target virtual machine is connected toohello internet.</span></span> 

## <a name="extension-schema"></a><span data-ttu-id="e9d2f-128">Tilläggsschema</span><span class="sxs-lookup"><span data-stu-id="e9d2f-128">Extension schema</span></span>

<span data-ttu-id="e9d2f-129">hello visar följande JSON hello schemat för hello tillägget OMS-Agent.</span><span class="sxs-lookup"><span data-stu-id="e9d2f-129">hello following JSON shows hello schema for hello OMS Agent extension.</span></span> <span data-ttu-id="e9d2f-130">hello tillägget kräver hello-ID och arbetsytan arbetsytenyckel från hello mål OMS-arbetsytan; Dessa värden finns i hello OMS-portalen.</span><span class="sxs-lookup"><span data-stu-id="e9d2f-130">hello extension requires hello workspace ID and workspace key from hello target OMS workspace; these values can be found in hello OMS portal.</span></span> <span data-ttu-id="e9d2f-131">Eftersom hello arbetsytenyckel ska behandlas som känsliga data, bör det lagras i en Inställningskonfiguration för skyddade.</span><span class="sxs-lookup"><span data-stu-id="e9d2f-131">Because hello workspace key should be treated as sensitive data, it should be stored in a protected setting configuration.</span></span> <span data-ttu-id="e9d2f-132">Azure VM tillägget skyddade inställningsdata krypteras och dekrypteras endast på hello virtuella måldatorn.</span><span class="sxs-lookup"><span data-stu-id="e9d2f-132">Azure VM extension protected setting data is encrypted, and only decrypted on hello target virtual machine.</span></span> <span data-ttu-id="e9d2f-133">Observera att **workspaceId** och **workspaceKey** är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="e9d2f-133">Note that **workspaceId** and **workspaceKey** are case-sensitive.</span></span>

```json
{
  "type": "extensions",
  "name": "OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.4",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

### <a name="property-values"></a><span data-ttu-id="e9d2f-134">Egenskapsvärden</span><span class="sxs-lookup"><span data-stu-id="e9d2f-134">Property values</span></span>

| <span data-ttu-id="e9d2f-135">Namn</span><span class="sxs-lookup"><span data-stu-id="e9d2f-135">Name</span></span> | <span data-ttu-id="e9d2f-136">Värdet / exempel</span><span class="sxs-lookup"><span data-stu-id="e9d2f-136">Value / Example</span></span> |
| ---- | ---- |
| <span data-ttu-id="e9d2f-137">apiVersion</span><span class="sxs-lookup"><span data-stu-id="e9d2f-137">apiVersion</span></span> | <span data-ttu-id="e9d2f-138">2015-06-15</span><span class="sxs-lookup"><span data-stu-id="e9d2f-138">2015-06-15</span></span> |
| <span data-ttu-id="e9d2f-139">Publisher</span><span class="sxs-lookup"><span data-stu-id="e9d2f-139">publisher</span></span> | <span data-ttu-id="e9d2f-140">Microsoft.EnterpriseCloud.Monitoring</span><span class="sxs-lookup"><span data-stu-id="e9d2f-140">Microsoft.EnterpriseCloud.Monitoring</span></span> |
| <span data-ttu-id="e9d2f-141">typ</span><span class="sxs-lookup"><span data-stu-id="e9d2f-141">type</span></span> | <span data-ttu-id="e9d2f-142">OmsAgentForLinux</span><span class="sxs-lookup"><span data-stu-id="e9d2f-142">OmsAgentForLinux</span></span> |
| <span data-ttu-id="e9d2f-143">typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="e9d2f-143">typeHandlerVersion</span></span> | <span data-ttu-id="e9d2f-144">1.4</span><span class="sxs-lookup"><span data-stu-id="e9d2f-144">1.4</span></span> |
| <span data-ttu-id="e9d2f-145">workspaceId (t.ex.)</span><span class="sxs-lookup"><span data-stu-id="e9d2f-145">workspaceId (e.g)</span></span> | <span data-ttu-id="e9d2f-146">6f680a37-00c6-41C7-a93f-1437e3462574</span><span class="sxs-lookup"><span data-stu-id="e9d2f-146">6f680a37-00c6-41c7-a93f-1437e3462574</span></span> |
| <span data-ttu-id="e9d2f-147">workspaceKey (t.ex.)</span><span class="sxs-lookup"><span data-stu-id="e9d2f-147">workspaceKey (e.g)</span></span> | <span data-ttu-id="e9d2f-148">z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI + rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ ==</span><span class="sxs-lookup"><span data-stu-id="e9d2f-148">z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI+rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ==</span></span> |


## <a name="template-deployment"></a><span data-ttu-id="e9d2f-149">Malldistribution</span><span class="sxs-lookup"><span data-stu-id="e9d2f-149">Template deployment</span></span>

<span data-ttu-id="e9d2f-150">Azure VM-tillägg kan distribueras med Azure Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="e9d2f-150">Azure VM extensions can be deployed with Azure Resource Manager templates.</span></span> <span data-ttu-id="e9d2f-151">Mallar är perfekt när du distribuerar en eller flera virtuella datorer som kräver post distributionskonfiguration, till exempel onboarding tooOMS.</span><span class="sxs-lookup"><span data-stu-id="e9d2f-151">Templates are ideal when deploying one or more virtual machines that require post deployment configuration such as onboarding tooOMS.</span></span> <span data-ttu-id="e9d2f-152">En Resource Manager-mall som innehåller hello OMS-agenten VM-tillägget kan hittas på hello [Azure Quick Start-galleriet](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-ubuntu-vm).</span><span class="sxs-lookup"><span data-stu-id="e9d2f-152">A sample Resource Manager template that includes hello OMS Agent VM extension can be found on hello [Azure Quick Start Gallery](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-ubuntu-vm).</span></span> 

<span data-ttu-id="e9d2f-153">hello JSON-konfiguration för ett tillägg för virtuell dator kan kapslas i hello virtuell datorresurs eller placeras på hello rot- eller översta nivån i en Resource Manager JSON-mall.</span><span class="sxs-lookup"><span data-stu-id="e9d2f-153">hello JSON configuration for a virtual machine extension can be nested inside hello virtual machine resource, or placed at hello root or top level of a Resource Manager JSON template.</span></span> <span data-ttu-id="e9d2f-154">hello placeringen av hello JSON configuration påverkar hello värdet hello resursens namn och typ.</span><span class="sxs-lookup"><span data-stu-id="e9d2f-154">hello placement of hello JSON configuration affects hello value of hello resource name and type.</span></span> <span data-ttu-id="e9d2f-155">Mer information finns i [ange namn och typ för underordnade resurser](../../azure-resource-manager/resource-manager-template-child-resource.md).</span><span class="sxs-lookup"><span data-stu-id="e9d2f-155">For more information, see [Set name and type for child resources](../../azure-resource-manager/resource-manager-template-child-resource.md).</span></span> 

<span data-ttu-id="e9d2f-156">hello förutsätter följande exempel hello OMS-tillägget är kapslad i hello virtuella datorresurser.</span><span class="sxs-lookup"><span data-stu-id="e9d2f-156">hello following example assumes hello OMS extension is nested inside hello virtual machine resource.</span></span> <span data-ttu-id="e9d2f-157">När för många kapslade hello tillägget resurs, hello JSON placeras i hello `"resources": []` objekt av hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e9d2f-157">When nesting hello extension resource, hello JSON is placed in hello `"resources": []` object of hello virtual machine.</span></span>

```json
{
  "type": "extensions",
  "name": "OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.4",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

<span data-ttu-id="e9d2f-158">När du monterar hello tillägget JSON hello roten i hello mallen hello resursnamnet innehåller en referens toohello överordnad virtuell dator och hello typen visar hello kapslade konfiguration.</span><span class="sxs-lookup"><span data-stu-id="e9d2f-158">When placing hello extension JSON at hello root of hello template, hello resource name includes a reference toohello parent virtual machine, and hello type reflects hello nested configuration.</span></span>  

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "<parentVmResource>/OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.4",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

## <a name="azure-cli-deployment"></a><span data-ttu-id="e9d2f-159">Azure CLI-distribution</span><span class="sxs-lookup"><span data-stu-id="e9d2f-159">Azure CLI deployment</span></span>

<span data-ttu-id="e9d2f-160">hello Azure CLI kan vara används toodeploy hello OMS-agenten VM-tillägget tooan befintlig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e9d2f-160">hello Azure CLI can be used toodeploy hello OMS Agent VM extension tooan existing virtual machine.</span></span> <span data-ttu-id="e9d2f-161">Ersätt hello OMS-nyckel och OMS-ID med de från din OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="e9d2f-161">Replace hello OMS key and OMS ID with those from your OMS workspace.</span></span> 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name OmsAgentForLinux \
  --publisher Microsoft.EnterpriseCloud.Monitoring \
  --version 1.4 --protected-settings '{"workspaceKey": "omskey"}' \
  --settings '{"workspaceId": "omsid"}'
```

## <a name="troubleshoot-and-support"></a><span data-ttu-id="e9d2f-162">Felsöka och stöd</span><span class="sxs-lookup"><span data-stu-id="e9d2f-162">Troubleshoot and support</span></span>

### <a name="troubleshoot"></a><span data-ttu-id="e9d2f-163">Felsöka</span><span class="sxs-lookup"><span data-stu-id="e9d2f-163">Troubleshoot</span></span>

<span data-ttu-id="e9d2f-164">Data om hello tillståndet för distributioner av tillägget kan hämtas från hello Azure-portalen och genom att använda hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="e9d2f-164">Data about hello state of extension deployments can be retrieved from hello Azure portal, and by using hello Azure CLI.</span></span> <span data-ttu-id="e9d2f-165">toosee hello distributionsstatusen för tillägg för en viss virtuell dator, kör följande kommando använder hello hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="e9d2f-165">toosee hello deployment state of extensions for a given VM, run hello following command using hello Azure CLI.</span></span>

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

<span data-ttu-id="e9d2f-166">Tillägget körning utdata är loggade toohello följande:</span><span class="sxs-lookup"><span data-stu-id="e9d2f-166">Extension execution output is logged toohello following file:</span></span>

```
/opt/microsoft/omsagent/bin/stdout
```

### <a name="error-codes-and-their-meanings"></a><span data-ttu-id="e9d2f-167">Felkoder och deras innebörd</span><span class="sxs-lookup"><span data-stu-id="e9d2f-167">Error codes and their meanings</span></span>

| <span data-ttu-id="e9d2f-168">Felkod</span><span class="sxs-lookup"><span data-stu-id="e9d2f-168">Error Code</span></span> | <span data-ttu-id="e9d2f-169">Betydelse</span><span class="sxs-lookup"><span data-stu-id="e9d2f-169">Meaning</span></span> | <span data-ttu-id="e9d2f-170">Möjlig åtgärd</span><span class="sxs-lookup"><span data-stu-id="e9d2f-170">Possible Action</span></span> |
| :---: | --- | --- |
| <span data-ttu-id="e9d2f-171">10</span><span class="sxs-lookup"><span data-stu-id="e9d2f-171">10</span></span> | <span data-ttu-id="e9d2f-172">Virtuella datorn är redan anslutet tooan OMS-arbetsyta</span><span class="sxs-lookup"><span data-stu-id="e9d2f-172">VM is already connected tooan OMS workspace</span></span> | <span data-ttu-id="e9d2f-173">tooconnect hello VM toohello arbetsytan som angetts i hello tillägget schemat, ange stopOnMultipleConnections toofalse i inställningar för offentliga eller ta bort den här egenskapen.</span><span class="sxs-lookup"><span data-stu-id="e9d2f-173">tooconnect hello VM toohello workspace specified in hello extension schema, set stopOnMultipleConnections toofalse in public settings or remove this property.</span></span> <span data-ttu-id="e9d2f-174">Den här virtuella datorn hämtar debiteras när för varje arbetsyta som den är ansluten till.</span><span class="sxs-lookup"><span data-stu-id="e9d2f-174">This VM gets billed once for each workspace it is connected to.</span></span> |
| <span data-ttu-id="e9d2f-175">11</span><span class="sxs-lookup"><span data-stu-id="e9d2f-175">11</span></span> | <span data-ttu-id="e9d2f-176">Ogiltig config tillhandahålls toohello tillägg</span><span class="sxs-lookup"><span data-stu-id="e9d2f-176">Invalid config provided toohello extension</span></span> | <span data-ttu-id="e9d2f-177">Följ hello föregående exempel tooset alla egenskapsvärden som krävs för distributionen.</span><span class="sxs-lookup"><span data-stu-id="e9d2f-177">Follow hello preceding examples tooset all property values necessary for deployment.</span></span> |
| <span data-ttu-id="e9d2f-178">12</span><span class="sxs-lookup"><span data-stu-id="e9d2f-178">12</span></span> | <span data-ttu-id="e9d2f-179">Hej dpkg Pakethanteraren är låst</span><span class="sxs-lookup"><span data-stu-id="e9d2f-179">hello dpkg package manager is locked</span></span> | <span data-ttu-id="e9d2f-180">Kontrollera att alla dpkg uppdateringsåtgärder på hello datorn har slutförts och försök igen.</span><span class="sxs-lookup"><span data-stu-id="e9d2f-180">Make sure all dpkg update operations on hello machine have finished and retry.</span></span> |
| <span data-ttu-id="e9d2f-181">20</span><span class="sxs-lookup"><span data-stu-id="e9d2f-181">20</span></span> | <span data-ttu-id="e9d2f-182">Aktivera kallas för tidigt</span><span class="sxs-lookup"><span data-stu-id="e9d2f-182">Enable called prematurely</span></span> | <span data-ttu-id="e9d2f-183">[Uppdatera hello Azure Linux-agenten](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/update-agent) toohello senaste tillgängliga versionen.</span><span class="sxs-lookup"><span data-stu-id="e9d2f-183">[Update hello Azure Linux Agent](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/update-agent) toohello latest available version.</span></span> |
| <span data-ttu-id="e9d2f-184">51</span><span class="sxs-lookup"><span data-stu-id="e9d2f-184">51</span></span> | <span data-ttu-id="e9d2f-185">Det här tillägget stöds inte på hello VM-operativsystemet</span><span class="sxs-lookup"><span data-stu-id="e9d2f-185">This extension is not supported on hello VM's operation system</span></span> | |
| <span data-ttu-id="e9d2f-186">55</span><span class="sxs-lookup"><span data-stu-id="e9d2f-186">55</span></span> | <span data-ttu-id="e9d2f-187">Det går inte att ansluta toohello Microsoft Operations Management Suite-tjänsten</span><span class="sxs-lookup"><span data-stu-id="e9d2f-187">Cannot connect toohello Microsoft Operations Management Suite service</span></span> | <span data-ttu-id="e9d2f-188">Kontrollera att hello systemet antingen har Internetåtkomst eller att en giltig HTTP-proxy har tillhandahållits.</span><span class="sxs-lookup"><span data-stu-id="e9d2f-188">Check that hello system either has Internet access, or that a valid HTTP proxy has been provided.</span></span> <span data-ttu-id="e9d2f-189">Kontrollera också hello är korrekt hello arbetsyte-ID.</span><span class="sxs-lookup"><span data-stu-id="e9d2f-189">Additionally, check hello correctness of hello workspace ID.</span></span> |

<span data-ttu-id="e9d2f-190">Mer information om felsökning finns på hello [OMS-Agent för Linux felsökningsguide](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md#).</span><span class="sxs-lookup"><span data-stu-id="e9d2f-190">Additional troubleshooting information can be found on hello [OMS-Agent-for-Linux Troubleshooting Guide](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md#).</span></span>

### <a name="support"></a><span data-ttu-id="e9d2f-191">Support</span><span class="sxs-lookup"><span data-stu-id="e9d2f-191">Support</span></span>

<span data-ttu-id="e9d2f-192">Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta hello Azure experter på hello [MSDN Azure och Stack Overflow-forum](https://azure.microsoft.com/en-us/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="e9d2f-192">If you need more help at any point in this article, you can contact hello Azure experts on hello [MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/en-us/support/forums/).</span></span> <span data-ttu-id="e9d2f-193">Alternativt kan du lagra en incident i Azure-supporten.</span><span class="sxs-lookup"><span data-stu-id="e9d2f-193">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="e9d2f-194">Gå toohello [Azure supportwebbplats](https://azure.microsoft.com/en-us/support/options/) och välja Get support.</span><span class="sxs-lookup"><span data-stu-id="e9d2f-194">Go toohello [Azure support site](https://azure.microsoft.com/en-us/support/options/) and select Get support.</span></span> <span data-ttu-id="e9d2f-195">Information om hur du använder Azure stöder läsa hello [vanliga frågor om Microsoft Azure-supporten](https://azure.microsoft.com/en-us/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="e9d2f-195">For information about using Azure Support, read hello [Microsoft Azure support FAQ](https://azure.microsoft.com/en-us/support/faq/).</span></span>
