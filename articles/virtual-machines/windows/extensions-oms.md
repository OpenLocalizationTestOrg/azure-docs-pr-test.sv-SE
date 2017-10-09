---
title: "aaaOMS tillägg för Azure virtuell dator för Windows | Microsoft Docs"
description: "Distribuera hello OMS-agent på Windows virtuell dator med ett tillägg för virtuell dator."
services: virtual-machines-windows
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: feae6176-2373-4034-b5d9-a32c6b4e1f10
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/14/2017
ms.author: nepeters
ms.openlocfilehash: 3000f66c0acdec1d1fad2125b8c6b72a92b1ec92
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="oms-virtual-machine-extension-for-windows"></a><span data-ttu-id="0c9bb-103">OMS tillägg för virtuell dator för Windows</span><span class="sxs-lookup"><span data-stu-id="0c9bb-103">OMS virtual machine extension for Windows</span></span>

<span data-ttu-id="0c9bb-104">Operations Management Suite (OMS) ger funktioner för övervakning, aviseringar, och reparationen i molnet och lokala resurser.</span><span class="sxs-lookup"><span data-stu-id="0c9bb-104">Operations Management Suite (OMS) provides monitoring, alerting, and alert remediation capabilities across cloud and on-premises assets.</span></span> <span data-ttu-id="0c9bb-105">hello OMS-Agent tillägg för virtuell dator för Windows publiceras och stöds av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0c9bb-105">hello OMS Agent virtual machine extension for Windows is published and supported by Microsoft.</span></span> <span data-ttu-id="0c9bb-106">hello-tillägget hello OMS-agent installeras på virtuella Azure-datorer och registrerar virtuella datorer i en befintlig OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="0c9bb-106">hello extension installs hello OMS agent on Azure virtual machines, and enrolls virtual machines into an existing OMS workspace.</span></span> <span data-ttu-id="0c9bb-107">Det här dokumentet information hello stöds plattformar, konfigurationer och distributionsalternativ för hello OMS-tillägg för virtuell dator för Windows.</span><span class="sxs-lookup"><span data-stu-id="0c9bb-107">This document details hello supported platforms, configurations, and deployment options for hello OMS virtual machine extension for Windows.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0c9bb-108">Krav</span><span class="sxs-lookup"><span data-stu-id="0c9bb-108">Prerequisites</span></span>

### <a name="operating-system"></a><span data-ttu-id="0c9bb-109">Operativsystem</span><span class="sxs-lookup"><span data-stu-id="0c9bb-109">Operating system</span></span>
<span data-ttu-id="0c9bb-110">hello OMS-Agent tillägget utgåvor för Windows kan köras mot Windows Server 2008 R2, 2012 och 2012 R2 2016.</span><span class="sxs-lookup"><span data-stu-id="0c9bb-110">hello OMS Agent extension for Windows can be run against Windows Server 2008 R2, 2012, 2012 R2, and 2016 releases.</span></span>

### <a name="internet-connectivity"></a><span data-ttu-id="0c9bb-111">Internetanslutning</span><span class="sxs-lookup"><span data-stu-id="0c9bb-111">Internet connectivity</span></span>
<span data-ttu-id="0c9bb-112">hello OMS-Agent tillägget för Windows kräver att hello mål den virtuella datorn är ansluten toohello internet.</span><span class="sxs-lookup"><span data-stu-id="0c9bb-112">hello OMS Agent extension for Windows requires that hello target virtual machine is connected toohello internet.</span></span> 

## <a name="extension-schema"></a><span data-ttu-id="0c9bb-113">Tilläggsschema</span><span class="sxs-lookup"><span data-stu-id="0c9bb-113">Extension schema</span></span>

