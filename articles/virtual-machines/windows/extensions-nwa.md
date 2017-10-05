---
title: "Azure Network Watcher Agent virtuella tillägget för Windows | Microsoft Docs"
description: "Distribuera Network Watcher Agent på Windows virtuell dator med ett tillägg för virtuell dator."
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
ms.openlocfilehash: b8d6a998bc86337b286a3434f44f762cca9b7e68
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="network-watcher-agent-virtual-machine-extension-for-windows"></a><span data-ttu-id="a58d6-103">Nätverk Watcher Agent tillägg för virtuell dator för Windows</span><span class="sxs-lookup"><span data-stu-id="a58d6-103">Network Watcher Agent virtual machine extension for Windows</span></span>

## <a name="overview"></a><span data-ttu-id="a58d6-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="a58d6-104">Overview</span></span>

<span data-ttu-id="a58d6-105">[Azure Nätverksbevakaren](https://review.docs.microsoft.com/en-us/azure/network-watcher/) är en prestanda övervakning, diagnostik och analytics nätverkstjänst som tillåter övervakning för Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="a58d6-105">[Azure Network Watcher](https://review.docs.microsoft.com/en-us/azure/network-watcher/) is a network performance monitoring, diagnostic, and analytics service that allows monitoring for Azure networks.</span></span> <span data-ttu-id="a58d6-106">Tillägget för virtuell dator Network Watcher Agent är ett krav för några av de Nätverksbevakaren funktionerna på virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="a58d6-106">The Network Watcher Agent virtual machine extension is a requirement for some of the Network Watcher features on Azure virtual machines.</span></span> <span data-ttu-id="a58d6-107">Detta omfattar att samla in nätverkstrafik på begäran och andra avancerade funktioner.</span><span class="sxs-lookup"><span data-stu-id="a58d6-107">This includes capturing network traffic on demand and other advanced functionality.</span></span>

<span data-ttu-id="a58d6-108">Det här dokumentet beskriver de plattformar som stöds och distributionsalternativ för tillägget för virtuell dator Network Watcher Agent för Windows.</span><span class="sxs-lookup"><span data-stu-id="a58d6-108">This document details the supported platforms and deployment options for the Network Watcher Agent virtual machine extension for Windows.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a58d6-109">Krav</span><span class="sxs-lookup"><span data-stu-id="a58d6-109">Prerequisites</span></span>

### <a name="operating-system"></a><span data-ttu-id="a58d6-110">Operativsystem</span><span class="sxs-lookup"><span data-stu-id="a58d6-110">Operating system</span></span>

<span data-ttu-id="a58d6-111">Network Watcher Agent-tillägget utgåvor för Windows kan köras mot Windows Server 2008 R2, 2012 och 2012 R2 2016.</span><span class="sxs-lookup"><span data-stu-id="a58d6-111">The Network Watcher Agent extension for Windows can be run against Windows Server 2008 R2, 2012, 2012 R2, and 2016 releases.</span></span> <span data-ttu-id="a58d6-112">Observera att Nano Server inte stöds just nu.</span><span class="sxs-lookup"><span data-stu-id="a58d6-112">Note that the Nano Server is not supported at this time.</span></span>

### <a name="internet-connectivity"></a><span data-ttu-id="a58d6-113">Internetanslutning</span><span class="sxs-lookup"><span data-stu-id="a58d6-113">Internet connectivity</span></span>

<span data-ttu-id="a58d6-114">Några av funktioner som Network Watcher Agent kräver att den virtuella måldatorn är ansluten till Internet.</span><span class="sxs-lookup"><span data-stu-id="a58d6-114">Some of the Network Watcher Agent functionality requires that the target virtual machine be connected to the Internet.</span></span> <span data-ttu-id="a58d6-115">Inte kan upprätta utgående anslutningar vissa Network Watcher Agent-funktioner kan fungera helt eller inte tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="a58d6-115">Without the ability to establish outgoing connections some of the Network Watcher Agent features may malfunction or become unavailable.</span></span> <span data-ttu-id="a58d6-116">Mer information finns i [Nätverksbevakaren dokumentationen](../../network-watcher/network-watcher-monitoring-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a58d6-116">For more details, please see the [Network Watcher documentation](../../network-watcher/network-watcher-monitoring-overview.md).</span></span>

## <a name="extension-schema"></a><span data-ttu-id="a58d6-117">Tilläggsschema</span><span class="sxs-lookup"><span data-stu-id="a58d6-117">Extension schema</span></span>

<span data-ttu-id="a58d6-118">Följande JSON visar schemat för tillägget Network Watcher Agent.</span><span class="sxs-lookup"><span data-stu-id="a58d6-118">The following JSON shows the schema for the Network Watcher Agent extension.</span></span> <span data-ttu-id="a58d6-119">Tillägget varken kräver inte heller stöder alla inställningar som anges av användaren just nu och förlitar sig på en standardkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="a58d6-119">The extension neither requires nor supports any user-supplied settings at this time and relies on its default configuration.</span></span>

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

### <a name="property-values"></a><span data-ttu-id="a58d6-120">Egenskapsvärden</span><span class="sxs-lookup"><span data-stu-id="a58d6-120">Property values</span></span>

| <span data-ttu-id="a58d6-121">Namn</span><span class="sxs-lookup"><span data-stu-id="a58d6-121">Name</span></span> | <span data-ttu-id="a58d6-122">Värdet / exempel</span><span class="sxs-lookup"><span data-stu-id="a58d6-122">Value / Example</span></span> |
| ---- | ---- |
| <span data-ttu-id="a58d6-123">apiVersion</span><span class="sxs-lookup"><span data-stu-id="a58d6-123">apiVersion</span></span> | <span data-ttu-id="a58d6-124">2015-06-15</span><span class="sxs-lookup"><span data-stu-id="a58d6-124">2015-06-15</span></span> |
| <span data-ttu-id="a58d6-125">Publisher</span><span class="sxs-lookup"><span data-stu-id="a58d6-125">publisher</span></span> | <span data-ttu-id="a58d6-126">Microsoft.Azure.NetworkWatcher</span><span class="sxs-lookup"><span data-stu-id="a58d6-126">Microsoft.Azure.NetworkWatcher</span></span> |
| <span data-ttu-id="a58d6-127">typ</span><span class="sxs-lookup"><span data-stu-id="a58d6-127">type</span></span> | <span data-ttu-id="a58d6-128">NetworkWatcherAgentWindows</span><span class="sxs-lookup"><span data-stu-id="a58d6-128">NetworkWatcherAgentWindows</span></span> |
| <span data-ttu-id="a58d6-129">typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="a58d6-129">typeHandlerVersion</span></span> | <span data-ttu-id="a58d6-130">1.4</span><span class="sxs-lookup"><span data-stu-id="a58d6-130">1.4</span></span> |


## <a name="template-deployment"></a><span data-ttu-id="a58d6-131">Malldistribution</span><span class="sxs-lookup"><span data-stu-id="a58d6-131">Template deployment</span></span>

<span data-ttu-id="a58d6-132">Azure VM-tillägg kan distribueras med Azure Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="a58d6-132">Azure VM extensions can be deployed with Azure Resource Manager templates.</span></span> <span data-ttu-id="a58d6-133">JSON-schema som beskrivs i föregående avsnitt kan användas i en Azure Resource Manager-mall för att köra tillägget Network Watcher Agent under en Azure Resource Manager för malldistribution.</span><span class="sxs-lookup"><span data-stu-id="a58d6-133">The JSON schema detailed in the previous section can be used in an Azure Resource Manager template to run the Network Watcher Agent extension during an Azure Resource Manager template deployment.</span></span>

## <a name="powershell-deployment"></a><span data-ttu-id="a58d6-134">PowerShell-distribution</span><span class="sxs-lookup"><span data-stu-id="a58d6-134">PowerShell deployment</span></span>

<span data-ttu-id="a58d6-135">Den `Set-AzureRmVMExtension` kommando kan användas för att distribuera Network Watcher Agent tillägget för virtuell dator till en befintlig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a58d6-135">The `Set-AzureRmVMExtension` command can be used to deploy the Network Watcher Agent virtual machine extension to an existing virtual machine.</span></span>

```powershell
Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup1" `
                       -Location "WestUS" `
                       -VMName "myVM1" `
                       -Name "networkWatcherAgent" `
                       -Publisher "Microsoft.Azure.NetworkWatcher" `
                       -Type "NetworkWatcherAgentWindows" `
                       -TypeHandlerVersion "1.4"
```

## <a name="troubleshooting-and-support"></a><span data-ttu-id="a58d6-136">Felsökning och support</span><span class="sxs-lookup"><span data-stu-id="a58d6-136">Troubleshooting and support</span></span>

### <a name="troubleshooting"></a><span data-ttu-id="a58d6-137">Felsökning</span><span class="sxs-lookup"><span data-stu-id="a58d6-137">Troubleshooting</span></span>

<span data-ttu-id="a58d6-138">Data om tillståndet för distributioner av tillägget kan hämtas från Azure-portalen och genom att använda Azure PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="a58d6-138">Data about the state of extension deployments can be retrieved from the Azure portal, and by using the Azure PowerShell module.</span></span> <span data-ttu-id="a58d6-139">Om du vill se distributionsstatusen för tillägg för en viss virtuell dator, kör du följande kommando med hjälp av Azure PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="a58d6-139">To see the deployment state of extensions for a given VM, run the following command using the Azure PowerShell module.</span></span>

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup1 -VMName myVM1 -Name networkWatcherAgent
```

<span data-ttu-id="a58d6-140">Tillägget utförande-utdatan loggas till filer som finns i följande katalog:</span><span class="sxs-lookup"><span data-stu-id="a58d6-140">Extension execution output is logged to files found in the following directory:</span></span>

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.NetworkWatcher.NetworkWatcherAgentWindows\
```

### <a name="support"></a><span data-ttu-id="a58d6-141">Support</span><span class="sxs-lookup"><span data-stu-id="a58d6-141">Support</span></span>

<span data-ttu-id="a58d6-142">Om du behöver mer hjälp när som helst i den här artikeln finns i användarhandboken för nätverket Watcher dokumentationen eller kontakta Azure-experter på den [MSDN Azure och Stack Overflow-forum](https://azure.microsoft.com/en-us/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="a58d6-142">If you need more help at any point in this article, you can refer to the Network Watcher User Guide documentation or contact the Azure experts on the [MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/en-us/support/forums/).</span></span> <span data-ttu-id="a58d6-143">Alternativt kan du lagra en incident i Azure-supporten.</span><span class="sxs-lookup"><span data-stu-id="a58d6-143">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="a58d6-144">Gå till den [Azure supportwebbplats](https://azure.microsoft.com/en-us/support/options/) och välja Get support.</span><span class="sxs-lookup"><span data-stu-id="a58d6-144">Go to the [Azure support site](https://azure.microsoft.com/en-us/support/options/) and select Get support.</span></span> <span data-ttu-id="a58d6-145">Information om hur du använder Azure stöder finns i [vanliga frågor om Microsoft Azure-supporten](https://azure.microsoft.com/en-us/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="a58d6-145">For information about using Azure Support, read the [Microsoft Azure support FAQ](https://azure.microsoft.com/en-us/support/faq/).</span></span>
