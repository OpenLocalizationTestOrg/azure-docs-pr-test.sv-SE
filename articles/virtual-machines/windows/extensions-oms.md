---
title: "OMS Azure tillägg för virtuell dator för Windows | Microsoft Docs"
description: "Distribuera OMS-agent på Windows virtuell dator med ett tillägg för virtuell dator."
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
ms.openlocfilehash: d933f488fdda0c1d37892be65f2712cf0eb5694e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="oms-virtual-machine-extension-for-windows"></a><span data-ttu-id="8d9ab-103">OMS tillägg för virtuell dator för Windows</span><span class="sxs-lookup"><span data-stu-id="8d9ab-103">OMS virtual machine extension for Windows</span></span>

<span data-ttu-id="8d9ab-104">Operations Management Suite (OMS) ger funktioner för övervakning, aviseringar, och reparationen i molnet och lokala resurser.</span><span class="sxs-lookup"><span data-stu-id="8d9ab-104">Operations Management Suite (OMS) provides monitoring, alerting, and alert remediation capabilities across cloud and on-premises assets.</span></span> <span data-ttu-id="8d9ab-105">Tillägget för virtuell dator OMS-Agent för Windows är publicerad och stöds av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="8d9ab-105">The OMS Agent virtual machine extension for Windows is published and supported by Microsoft.</span></span> <span data-ttu-id="8d9ab-106">Tillägget OMS-agent installeras på virtuella Azure-datorer och registrerar virtuella datorer i en befintlig OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="8d9ab-106">The extension installs the OMS agent on Azure virtual machines, and enrolls virtual machines into an existing OMS workspace.</span></span> <span data-ttu-id="8d9ab-107">Det här dokumentet beskriver de plattformar som stöds, konfigurationer och distributionsalternativ för OMS-tillägget för virtuell dator för Windows.</span><span class="sxs-lookup"><span data-stu-id="8d9ab-107">This document details the supported platforms, configurations, and deployment options for the OMS virtual machine extension for Windows.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8d9ab-108">Krav</span><span class="sxs-lookup"><span data-stu-id="8d9ab-108">Prerequisites</span></span>

### <a name="operating-system"></a><span data-ttu-id="8d9ab-109">Operativsystem</span><span class="sxs-lookup"><span data-stu-id="8d9ab-109">Operating system</span></span>
<span data-ttu-id="8d9ab-110">Tillägget OMS-Agent utgåvor för Windows kan köras mot Windows Server 2008 R2, 2012 och 2012 R2 2016.</span><span class="sxs-lookup"><span data-stu-id="8d9ab-110">The OMS Agent extension for Windows can be run against Windows Server 2008 R2, 2012, 2012 R2, and 2016 releases.</span></span>

### <a name="internet-connectivity"></a><span data-ttu-id="8d9ab-111">Internetanslutning</span><span class="sxs-lookup"><span data-stu-id="8d9ab-111">Internet connectivity</span></span>
<span data-ttu-id="8d9ab-112">OMS-Agent-tillägget för Windows kräver att den virtuella måldatorn är ansluten till internet.</span><span class="sxs-lookup"><span data-stu-id="8d9ab-112">The OMS Agent extension for Windows requires that the target virtual machine is connected to the internet.</span></span> 

## <a name="extension-schema"></a><span data-ttu-id="8d9ab-113">Tilläggsschema</span><span class="sxs-lookup"><span data-stu-id="8d9ab-113">Extension schema</span></span>

<span data-ttu-id="8d9ab-114">Följande JSON visar schemat för tillägget OMS-Agent.</span><span class="sxs-lookup"><span data-stu-id="8d9ab-114">The following JSON shows the schema for the OMS Agent extension.</span></span> <span data-ttu-id="8d9ab-115">Tillägget kräver arbetsytans Id och arbetsytenyckel från OMS målarbetsytan, finns dessa i OMS-portalen.</span><span class="sxs-lookup"><span data-stu-id="8d9ab-115">The extension requires the workspace Id and workspace key from the target OMS workspace, these can be found in the OMS portal.</span></span> <span data-ttu-id="8d9ab-116">Eftersom arbetsytans ska behandlas som känsliga data, bör det lagras i en Inställningskonfiguration för skyddade.</span><span class="sxs-lookup"><span data-stu-id="8d9ab-116">Because the workspace key should be treated as sensitive data, it should be stored in a protected setting configuration.</span></span> <span data-ttu-id="8d9ab-117">Azure för VM-tillägget skyddade inställningsdata krypteras och dekrypteras endast på den virtuella måldatorn.</span><span class="sxs-lookup"><span data-stu-id="8d9ab-117">Azure VM extension protected setting data is encrypted, and only decrypted on the target virtual machine.</span></span> <span data-ttu-id="8d9ab-118">Observera att **workspaceId** och **workspaceKey** är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="8d9ab-118">Note that **workspaceId** and **workspaceKey** are case-sensitive.</span></span>

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
### <a name="property-values"></a><span data-ttu-id="8d9ab-119">Egenskapsvärden</span><span class="sxs-lookup"><span data-stu-id="8d9ab-119">Property values</span></span>

