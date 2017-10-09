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
# <a name="authoring-azure-resource-manager-templates-with-windows-vm-extensions"></a>Skapa Azure Resource Manager-mallar med Windows VM-tillägg
[!INCLUDE [virtual-machines-common-extensions-authoring-templates](../../../includes/virtual-machines-common-extensions-authoring-templates.md)]

Kör följande Azure PowerShell-cmdleten hello från Azure PowerShell:

      Get-AzureVMAvailableExtension


Denna cmdlet returnerar hello utgivarens namn, namn och version på följande sätt:

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

Mappa dessa tre egenskaper för ”publisher”, ”typ” och ”typeHandlerVersion” i hello ovan mallen fragment.

> [!NOTE]
> Det är alltid rekommenderade toouse hello senaste tillägget version tooget hello mest uppdaterade funktioner.
> 
> 

## <a name="identifying-hello-schema-for-hello-extension-configuration-parameters"></a>Identifiera hello schemat för hello konfigurationsparametrar för tillägg
hello nästa steg med att redigera en mall för tillägget är tooidentify hello format för att tillhandahålla konfigurationsparametrar. Varje tillägg stöder en egen uppsättning parametrar.

toolook på exempel konfigurationer för Windows-tillägg finns [Windows tillägg exempel](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Se toohello efter tooget en mall som är helt klar med VM-tillägg.

[Tillägget för anpassat skript på en Windows VM](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/201-list-storage-keys-windows-vm/azuredeploy.json/)

När du redigerar hello mallen distribuerar du den med hjälp av Azure PowerShell.

