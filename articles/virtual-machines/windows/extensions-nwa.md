---
title: "aaaAzure Network Watcher Agent tillägg för virtuell dator för Windows | Microsoft Docs"
description: "Distribuera hello Network Watcher Agent på Windows virtuell dator med ett tillägg för virtuell dator."
services: virtual-machines-windows
documentationcenter: 
author: dennisg
manager: amku
editor: 
tags: azure-resource-manager
ms.assetid: 27e46af7-2150-45e8-b084-ba33de8c5e3f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 02/14/2017
ms.author: dennisg
ms.openlocfilehash: 21298706e462ff32c4d314f9a1ad127074ddf481
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="network-watcher-agent-virtual-machine-extension-for-windows"></a><span data-ttu-id="6a237-103">Nätverk Watcher Agent tillägg för virtuell dator för Windows</span><span class="sxs-lookup"><span data-stu-id="6a237-103">Network Watcher Agent virtual machine extension for Windows</span></span>

## <a name="overview"></a><span data-ttu-id="6a237-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="6a237-104">Overview</span></span>

<span data-ttu-id="6a237-105">[Azure Nätverksbevakaren](https://review.docs.microsoft.com/en-us/azure/network-watcher/) är en prestanda övervakning, diagnostik och analytics nätverkstjänst som tillåter övervakning för Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="6a237-105">[Azure Network Watcher](https://review.docs.microsoft.com/en-us/azure/network-watcher/) is a network performance monitoring, diagnostic, and analytics service that allows monitoring for Azure networks.</span></span> <span data-ttu-id="6a237-106">hello Network Watcher Agent tillägg för virtuell dator är ett krav för några av hello Nätverksbevakaren funktioner på virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="6a237-106">hello Network Watcher Agent virtual machine extension is a requirement for some of hello Network Watcher features on Azure virtual machines.</span></span> <span data-ttu-id="6a237-107">Detta omfattar att samla in nätverkstrafik på begäran och andra avancerade funktioner.</span><span class="sxs-lookup"><span data-stu-id="6a237-107">This includes capturing network traffic on demand and other advanced functionality.</span></span>

<span data-ttu-id="6a237-108">Det här dokumentet information hello stöds plattformar och distributionsalternativ för hello Network Watcher Agent tillägg för virtuell dator för Windows.</span><span class="sxs-lookup"><span data-stu-id="6a237-108">This document details hello supported platforms and deployment options for hello Network Watcher Agent virtual machine extension for Windows.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6a237-109">Krav</span><span class="sxs-lookup"><span data-stu-id="6a237-109">Prerequisites</span></span>

### <a name="operating-system"></a><span data-ttu-id="6a237-110">Operativsystem</span><span class="sxs-lookup"><span data-stu-id="6a237-110">Operating system</span></span>

<span data-ttu-id="6a237-111">hello Network Watcher Agent tillägget utgåvor för Windows kan köras mot Windows Server 2008 R2, 2012 och 2012 R2 2016.</span><span class="sxs-lookup"><span data-stu-id="6a237-111">hello Network Watcher Agent extension for Windows can be run against Windows Server 2008 R2, 2012, 2012 R2, and 2016 releases.</span></span> <span data-ttu-id="6a237-112">Observera att hello Nano Server stöds inte just nu.</span><span class="sxs-lookup"><span data-stu-id="6a237-112">Note that hello Nano Server is not supported at this time.</span></span>

### <a name="internet-connectivity"></a><span data-ttu-id="6a237-113">Internetanslutning</span><span class="sxs-lookup"><span data-stu-id="6a237-113">Internet connectivity</span></span>

<span data-ttu-id="6a237-114">Vissa av hello Network Watcher Agent funktioner kräver att den virtuella måldatorn hello anslutna toohello Internet.</span><span class="sxs-lookup"><span data-stu-id="6a237-114">Some of hello Network Watcher Agent functionality requires that hello target virtual machine be connected toohello Internet.</span></span> <span data-ttu-id="6a237-115">Utan hello möjlighet tooestablish utgående anslutningar några av funktionerna för hello Network Watcher Agent kan fungera helt eller inte tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="6a237-115">Without hello ability tooestablish outgoing connections some of hello Network Watcher Agent features may malfunction or become unavailable.</span></span> <span data-ttu-id="6a237-116">Mer information finns i hello [Nätverksbevakaren dokumentationen](../../network-watcher/network-watcher-monitoring-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6a237-116">For more details, please see hello [Network Watcher documentation](../../network-watcher/network-watcher-monitoring-overview.md).</span></span>

## <a name="extension-schema"></a><span data-ttu-id="6a237-117">Tilläggsschema</span><span class="sxs-lookup"><span data-stu-id="6a237-117">Extension schema</span></span>

<span data-ttu-id="6a237-118">hello visar följande JSON hello schemat för hello Network Watcher Agent tillägg.</span><span class="sxs-lookup"><span data-stu-id="6a237-118">hello following JSON shows hello schema for hello Network Watcher Agent extension.</span></span> <span data-ttu-id="6a237-119">hello tillägget varken kräver inte heller stöder alla inställningar som anges av användaren just nu och förlitar sig på en standardkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="6a237-119">hello extension neither requires nor supports any user-supplied settings at this time and relies on its default configuration.</span></span>

```json
{
    "type": "extensions",
    "name": "Microsoft.Azure.NetworkWatcher",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Azure.NetworkWatcher",
        "type": "NetworkWatcherAgentWindows",
        "typeHandlerVersion": "1.4",
        "autoUpgradeMinorVersion": true
    }
}
```

### <a name="property-values"></a><span data-ttu-id="6a237-120">Egenskapsvärden</span><span class="sxs-lookup"><span data-stu-id="6a237-120">Property values</span></span>

| <span data-ttu-id="6a237-121">Namn</span><span class="sxs-lookup"><span data-stu-id="6a237-121">Name</span></span> | <span data-ttu-id="6a237-122">Värdet / exempel</span><span class="sxs-lookup"><span data-stu-id="6a237-122">Value / Example</span></span> |
| ---- | ---- |
| <span data-ttu-id="6a237-123">apiVersion</span><span class="sxs-lookup"><span data-stu-id="6a237-123">apiVersion</span></span> | <span data-ttu-id="6a237-124">2015-06-15</span><span class="sxs-lookup"><span data-stu-id="6a237-124">2015-06-15</span></span> |
| <span data-ttu-id="6a237-125">Publisher</span><span class="sxs-lookup"><span data-stu-id="6a237-125">publisher</span></span> | <span data-ttu-id="6a237-126">Microsoft.Azure.NetworkWatcher</span><span class="sxs-lookup"><span data-stu-id="6a237-126">Microsoft.Azure.NetworkWatcher</span></span> |
| <span data-ttu-id="6a237-127">typ</span><span class="sxs-lookup"><span data-stu-id="6a237-127">type</span></span> | <span data-ttu-id="6a237-128">NetworkWatcherAgentWindows</span><span class="sxs-lookup"><span data-stu-id="6a237-128">NetworkWatcherAgentWindows</span></span> |
| <span data-ttu-id="6a237-129">typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="6a237-129">typeHandlerVersion</span></span> | <span data-ttu-id="6a237-130">1.4</span><span class="sxs-lookup"><span data-stu-id="6a237-130">1.4</span></span> |


## <a name="template-deployment"></a><span data-ttu-id="6a237-131">Malldistribution</span><span class="sxs-lookup"><span data-stu-id="6a237-131">Template deployment</span></span>

<span data-ttu-id="6a237-132">Azure VM-tillägg kan distribueras med Azure Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="6a237-132">Azure VM extensions can be deployed with Azure Resource Manager templates.</span></span> <span data-ttu-id="6a237-133">hello JSON-schema som beskrivs i föregående avsnitt i hello kan användas i en Azure Resource Manager mallen toorun hello Network Watcher Agent tillägg under en Azure Resource Manager för malldistribution.</span><span class="sxs-lookup"><span data-stu-id="6a237-133">hello JSON schema detailed in hello previous section can be used in an Azure Resource Manager template toorun hello Network Watcher Agent extension during an Azure Resource Manager template deployment.</span></span>

## <a name="powershell-deployment"></a><span data-ttu-id="6a237-134">PowerShell-distribution</span><span class="sxs-lookup"><span data-stu-id="6a237-134">PowerShell deployment</span></span>

<span data-ttu-id="6a237-135">Hej `Set-AzureRmVMExtension` kommandot kan det vara används toodeploy hello Network Watcher Agent virtuella tillägget tooan befintlig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="6a237-135">hello `Set-AzureRmVMExtension` command can be used toodeploy hello Network Watcher Agent virtual machine extension tooan existing virtual machine.</span></span>

```powershell
Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup1" `
                       -Location "WestUS" `
                       -VMName "myVM1" `
                       -Name "networkWatcherAgent" `
                       -Publisher "Microsoft.Azure.NetworkWatcher" `
                       -Type "NetworkWatcherAgentWindows" `
                       -TypeHandlerVersion "1.4"
```

## <a name="troubleshooting-and-support"></a><span data-ttu-id="6a237-136">Felsökning och support</span><span class="sxs-lookup"><span data-stu-id="6a237-136">Troubleshooting and support</span></span>

### <a name="troubleshooting"></a><span data-ttu-id="6a237-137">Felsökning</span><span class="sxs-lookup"><span data-stu-id="6a237-137">Troubleshooting</span></span>

<span data-ttu-id="6a237-138">Data om hello tillståndet för distributioner av tillägget kan hämtas från hello Azure-portalen och genom att använda hello Azure PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="6a237-138">Data about hello state of extension deployments can be retrieved from hello Azure portal, and by using hello Azure PowerShell module.</span></span> <span data-ttu-id="6a237-139">toosee hello distributionsstatusen för tillägg för en viss virtuell kör hello följande kommando använder hello Azure PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="6a237-139">toosee hello deployment state of extensions for a given VM, run hello following command using hello Azure PowerShell module.</span></span>

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup1 -VMName myVM1 -Name networkWatcherAgent
```

<span data-ttu-id="6a237-140">Tillägget körning utdata är loggade toofiles hittades i hello följande katalog:</span><span class="sxs-lookup"><span data-stu-id="6a237-140">Extension execution output is logged toofiles found in hello following directory:</span></span>

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.NetworkWatcher.NetworkWatcherAgentWindows\
```

### <a name="support"></a><span data-ttu-id="6a237-141">Support</span><span class="sxs-lookup"><span data-stu-id="6a237-141">Support</span></span>

<span data-ttu-id="6a237-142">Om du behöver mer hjälp när som helst i den här artikeln kan du referera toohello användarhandboken för nätverket Watcher dokumentationen eller kontakta hello Azure experter på hello [MSDN Azure och Stack Overflow-forum](https://azure.microsoft.com/en-us/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="6a237-142">If you need more help at any point in this article, you can refer toohello Network Watcher User Guide documentation or contact hello Azure experts on hello [MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/en-us/support/forums/).</span></span> <span data-ttu-id="6a237-143">Alternativt kan du lagra en incident i Azure-supporten.</span><span class="sxs-lookup"><span data-stu-id="6a237-143">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="6a237-144">Gå toohello [Azure supportwebbplats](https://azure.microsoft.com/en-us/support/options/) och välja Get support.</span><span class="sxs-lookup"><span data-stu-id="6a237-144">Go toohello [Azure support site](https://azure.microsoft.com/en-us/support/options/) and select Get support.</span></span> <span data-ttu-id="6a237-145">Information om hur du använder Azure stöder läsa hello [vanliga frågor om Microsoft Azure-supporten](https://azure.microsoft.com/en-us/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="6a237-145">For information about using Azure Support, read hello [Microsoft Azure support FAQ](https://azure.microsoft.com/en-us/support/faq/).</span></span>