<span data-ttu-id="0c9bb-114">hello visar följande JSON hello schemat för hello tillägget OMS-Agent.</span><span class="sxs-lookup"><span data-stu-id="0c9bb-114">hello following JSON shows hello schema for hello OMS Agent extension.</span></span> <span data-ttu-id="0c9bb-115">hello tillägget kräver hello arbetsytan Id och arbetsytenyckel från hello mål OMS-arbetsyta, finns dessa i hello OMS-portalen.</span><span class="sxs-lookup"><span data-stu-id="0c9bb-115">hello extension requires hello workspace Id and workspace key from hello target OMS workspace, these can be found in hello OMS portal.</span></span> <span data-ttu-id="0c9bb-116">Eftersom hello arbetsytenyckel ska behandlas som känsliga data, bör det lagras i en Inställningskonfiguration för skyddade.</span><span class="sxs-lookup"><span data-stu-id="0c9bb-116">Because hello workspace key should be treated as sensitive data, it should be stored in a protected setting configuration.</span></span> <span data-ttu-id="0c9bb-117">Azure VM tillägget skyddade inställningsdata krypteras och dekrypteras endast på hello virtuella måldatorn.</span><span class="sxs-lookup"><span data-stu-id="0c9bb-117">Azure VM extension protected setting data is encrypted, and only decrypted on hello target virtual machine.</span></span> <span data-ttu-id="0c9bb-118">Observera att **workspaceId** och **workspaceKey** är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="0c9bb-118">Note that **workspaceId** and **workspaceKey** are case-sensitive.</span></span>

```json
{
    "type": "extensions",
    "name": "OMSExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.EnterpriseCloud.Monitoring",
        "type": "MicrosoftMonitoringAgent",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "workspaceId": "myWorkSpaceId"
        },
        "protectedSettings": {
            "workspaceKey": "myWorkspaceKey"
        }
    }
}
```
### <a name="property-values"></a><span data-ttu-id="0c9bb-119">Egenskapsvärden</span><span class="sxs-lookup"><span data-stu-id="0c9bb-119">Property values</span></span>

| <span data-ttu-id="0c9bb-120">Namn</span><span class="sxs-lookup"><span data-stu-id="0c9bb-120">Name</span></span> | <span data-ttu-id="0c9bb-121">Värdet / exempel</span><span class="sxs-lookup"><span data-stu-id="0c9bb-121">Value / Example</span></span> |
| ---- | ---- |
| <span data-ttu-id="0c9bb-122">apiVersion</span><span class="sxs-lookup"><span data-stu-id="0c9bb-122">apiVersion</span></span> | <span data-ttu-id="0c9bb-123">2015-06-15</span><span class="sxs-lookup"><span data-stu-id="0c9bb-123">2015-06-15</span></span> |
| <span data-ttu-id="0c9bb-124">Publisher</span><span class="sxs-lookup"><span data-stu-id="0c9bb-124">publisher</span></span> | <span data-ttu-id="0c9bb-125">Microsoft.EnterpriseCloud.Monitoring</span><span class="sxs-lookup"><span data-stu-id="0c9bb-125">Microsoft.EnterpriseCloud.Monitoring</span></span> |
| <span data-ttu-id="0c9bb-126">typ</span><span class="sxs-lookup"><span data-stu-id="0c9bb-126">type</span></span> | <span data-ttu-id="0c9bb-127">MicrosoftMonitoringAgent</span><span class="sxs-lookup"><span data-stu-id="0c9bb-127">MicrosoftMonitoringAgent</span></span> |
| <span data-ttu-id="0c9bb-128">typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="0c9bb-128">typeHandlerVersion</span></span> | <span data-ttu-id="0c9bb-129">1.0</span><span class="sxs-lookup"><span data-stu-id="0c9bb-129">1.0</span></span> |
| <span data-ttu-id="0c9bb-130">workspaceId (t.ex.)</span><span class="sxs-lookup"><span data-stu-id="0c9bb-130">workspaceId (e.g)</span></span> | <span data-ttu-id="0c9bb-131">6f680a37-00c6-41C7-a93f-1437e3462574</span><span class="sxs-lookup"><span data-stu-id="0c9bb-131">6f680a37-00c6-41c7-a93f-1437e3462574</span></span> |
| <span data-ttu-id="0c9bb-132">workspaceKey (t.ex.)</span><span class="sxs-lookup"><span data-stu-id="0c9bb-132">workspaceKey (e.g)</span></span> | <span data-ttu-id="0c9bb-133">z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI + rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ ==</span><span class="sxs-lookup"><span data-stu-id="0c9bb-133">z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI+rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ==</span></span> |

