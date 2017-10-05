---
title: "Skapa mallar med Linux VM-tillägg | Microsoft Docs"
description: "Lär dig mer om att skapa Azure Resource Manager-mallar med tillägg för virtuella Linux-datorer"
services: virtual-machines-linux
documentationcenter: 
author: kundanap
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 322f8f0b-6697-4acb-b5f3-b3f58d28358b
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/29/2016
ms.author: kundanap
ms.openlocfilehash: 8b017306474670bf8dde1440128e16ec35146f24
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="authoring-azure-resource-manager-templates-with-linux-vm-extensions"></a><span data-ttu-id="e01c8-103">Skapa Azure Resource Manager-mallar med Linux VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="e01c8-103">Authoring Azure Resource Manager templates with Linux VM extensions</span></span>
[!INCLUDE [virtual-machines-common-extensions-authoring-templates](../../../includes/virtual-machines-common-extensions-authoring-templates.md)]

<span data-ttu-id="e01c8-104">Kör följande på anslutningskommando från Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="e01c8-104">From Azure CLI, run the following commnad:</span></span>

      Azure VM extension list

<span data-ttu-id="e01c8-105">Det här kommandot returnerar Utgivarnamn, namn och version som följande:</span><span class="sxs-lookup"><span data-stu-id="e01c8-105">This command returns the publisher name, extension name and version as following:</span></span>

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

<span data-ttu-id="e01c8-106">De här egenskaperna mappas till ”publisher”, ”typ” och ”typeHandlerVersion” i ovanstående mall-fragment.</span><span class="sxs-lookup"><span data-stu-id="e01c8-106">These three properties map to "publisher", "type", and "typeHandlerVersion" respectively in the above template snippet.</span></span>

> [!NOTE]
> <span data-ttu-id="e01c8-107">Det har alltid bör du använda den senaste tilläggsversionen för att hämta den senaste uppdaterade funktionen.</span><span class="sxs-lookup"><span data-stu-id="e01c8-107">It's always recommended to use the latest extension version to get the most updated functionality.</span></span>
> 
> 

## <a name="identifying-the-schema-for-the-extension-configuration-parameters"></a><span data-ttu-id="e01c8-108">Identifiera schemat för konfigurationsparametrarna för tillägg</span><span class="sxs-lookup"><span data-stu-id="e01c8-108">Identifying the schema for the extension configuration parameters</span></span>
<span data-ttu-id="e01c8-109">Nästa steg med att redigera en mall för tillägget är att identifiera format för att tillhandahålla konfigurationsparametrar.</span><span class="sxs-lookup"><span data-stu-id="e01c8-109">The next step with authoring an extension template is to identify the format for providing configuration parameters.</span></span> <span data-ttu-id="e01c8-110">Varje tillägg stöder en egen uppsättning parametrar.</span><span class="sxs-lookup"><span data-stu-id="e01c8-110">Each extension supports its own set of parameters.</span></span>

<span data-ttu-id="e01c8-111">Om du vill titta på exemplet konfigurationer för Linux-tillägg, klickar du på dokumentation finns [Linux eExtensions exempel](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e01c8-111">To look at sample configurations for Linux extensions, click the documentation for see [Linux eExtensions samples](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="e01c8-112">Läs följande om du vill hämta en mall som är helt klar med VM-tillägg.</span><span class="sxs-lookup"><span data-stu-id="e01c8-112">Please refer to the following to get a fully complete template with VM Extensions.</span></span>

[<span data-ttu-id="e01c8-113">Tillägget för anpassat skript på en Linux-VM</span><span class="sxs-lookup"><span data-stu-id="e01c8-113">Custom script extension on a Linux VM</span></span>](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/mongodb-on-ubuntu/azuredeploy.json/)

<span data-ttu-id="e01c8-114">När du redigerar mallen kan du distribuera den med hjälp av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="e01c8-114">After authoring the template, you can deploy it using the Azure CLI.</span></span>

