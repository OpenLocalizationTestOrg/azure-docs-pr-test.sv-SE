---
title: "Skapa mallar med Windows VM-tillägg | Microsoft Docs"
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
ms.openlocfilehash: 338668df33783d8ef62c6710f4d96ce805296fe5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="authoring-azure-resource-manager-templates-with-windows-vm-extensions"></a><span data-ttu-id="73eb9-103">Skapa Azure Resource Manager-mallar med Windows VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="73eb9-103">Authoring Azure Resource Manager templates with Windows VM extensions</span></span>
[!INCLUDE [virtual-machines-common-extensions-authoring-templates](../../../includes/virtual-machines-common-extensions-authoring-templates.md)]

<span data-ttu-id="73eb9-104">Kör följande Azure PowerShell-cmdlet från Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="73eb9-104">From Azure PowerShell, run the following Azure PowerShell cmdlet:</span></span>

      Get-AzureVMAvailableExtension


<span data-ttu-id="73eb9-105">Denna cmdlet returnerar utgivarens namn, namn och version på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="73eb9-105">This cmdlet returns the publisher name, extension name, and version as follows:</span></span>

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

<span data-ttu-id="73eb9-106">De här egenskaperna mappas till ”publisher”, ”typ” och ”typeHandlerVersion” i ovanstående mall-fragment.</span><span class="sxs-lookup"><span data-stu-id="73eb9-106">These three properties map to "publisher", "type", and "typeHandlerVersion" respectively in the above template snippet.</span></span>

> [!NOTE]
> <span data-ttu-id="73eb9-107">Det har alltid bör du använda den senaste tilläggsversionen för att hämta den senaste uppdaterade funktionen.</span><span class="sxs-lookup"><span data-stu-id="73eb9-107">It's always recommended to use the latest extension version to get the most updated functionality.</span></span>
> 
> 

## <a name="identifying-the-schema-for-the-extension-configuration-parameters"></a><span data-ttu-id="73eb9-108">Identifiera schemat för konfigurationsparametrarna för tillägg</span><span class="sxs-lookup"><span data-stu-id="73eb9-108">Identifying the schema for the extension configuration parameters</span></span>
<span data-ttu-id="73eb9-109">Nästa steg med att redigera en mall för tillägget är att identifiera format för att tillhandahålla konfigurationsparametrar.</span><span class="sxs-lookup"><span data-stu-id="73eb9-109">The next step with authoring an extension template is to identify the format for providing configuration parameters.</span></span> <span data-ttu-id="73eb9-110">Varje tillägg stöder en egen uppsättning parametrar.</span><span class="sxs-lookup"><span data-stu-id="73eb9-110">Each extension supports its own set of parameters.</span></span>

<span data-ttu-id="73eb9-111">Om du vill titta på exemplet konfigurationer för Windows-tillägg finns [Windows tillägg exempel](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="73eb9-111">To look at sample configurations for Windows extensions, see [Windows extensions samples](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="73eb9-112">Läs följande om du vill hämta en mall som är helt klar med VM-tillägg.</span><span class="sxs-lookup"><span data-stu-id="73eb9-112">Please refer to the following to get a fully complete template with VM extensions.</span></span>

[<span data-ttu-id="73eb9-113">Tillägget för anpassat skript på en Windows VM</span><span class="sxs-lookup"><span data-stu-id="73eb9-113">Custom Script Extension on a Windows VM</span></span>](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/201-list-storage-keys-windows-vm/azuredeploy.json/)

<span data-ttu-id="73eb9-114">När du redigerar mallen kan du distribuera den med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="73eb9-114">After authoring the template, you can deploy it using Azure PowerShell.</span></span>