## <a name="template-deployment"></a><span data-ttu-id="0c9bb-134">Malldistribution</span><span class="sxs-lookup"><span data-stu-id="0c9bb-134">Template deployment</span></span>

<span data-ttu-id="0c9bb-135">Azure VM-tillägg kan distribueras med Azure Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="0c9bb-135">Azure VM extensions can be deployed with Azure Resource Manager templates.</span></span> <span data-ttu-id="0c9bb-136">hello JSON-schema som beskrivs i föregående avsnitt i hello kan användas i en Azure Resource Manager mallen toorun hello tillägget OMS-Agent under en Azure Resource Manager för malldistribution.</span><span class="sxs-lookup"><span data-stu-id="0c9bb-136">hello JSON schema detailed in hello previous section can be used in an Azure Resource Manager template toorun hello OMS Agent extension during an Azure Resource Manager template deployment.</span></span> <span data-ttu-id="0c9bb-137">En exempelmall som innehåller hello OMS-agenten VM-tillägget kan hittas på hello [Azure Quick Start-galleriet](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-windows-vm).</span><span class="sxs-lookup"><span data-stu-id="0c9bb-137">A sample template that includes hello OMS Agent VM extension can be found on hello [Azure Quick Start Gallery](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-windows-vm).</span></span> 

<span data-ttu-id="0c9bb-138">hello JSON för ett tillägg för virtuell dator kan kapslas i hello virtuell datorresurs, eller placeras på hello rot- eller översta nivån i en Resource Manager JSON-mall.</span><span class="sxs-lookup"><span data-stu-id="0c9bb-138">hello JSON for a virtual machine extension can be nested inside hello virtual machine resource, or placed at hello root or top level of a Resource Manager JSON template.</span></span> <span data-ttu-id="0c9bb-139">hello placeringen av hello JSON påverkar hello värdet för hello resursnamnet och typen.</span><span class="sxs-lookup"><span data-stu-id="0c9bb-139">hello placement of hello JSON affects hello value of hello resource name and type.</span></span> <span data-ttu-id="0c9bb-140">Mer information finns i [ange namn och typ för underordnade resurser](../../azure-resource-manager/resource-manager-template-child-resource.md).</span><span class="sxs-lookup"><span data-stu-id="0c9bb-140">For more information, see [Set name and type for child resources](../../azure-resource-manager/resource-manager-template-child-resource.md).</span></span> 

<span data-ttu-id="0c9bb-141">hello förutsätter följande exempel hello OMS-tillägget är kapslad i hello virtuella datorresurser.</span><span class="sxs-lookup"><span data-stu-id="0c9bb-141">hello following example assumes hello OMS extension is nested inside hello virtual machine resource.</span></span> <span data-ttu-id="0c9bb-142">När för många kapslade hello tillägget resurs, hello JSON placeras i hello `"resources": []` objekt av hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="0c9bb-142">When nesting hello extension resource, hello JSON is placed in hello `"resources": []` object of hello virtual machine.</span></span>


```json
{
    "type": "extensions",
    "name": "OMSExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.EnterpriseCloud.Monitoring",
        "type": "MicrosoftMonitoringAgent",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "workspaceId": "myWorkSpaceId"
        },
        "protectedSettings": {
            "workspaceKey": "myWorkspaceKey"
        }
    }
}
```

<span data-ttu-id="0c9bb-143">När du monterar hello tillägget JSON hello roten i hello mallen hello resursnamnet innehåller en referens toohello överordnad virtuell dator och hello typen visar hello kapslade konfiguration.</span><span class="sxs-lookup"><span data-stu-id="0c9bb-143">When placing hello extension JSON at hello root of hello template, hello resource name includes a reference toohello parent virtual machine, and hello type reflects hello nested configuration.</span></span> 

```json
{
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "<parentVmResource>/OMSExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.EnterpriseCloud.Monitoring",
        "type": "MicrosoftMonitoringAgent",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "workspaceId": "myWorkSpaceId"
        },
        "protectedSettings": {
            "workspaceKey": "myWorkspaceKey"
        }
    }
}
```