| <span data-ttu-id="8d9ab-120">Namn</span><span class="sxs-lookup"><span data-stu-id="8d9ab-120">Name</span></span> | <span data-ttu-id="8d9ab-121">Värdet / exempel</span><span class="sxs-lookup"><span data-stu-id="8d9ab-121">Value / Example</span></span> |
| ---- | ---- |
| <span data-ttu-id="8d9ab-122">apiVersion</span><span class="sxs-lookup"><span data-stu-id="8d9ab-122">apiVersion</span></span> | <span data-ttu-id="8d9ab-123">2015-06-15</span><span class="sxs-lookup"><span data-stu-id="8d9ab-123">2015-06-15</span></span> |
| <span data-ttu-id="8d9ab-124">Publisher</span><span class="sxs-lookup"><span data-stu-id="8d9ab-124">publisher</span></span> | <span data-ttu-id="8d9ab-125">Microsoft.EnterpriseCloud.Monitoring</span><span class="sxs-lookup"><span data-stu-id="8d9ab-125">Microsoft.EnterpriseCloud.Monitoring</span></span> |
| <span data-ttu-id="8d9ab-126">typ</span><span class="sxs-lookup"><span data-stu-id="8d9ab-126">type</span></span> | <span data-ttu-id="8d9ab-127">MicrosoftMonitoringAgent</span><span class="sxs-lookup"><span data-stu-id="8d9ab-127">MicrosoftMonitoringAgent</span></span> |
| <span data-ttu-id="8d9ab-128">typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="8d9ab-128">typeHandlerVersion</span></span> | <span data-ttu-id="8d9ab-129">1.0</span><span class="sxs-lookup"><span data-stu-id="8d9ab-129">1.0</span></span> |
| <span data-ttu-id="8d9ab-130">workspaceId (t.ex.)</span><span class="sxs-lookup"><span data-stu-id="8d9ab-130">workspaceId (e.g)</span></span> | <span data-ttu-id="8d9ab-131">6f680a37-00c6-41C7-a93f-1437e3462574</span><span class="sxs-lookup"><span data-stu-id="8d9ab-131">6f680a37-00c6-41c7-a93f-1437e3462574</span></span> |
| <span data-ttu-id="8d9ab-132">workspaceKey (t.ex.)</span><span class="sxs-lookup"><span data-stu-id="8d9ab-132">workspaceKey (e.g)</span></span> | <span data-ttu-id="8d9ab-133">z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI + rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ ==</span><span class="sxs-lookup"><span data-stu-id="8d9ab-133">z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI+rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ==</span></span> |

## <a name="template-deployment"></a><span data-ttu-id="8d9ab-134">Malldistribution</span><span class="sxs-lookup"><span data-stu-id="8d9ab-134">Template deployment</span></span>

<span data-ttu-id="8d9ab-135">Azure VM-tillägg kan distribueras med Azure Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="8d9ab-135">Azure VM extensions can be deployed with Azure Resource Manager templates.</span></span> <span data-ttu-id="8d9ab-136">JSON-schema som beskrivs i föregående avsnitt kan användas i en Azure Resource Manager-mall för att köra tillägget OMS-Agent under en Azure Resource Manager för malldistribution.</span><span class="sxs-lookup"><span data-stu-id="8d9ab-136">The JSON schema detailed in the previous section can be used in an Azure Resource Manager template to run the OMS Agent extension during an Azure Resource Manager template deployment.</span></span> <span data-ttu-id="8d9ab-137">En exempelmall som innehåller OMS-agenten VM-tillägget kan hittas på den [Azure Quick Start-galleriet](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-windows-vm).</span><span class="sxs-lookup"><span data-stu-id="8d9ab-137">A sample template that includes the OMS Agent VM extension can be found on the [Azure Quick Start Gallery](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-windows-vm).</span></span> 

<span data-ttu-id="8d9ab-138">JSON för ett tillägg för virtuell dator kan kapslas i den virtuella datorresursen eller placeras i roten eller översta nivån i en Resource Manager JSON-mall.</span><span class="sxs-lookup"><span data-stu-id="8d9ab-138">The JSON for a virtual machine extension can be nested inside the virtual machine resource, or placed at the root or top level of a Resource Manager JSON template.</span></span> <span data-ttu-id="8d9ab-139">Placeringen av JSON påverkar värdet av resursens namn och typen.</span><span class="sxs-lookup"><span data-stu-id="8d9ab-139">The placement of the JSON affects the value of the resource name and type.</span></span> <span data-ttu-id="8d9ab-140">Mer information finns i [ange namn och typ för underordnade resurser](../../azure-resource-manager/resource-manager-template-child-resource.md).</span><span class="sxs-lookup"><span data-stu-id="8d9ab-140">For more information, see [Set name and type for child resources](../../azure-resource-manager/resource-manager-template-child-resource.md).</span></span> 

