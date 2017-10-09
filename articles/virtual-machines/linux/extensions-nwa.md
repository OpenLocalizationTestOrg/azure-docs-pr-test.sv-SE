---
title: "aaaAzure Network Watcher Agent tillägg för virtuell dator för Linux | Microsoft Docs"
description: "Distribuera hello Network Watcher Agent på Linux-dator som använder ett tillägg för virtuell dator."
services: virtual-machines-linux
documentationcenter: 
author: dennisg
manager: amku
editor: 
tags: azure-resource-manager
ms.assetid: 5c81e94c-e127-4dd2-ae83-a236c4512345
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/14/2017
ms.author: dennisg
ms.openlocfilehash: 84bed132cbda83d0917be490f9a50914578952a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="network-watcher-agent-virtual-machine-extension-for-linux"></a><span data-ttu-id="798a9-103">Nätverk Watcher Agent tillägg för virtuell dator för Linux</span><span class="sxs-lookup"><span data-stu-id="798a9-103">Network Watcher Agent virtual machine extension for Linux</span></span>

## <a name="overview"></a><span data-ttu-id="798a9-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="798a9-104">Overview</span></span>

<span data-ttu-id="798a9-105">[Azure Nätverksbevakaren](https://review.docs.microsoft.com/en-us/azure/network-watcher/) är en prestanda övervakning, diagnostik och analytics nätverkstjänst som tillåter övervakning för Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="798a9-105">[Azure Network Watcher](https://review.docs.microsoft.com/en-us/azure/network-watcher/) is a network performance monitoring, diagnostic, and analytics service that allows monitoring for Azure networks.</span></span> <span data-ttu-id="798a9-106">hello Network Watcher Agent tillägg för virtuell dator är ett krav för några av hello Nätverksbevakaren funktioner på virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="798a9-106">hello Network Watcher Agent virtual machine extension is a requirement for some of hello Network Watcher features on Azure virtual machines.</span></span> <span data-ttu-id="798a9-107">Detta omfattar att samla in nätverkstrafik på begäran och andra avancerade funktioner.</span><span class="sxs-lookup"><span data-stu-id="798a9-107">This includes capturing network traffic on demand and other advanced functionality.</span></span>

<span data-ttu-id="798a9-108">Det här dokumentet information hello stöds plattformar och distributionsalternativ för hello tillägg för virtuell dator i nätverket Watcher Agent för Linux.</span><span class="sxs-lookup"><span data-stu-id="798a9-108">This document details hello supported platforms and deployment options for hello Network Watcher Agent virtual machine extension for Linux.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="798a9-109">Krav</span><span class="sxs-lookup"><span data-stu-id="798a9-109">Prerequisites</span></span>

### <a name="operating-system"></a><span data-ttu-id="798a9-110">Operativsystem</span><span class="sxs-lookup"><span data-stu-id="798a9-110">Operating system</span></span>

<span data-ttu-id="798a9-111">hello Network Watcher Agent tillägget kan köras mot dessa Linux-distributioner:</span><span class="sxs-lookup"><span data-stu-id="798a9-111">hello Network Watcher Agent extension can be run against these Linux distributions:</span></span>

| <span data-ttu-id="798a9-112">Distribution</span><span class="sxs-lookup"><span data-stu-id="798a9-112">Distribution</span></span> | <span data-ttu-id="798a9-113">Version</span><span class="sxs-lookup"><span data-stu-id="798a9-113">Version</span></span> |
|---|---|
| <span data-ttu-id="798a9-114">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="798a9-114">Ubuntu</span></span> | <span data-ttu-id="798a9-115">16.04 LTS, 14.04 LTS och 12.04 LTS</span><span class="sxs-lookup"><span data-stu-id="798a9-115">16.04 LTS, 14.04 LTS and 12.04 LTS</span></span> |
| <span data-ttu-id="798a9-116">Debian</span><span class="sxs-lookup"><span data-stu-id="798a9-116">Debian</span></span> | <span data-ttu-id="798a9-117">7 och 8</span><span class="sxs-lookup"><span data-stu-id="798a9-117">7 and 8</span></span> |
| <span data-ttu-id="798a9-118">Redhat</span><span class="sxs-lookup"><span data-stu-id="798a9-118">RedHat</span></span> | <span data-ttu-id="798a9-119">6.x och 7.x</span><span class="sxs-lookup"><span data-stu-id="798a9-119">6.x and 7.x</span></span> |
| <span data-ttu-id="798a9-120">Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="798a9-120">Oracle Linux</span></span> | <span data-ttu-id="798a9-121">7 x</span><span class="sxs-lookup"><span data-stu-id="798a9-121">7x</span></span> |
| <span data-ttu-id="798a9-122">SUSE</span><span class="sxs-lookup"><span data-stu-id="798a9-122">Suse</span></span> | <span data-ttu-id="798a9-123">11 och 12</span><span class="sxs-lookup"><span data-stu-id="798a9-123">11 and 12</span></span> |
| <span data-ttu-id="798a9-124">OpenSuse</span><span class="sxs-lookup"><span data-stu-id="798a9-124">OpenSuse</span></span> | <span data-ttu-id="798a9-125">7.0</span><span class="sxs-lookup"><span data-stu-id="798a9-125">7.0</span></span> |
| <span data-ttu-id="798a9-126">CentOS</span><span class="sxs-lookup"><span data-stu-id="798a9-126">CentOS</span></span> | <span data-ttu-id="798a9-127">7.0</span><span class="sxs-lookup"><span data-stu-id="798a9-127">7.0</span></span> |

<span data-ttu-id="798a9-128">Observera att virtuell CoreOS inte stöds just nu.</span><span class="sxs-lookup"><span data-stu-id="798a9-128">Note that CoreOS is not supported at this time.</span></span>

### <a name="internet-connectivity"></a><span data-ttu-id="798a9-129">Internetanslutning</span><span class="sxs-lookup"><span data-stu-id="798a9-129">Internet connectivity</span></span>

<span data-ttu-id="798a9-130">Vissa av hello Network Watcher Agent funktioner kräver att den virtuella måldatorn hello anslutna toohello Internet.</span><span class="sxs-lookup"><span data-stu-id="798a9-130">Some of hello Network Watcher Agent functionality requires that hello target virtual machine be connected toohello Internet.</span></span> <span data-ttu-id="798a9-131">Utan hello möjlighet tooestablish utgående anslutningar några av funktionerna för hello Network Watcher Agent kan fungera helt eller inte tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="798a9-131">Without hello ability tooestablish outgoing connections some of hello Network Watcher Agent features may malfunction or become unavailable.</span></span> <span data-ttu-id="798a9-132">Mer information finns i hello [Nätverksbevakaren dokumentationen](https://review.docs.microsoft.com/en-us/azure/network-watcher/).</span><span class="sxs-lookup"><span data-stu-id="798a9-132">For more details, please see hello [Network Watcher documentation](https://review.docs.microsoft.com/en-us/azure/network-watcher/).</span></span>

## <a name="extension-schema"></a><span data-ttu-id="798a9-133">Tilläggsschema</span><span class="sxs-lookup"><span data-stu-id="798a9-133">Extension schema</span></span>

<span data-ttu-id="798a9-134">hello visar följande JSON hello schemat för hello Network Watcher Agent tillägg.</span><span class="sxs-lookup"><span data-stu-id="798a9-134">hello following JSON shows hello schema for hello Network Watcher Agent extension.</span></span> <span data-ttu-id="798a9-135">hello tillägget varken kräver inte heller stöder alla inställningar som anges av användaren just nu och förlitar sig på en standardkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="798a9-135">hello extension neither requires nor supports any user-supplied settings at this time and relies on its default configuration.</span></span>

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
        "type": "NetworkWatcherAgentLinux",
        "typeHandlerVersion": "1.4",
        "autoUpgradeMinorVersion": true
    }
}
```

### <a name="property-values"></a><span data-ttu-id="798a9-136">Egenskapsvärden</span><span class="sxs-lookup"><span data-stu-id="798a9-136">Property values</span></span>

| <span data-ttu-id="798a9-137">Namn</span><span class="sxs-lookup"><span data-stu-id="798a9-137">Name</span></span> | <span data-ttu-id="798a9-138">Värdet / exempel</span><span class="sxs-lookup"><span data-stu-id="798a9-138">Value / Example</span></span> |
| ---- | ---- |
| <span data-ttu-id="798a9-139">apiVersion</span><span class="sxs-lookup"><span data-stu-id="798a9-139">apiVersion</span></span> | <span data-ttu-id="798a9-140">2015-06-15</span><span class="sxs-lookup"><span data-stu-id="798a9-140">2015-06-15</span></span> |
| <span data-ttu-id="798a9-141">Publisher</span><span class="sxs-lookup"><span data-stu-id="798a9-141">publisher</span></span> | <span data-ttu-id="798a9-142">Microsoft.Azure.NetworkWatcher</span><span class="sxs-lookup"><span data-stu-id="798a9-142">Microsoft.Azure.NetworkWatcher</span></span> |
| <span data-ttu-id="798a9-143">typ</span><span class="sxs-lookup"><span data-stu-id="798a9-143">type</span></span> | <span data-ttu-id="798a9-144">NetworkWatcherAgentLinux</span><span class="sxs-lookup"><span data-stu-id="798a9-144">NetworkWatcherAgentLinux</span></span> |
| <span data-ttu-id="798a9-145">typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="798a9-145">typeHandlerVersion</span></span> | <span data-ttu-id="798a9-146">1.4</span><span class="sxs-lookup"><span data-stu-id="798a9-146">1.4</span></span> |

## <a name="template-deployment"></a><span data-ttu-id="798a9-147">Malldistribution</span><span class="sxs-lookup"><span data-stu-id="798a9-147">Template deployment</span></span>

<span data-ttu-id="798a9-148">Azure VM-tillägg kan distribueras med Azure Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="798a9-148">Azure VM extensions can be deployed with Azure Resource Manager templates.</span></span> <span data-ttu-id="798a9-149">hello JSON-schema som beskrivs i föregående avsnitt i hello kan användas i en Azure Resource Manager mallen toorun hello Network Watcher Agent tillägg under en Azure Resource Manager för malldistribution.</span><span class="sxs-lookup"><span data-stu-id="798a9-149">hello JSON schema detailed in hello previous section can be used in an Azure Resource Manager template toorun hello Network Watcher Agent extension during an Azure Resource Manager template deployment.</span></span>

## <a name="azure-cli-deployment"></a><span data-ttu-id="798a9-150">Azure CLI-distribution</span><span class="sxs-lookup"><span data-stu-id="798a9-150">Azure CLI deployment</span></span>

<span data-ttu-id="798a9-151">hello Azure CLI kan vara används toodeploy hello nätverket Watcher Agent VM-tillägget tooan befintlig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="798a9-151">hello Azure CLI can be used toodeploy hello Network Watcher Agent VM extension tooan existing virtual machine.</span></span>

```azurecli
azure vm extension set myResourceGroup1 myVM1 NetworkWatcherAgentLinux Microsoft.Azure.NetworkWatcher 1.4
```

## <a name="troubleshooting-and-support"></a><span data-ttu-id="798a9-152">Felsökning och support</span><span class="sxs-lookup"><span data-stu-id="798a9-152">Troubleshooting and support</span></span>

### <a name="troubleshooting"></a><span data-ttu-id="798a9-153">Felsökning</span><span class="sxs-lookup"><span data-stu-id="798a9-153">Troubleshooting</span></span>

<span data-ttu-id="798a9-154">Data om hello tillståndet för distributioner av tillägget kan hämtas från hello Azure-portalen och genom att använda hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="798a9-154">Data about hello state of extension deployments can be retrieved from hello Azure portal, and by using hello Azure CLI.</span></span> <span data-ttu-id="798a9-155">toosee hello distributionsstatusen för tillägg för en viss virtuell dator, kör följande kommando använder hello hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="798a9-155">toosee hello deployment state of extensions for a given VM, run hello following command using hello Azure CLI.</span></span>

```azurecli
azure vm extension get myResourceGroup1 myVM1
```

<span data-ttu-id="798a9-156">Tillägget körning utdata är loggade toofiles hittades i hello följande katalog:</span><span class="sxs-lookup"><span data-stu-id="798a9-156">Extension execution output is logged toofiles found in hello following directory:</span></span>

`
/var/log/azure/Microsoft.Azure.NetworkWatcher.NetworkWatcherAgentLinux/
`

### <a name="support"></a><span data-ttu-id="798a9-157">Support</span><span class="sxs-lookup"><span data-stu-id="798a9-157">Support</span></span>

<span data-ttu-id="798a9-158">Om du behöver mer hjälp när som helst i den här artikeln kan du se toohello Nätverksbevakaren dokumentationen eller kontakta hello Azure experter på hello [MSDN Azure och Stack Overflow-forum](https://azure.microsoft.com/en-us/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="798a9-158">If you need more help at any point in this article, you can refer toohello Network Watcher documentation or contact hello Azure experts on hello [MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/en-us/support/forums/).</span></span> <span data-ttu-id="798a9-159">Alternativt kan du lagra en incident i Azure-supporten.</span><span class="sxs-lookup"><span data-stu-id="798a9-159">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="798a9-160">Gå toohello [Azure supportwebbplats](https://azure.microsoft.com/en-us/support/options/) och välja Get support.</span><span class="sxs-lookup"><span data-stu-id="798a9-160">Go toohello [Azure support site](https://azure.microsoft.com/en-us/support/options/) and select Get support.</span></span> <span data-ttu-id="798a9-161">Information om hur du använder Azure stöder läsa hello [vanliga frågor om Microsoft Azure-supporten](https://azure.microsoft.com/en-us/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="798a9-161">For information about using Azure Support, read hello [Microsoft Azure support FAQ](https://azure.microsoft.com/en-us/support/faq/).</span></span>