## <a name="powershell-deployment"></a><span data-ttu-id="0c9bb-144">PowerShell-distribution</span><span class="sxs-lookup"><span data-stu-id="0c9bb-144">PowerShell deployment</span></span>

<span data-ttu-id="0c9bb-145">Hej `Set-AzureRmVMExtension` kommandot kan det vara används toodeploy hello OMS-Agent virtuella tillägget tooan befintlig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="0c9bb-145">hello `Set-AzureRmVMExtension` command can be used toodeploy hello OMS Agent virtual machine extension tooan existing virtual machine.</span></span> <span data-ttu-id="0c9bb-146">Innan du kör kommandot hello måste hello offentliga och privata konfigurationer toobe som lagras i en PowerShell-hash-tabell.</span><span class="sxs-lookup"><span data-stu-id="0c9bb-146">Before running hello command, hello public and private configurations need toobe stored in a PowerShell hash table.</span></span> 

```powershell
$PublicSettings = @{"workspaceId" = "myWorkspaceId"}
$ProtectedSettings = @{"workspaceKey" = "myWorkspaceKey"}

Set-AzureRmVMExtension -ExtensionName "Microsoft.EnterpriseCloud.Monitoring" `
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Publisher "Microsoft.EnterpriseCloud.Monitoring" `
    -ExtensionType "MicrosoftMonitoringAgent" `
    -TypeHandlerVersion 1.0 `
    -Settings $PublicSettings `
    -ProtectedSettings $ProtectedSettings `
    -Location WestUS ` 
```

## <a name="troubleshoot-and-support"></a><span data-ttu-id="0c9bb-147">Felsöka och stöd</span><span class="sxs-lookup"><span data-stu-id="0c9bb-147">Troubleshoot and support</span></span>

### <a name="troubleshoot"></a><span data-ttu-id="0c9bb-148">Felsöka</span><span class="sxs-lookup"><span data-stu-id="0c9bb-148">Troubleshoot</span></span>

<span data-ttu-id="0c9bb-149">Data om hello tillståndet för distributioner av tillägget kan hämtas från hello Azure-portalen och genom att använda hello Azure PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="0c9bb-149">Data about hello state of extension deployments can be retrieved from hello Azure portal, and by using hello Azure PowerShell module.</span></span> <span data-ttu-id="0c9bb-150">toosee hello distributionsstatusen för tillägg för en viss virtuell kör hello följande kommando använder hello Azure PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="0c9bb-150">toosee hello deployment state of extensions for a given VM, run hello following command using hello Azure PowerShell module.</span></span>

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

<span data-ttu-id="0c9bb-151">Tillägget körning utdata är loggade toofiles hittades i hello följande katalog:</span><span class="sxs-lookup"><span data-stu-id="0c9bb-151">Extension execution output is logged toofiles found in hello following directory:</span></span>

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\
```

### <a name="support"></a><span data-ttu-id="0c9bb-152">Support</span><span class="sxs-lookup"><span data-stu-id="0c9bb-152">Support</span></span>

<span data-ttu-id="0c9bb-153">Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta hello Azure experter på hello [MSDN Azure och Stack Overflow-forum](https://azure.microsoft.com/en-us/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="0c9bb-153">If you need more help at any point in this article, you can contact hello Azure experts on hello [MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/en-us/support/forums/).</span></span> <span data-ttu-id="0c9bb-154">Alternativt kan du lagra en incident i Azure-supporten.</span><span class="sxs-lookup"><span data-stu-id="0c9bb-154">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="0c9bb-155">Gå toohello [Azure supportwebbplats](https://azure.microsoft.com/en-us/support/options/) och välja Get support.</span><span class="sxs-lookup"><span data-stu-id="0c9bb-155">Go toohello [Azure support site](https://azure.microsoft.com/en-us/support/options/) and select Get support.</span></span> <span data-ttu-id="0c9bb-156">Information om hur du använder Azure stöder läsa hello [vanliga frågor om Microsoft Azure-supporten](https://azure.microsoft.com/en-us/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="0c9bb-156">For information about using Azure Support, read hello [Microsoft Azure support FAQ](https://azure.microsoft.com/en-us/support/faq/).</span></span>