<span data-ttu-id="8d9ab-141">I följande exempel förutsätter OMS-tillägget är kapslad i den virtuella datorresursen.</span><span class="sxs-lookup"><span data-stu-id="8d9ab-141">The following example assumes the OMS extension is nested inside the virtual machine resource.</span></span> <span data-ttu-id="8d9ab-142">När kapsla resursen tillägget JSON placeras i den `"resources": []` objekt av den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="8d9ab-142">When nesting the extension resource, the JSON is placed in the `"resources": []` object of the virtual machine.</span></span>


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

<span data-ttu-id="8d9ab-143">När du monterar tillägget JSON i roten på mallen resursnamnet innehåller en referens till den överordnade virtuella datorn och typen visar kapslade konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="8d9ab-143">When placing the extension JSON at the root of the template, the resource name includes a reference to the parent virtual machine, and the type reflects the nested configuration.</span></span> 

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

## <a name="powershell-deployment"></a><span data-ttu-id="8d9ab-144">PowerShell-distribution</span><span class="sxs-lookup"><span data-stu-id="8d9ab-144">PowerShell deployment</span></span>

<span data-ttu-id="8d9ab-145">Den `Set-AzureRmVMExtension` kommando kan användas för att distribuera OMS-Agent tillägget för virtuell dator till en befintlig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="8d9ab-145">The `Set-AzureRmVMExtension` command can be used to deploy the OMS Agent virtual machine extension to an existing virtual machine.</span></span> <span data-ttu-id="8d9ab-146">Innan du kör kommandot måste de offentliga och privata konfigurationerna lagras i en PowerShell-hash-tabell.</span><span class="sxs-lookup"><span data-stu-id="8d9ab-146">Before running the command, the public and private configurations need to be stored in a PowerShell hash table.</span></span> 

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

## <a name="troubleshoot-and-support"></a><span data-ttu-id="8d9ab-147">Felsöka och stöd</span><span class="sxs-lookup"><span data-stu-id="8d9ab-147">Troubleshoot and support</span></span>

### <a name="troubleshoot"></a><span data-ttu-id="8d9ab-148">Felsöka</span><span class="sxs-lookup"><span data-stu-id="8d9ab-148">Troubleshoot</span></span>

<span data-ttu-id="8d9ab-149">Data om tillståndet för distributioner av tillägget kan hämtas från Azure-portalen och genom att använda Azure PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="8d9ab-149">Data about the state of extension deployments can be retrieved from the Azure portal, and by using the Azure PowerShell module.</span></span> <span data-ttu-id="8d9ab-150">Om du vill se distributionsstatusen för tillägg för en viss virtuell dator, kör du följande kommando med hjälp av Azure PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="8d9ab-150">To see the deployment state of extensions for a given VM, run the following command using the Azure PowerShell module.</span></span>

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

<span data-ttu-id="8d9ab-151">Tillägget utförande-utdatan loggas till filer som finns i följande katalog:</span><span class="sxs-lookup"><span data-stu-id="8d9ab-151">Extension execution output is logged to files found in the following directory:</span></span>

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\
```

### <a name="support"></a><span data-ttu-id="8d9ab-152">Support</span><span class="sxs-lookup"><span data-stu-id="8d9ab-152">Support</span></span>

<span data-ttu-id="8d9ab-153">Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta Azure-experter på den [MSDN Azure och Stack Overflow-forum](https://azure.microsoft.com/en-us/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="8d9ab-153">If you need more help at any point in this article, you can contact the Azure experts on the [MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/en-us/support/forums/).</span></span> <span data-ttu-id="8d9ab-154">Alternativt kan du lagra en incident i Azure-supporten.</span><span class="sxs-lookup"><span data-stu-id="8d9ab-154">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="8d9ab-155">Gå till den [Azure supportwebbplats](https://azure.microsoft.com/en-us/support/options/) och välja Get support.</span><span class="sxs-lookup"><span data-stu-id="8d9ab-155">Go to the [Azure support site](https://azure.microsoft.com/en-us/support/options/) and select Get support.</span></span> <span data-ttu-id="8d9ab-156">Information om hur du använder Azure stöder finns i [vanliga frågor om Microsoft Azure-supporten](https://azure.microsoft.com/en-us/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="8d9ab-156">For information about using Azure Support, read the [Microsoft Azure support FAQ](https://azure.microsoft.com/en-us/support/faq/).</span></span>
