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
# <a name="authoring-azure-resource-manager-templates-with-windows-vm-extensions"></a>Skapa Azure Resource Manager-mallar med Windows VM-tillägg
[!INCLUDE [virtual-machines-common-extensions-authoring-templates](../../../includes/virtual-machines-common-extensions-authoring-templates.md)]

Kör följande Azure PowerShell-cmdlet från Azure PowerShell:

      Get-AzureVMAvailableExtension


Denna cmdlet returnerar utgivarens namn, namn och version på följande sätt:

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

De här egenskaperna mappas till ”publisher”, ”typ” och ”typeHandlerVersion” i ovanstående mall-fragment.

> [!NOTE]
> Det har alltid bör du använda den senaste tilläggsversionen för att hämta den senaste uppdaterade funktionen.
> 
> 

## <a name="identifying-the-schema-for-the-extension-configuration-parameters"></a>Identifiera schemat för konfigurationsparametrarna för tillägg
Nästa steg med att redigera en mall för tillägget är att identifiera format för att tillhandahålla konfigurationsparametrar. Varje tillägg stöder en egen uppsättning parametrar.

Om du vill titta på exemplet konfigurationer för Windows-tillägg finns [Windows tillägg exempel](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Läs följande om du vill hämta en mall som är helt klar med VM-tillägg.

[Tillägget för anpassat skript på en Windows VM](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/201-list-storage-keys-windows-vm/azuredeploy.json/)

När du redigerar mallen kan du distribuera den med hjälp av Azure PowerShell.

