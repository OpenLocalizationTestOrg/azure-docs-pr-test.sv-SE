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
# <a name="authoring-azure-resource-manager-templates-with-linux-vm-extensions"></a>Skapa Azure Resource Manager-mallar med Linux VM-tillägg
[!INCLUDE [virtual-machines-common-extensions-authoring-templates](../../../includes/virtual-machines-common-extensions-authoring-templates.md)]

Från Azure CLI kör du följande på anslutningskommando hello:

      Azure VM extension list

Det här kommandot returnerar hello utgivarens namn, namn och version enligt följande:

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

toolook på exempel konfigurationer för Linux-tillägg, klicka på hello-dokumentationen finns [Linux eExtensions exempel](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Se toohello efter tooget en mall som är helt klar med VM-tillägg.

[Tillägget för anpassat skript på en Linux-VM](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/mongodb-on-ubuntu/azuredeploy.json/)

När du redigerar hello mallen distribuerar du den med hjälp av hello Azure CLI.

