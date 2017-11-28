---
title: "aaaAuthoring mallar med Linux VM-tillägg | Microsoft Docs"
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
ms.openlocfilehash: b797e442d8706956bbc06c5be611a2b0119055d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="authoring-azure-resource-manager-templates-with-linux-vm-extensions"></a><span data-ttu-id="f6a72-103">Skapa Azure Resource Manager-mallar med Linux VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="f6a72-103">Authoring Azure Resource Manager templates with Linux VM extensions</span></span>
[!INCLUDE [virtual-machines-common-extensions-authoring-templates](../../../includes/virtual-machines-common-extensions-authoring-templates.md)]

<span data-ttu-id="f6a72-104">Från Azure CLI kör du följande på anslutningskommando hello:</span><span class="sxs-lookup"><span data-stu-id="f6a72-104">From Azure CLI, run hello following commnad:</span></span>

      Azure VM extension list

<span data-ttu-id="f6a72-105">Det här kommandot returnerar hello utgivarens namn, namn och version enligt följande:</span><span class="sxs-lookup"><span data-stu-id="f6a72-105">This command returns hello publisher name, extension name and version as following:</span></span>

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

<span data-ttu-id="f6a72-106">Mappa dessa tre egenskaper för ”publisher”, ”typ” och ”typeHandlerVersion” i hello ovan mallen fragment.</span><span class="sxs-lookup"><span data-stu-id="f6a72-106">These three properties map too"publisher", "type", and "typeHandlerVersion" respectively in hello above template snippet.</span></span>

> [!NOTE]
> <span data-ttu-id="f6a72-107">Det är alltid rekommenderade toouse hello senaste tillägget version tooget hello mest uppdaterade funktioner.</span><span class="sxs-lookup"><span data-stu-id="f6a72-107">It's always recommended toouse hello latest extension version tooget hello most updated functionality.</span></span>
> 
> 

## <a name="identifying-hello-schema-for-hello-extension-configuration-parameters"></a><span data-ttu-id="f6a72-108">Identifiera hello schemat för hello konfigurationsparametrar för tillägg</span><span class="sxs-lookup"><span data-stu-id="f6a72-108">Identifying hello schema for hello extension configuration parameters</span></span>
<span data-ttu-id="f6a72-109">hello nästa steg med att redigera en mall för tillägget är tooidentify hello format för att tillhandahålla konfigurationsparametrar.</span><span class="sxs-lookup"><span data-stu-id="f6a72-109">hello next step with authoring an extension template is tooidentify hello format for providing configuration parameters.</span></span> <span data-ttu-id="f6a72-110">Varje tillägg stöder en egen uppsättning parametrar.</span><span class="sxs-lookup"><span data-stu-id="f6a72-110">Each extension supports its own set of parameters.</span></span>

<span data-ttu-id="f6a72-111">toolook på exempel konfigurationer för Linux-tillägg, klicka på hello-dokumentationen finns [Linux eExtensions exempel](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f6a72-111">toolook at sample configurations for Linux extensions, click hello documentation for see [Linux eExtensions samples](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="f6a72-112">Se toohello efter tooget en mall som är helt klar med VM-tillägg.</span><span class="sxs-lookup"><span data-stu-id="f6a72-112">Please refer toohello following tooget a fully complete template with VM Extensions.</span></span>

[<span data-ttu-id="f6a72-113">Tillägget för anpassat skript på en Linux-VM</span><span class="sxs-lookup"><span data-stu-id="f6a72-113">Custom script extension on a Linux VM</span></span>](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/mongodb-on-ubuntu/azuredeploy.json/)

<span data-ttu-id="f6a72-114">När du redigerar hello mallen distribuerar du den med hjälp av hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="f6a72-114">After authoring hello template, you can deploy it using hello Azure CLI.</span></span>

