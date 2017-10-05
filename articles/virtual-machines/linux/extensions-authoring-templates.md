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
# <a name="authoring-azure-resource-manager-templates-with-linux-vm-extensions"></a>Skapa Azure Resource Manager-mallar med Linux VM-tillägg
[!INCLUDE [virtual-machines-common-extensions-authoring-templates](../../../includes/virtual-machines-common-extensions-authoring-templates.md)]

Kör följande på anslutningskommando från Azure CLI:

      Azure VM extension list

Det här kommandot returnerar Utgivarnamn, namn och version som följande:

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

Om du vill titta på exemplet konfigurationer för Linux-tillägg, klickar du på dokumentation finns [Linux eExtensions exempel](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Läs följande om du vill hämta en mall som är helt klar med VM-tillägg.

[Tillägget för anpassat skript på en Linux-VM](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/mongodb-on-ubuntu/azuredeploy.json/)

När du redigerar mallen kan du distribuera den med hjälp av Azure CLI.

