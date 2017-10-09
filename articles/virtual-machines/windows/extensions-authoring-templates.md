---
title: "aaaAuthoring mallar med Windows VM-tillägg | Microsoft Docs"
description: "Lär dig mer om att skapa Azure Resource Manager-mallar med tillägg för virtuella Windows-datorer"
services: virtual-machines-windows
documentationcenter: 
author: kundanap
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 418dd1f7-ded8-45ab-9a5a-a59d245e2555
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/29/2016
ms.author: kundanap
ms.openlocfilehash: c5156cb3859f7ff86bebda942150d268e57d6486
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="authoring-azure-resource-manager-templates-with-windows-vm-extensions"></a><span data-ttu-id="97a3b-103">Skapa Azure Resource Manager-mallar med Windows VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="97a3b-103">Authoring Azure Resource Manager templates with Windows VM extensions</span></span>
[!INCLUDE [virtual-machines-common-extensions-authoring-templates](../../../includes/virtual-machines-common-extensions-authoring-templates.md)]

<span data-ttu-id="97a3b-104">Kör följande Azure PowerShell-cmdleten hello från Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="97a3b-104">From Azure PowerShell, run hello following Azure PowerShell cmdlet:</span></span>

      Get-AzureVMAvailableExtension


<span data-ttu-id="97a3b-105">Denna cmdlet returnerar hello utgivarens namn, namn och version på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="97a3b-105">This cmdlet returns hello publisher name, extension name, and version as follows:</span></span>

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

<span data-ttu-id="97a3b-106">Mappa dessa tre egenskaper för ”publisher”, ”typ” och ”typeHandlerVersion” i hello ovan mallen fragment.</span><span class="sxs-lookup"><span data-stu-id="97a3b-106">These three properties map too"publisher", "type", and "typeHandlerVersion" respectively in hello above template snippet.</span></span>

> [!NOTE]
> <span data-ttu-id="97a3b-107">Det är alltid rekommenderade toouse hello senaste tillägget version tooget hello mest uppdaterade funktioner.</span><span class="sxs-lookup"><span data-stu-id="97a3b-107">It's always recommended toouse hello latest extension version tooget hello most updated functionality.</span></span>
> 
> 

## <a name="identifying-hello-schema-for-hello-extension-configuration-parameters"></a><span data-ttu-id="97a3b-108">Identifiera hello schemat för hello konfigurationsparametrar för tillägg</span><span class="sxs-lookup"><span data-stu-id="97a3b-108">Identifying hello schema for hello extension configuration parameters</span></span>
<span data-ttu-id="97a3b-109">hello nästa steg med att redigera en mall för tillägget är tooidentify hello format för att tillhandahålla konfigurationsparametrar.</span><span class="sxs-lookup"><span data-stu-id="97a3b-109">hello next step with authoring an extension template is tooidentify hello format for providing configuration parameters.</span></span> <span data-ttu-id="97a3b-110">Varje tillägg stöder en egen uppsättning parametrar.</span><span class="sxs-lookup"><span data-stu-id="97a3b-110">Each extension supports its own set of parameters.</span></span>

<span data-ttu-id="97a3b-111">toolook på exempel konfigurationer för Windows-tillägg finns [Windows tillägg exempel](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="97a3b-111">toolook at sample configurations for Windows extensions, see [Windows extensions samples](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="97a3b-112">Se toohello efter tooget en mall som är helt klar med VM-tillägg.</span><span class="sxs-lookup"><span data-stu-id="97a3b-112">Please refer toohello following tooget a fully complete template with VM extensions.</span></span>

[<span data-ttu-id="97a3b-113">Tillägget för anpassat skript på en Windows VM</span><span class="sxs-lookup"><span data-stu-id="97a3b-113">Custom Script Extension on a Windows VM</span></span>](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/201-list-storage-keys-windows-vm/azuredeploy.json/)

<span data-ttu-id="97a3b-114">När du redigerar hello mallen distribuerar du den med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="97a3b-114">After authoring hello template, you can deploy it using Azure PowerShell.</span></span>

