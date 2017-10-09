---
title: "aaaAzure skriptexempel PowerShell - öppna programmet port i belastningsutjämnaren | Microsoft Docs"
description: "Azure PowerShell skriptexempel - öppna en port i hello Azure belastningsutjämnare för ett Service Fabric-program."
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 6acb28942851dce63f89f7de362b7cf1dc7b1fee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="open-an-application-port-in-hello-azure-load-balancer"></a>Öppna en port för programmet i hello Azure belastningsutjämnare

Ett Service Fabric-program som körs i Azure är placerad bakom hello Azure belastningsutjämnare. Det här exempelskriptet öppnar en port i en Azure belastningsutjämnare så att ett Service Fabric-program kan kommunicera med externa klienter. Anpassa hello parametrarna efter behov. 

Om det behövs installerar du hello Service Fabric PowerShell-modulen med hello [Service Fabric SDK](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Exempelskript

[!code-powershell[main](../../../powershell_scripts/service-fabric/open-port-in-load-balancer/open-port-in-load-balancer.ps1 "Open a port in hello load balancer")]

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder hello följande kommandon. Varje kommando i hello tabellen länkar toocommand specifika-dokumentationen.

| Kommando | Anteckningar |
|---|---|
| [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource) | Hämtar en Azure-resurs.  |
| [Get-AzureRmLoadBalancer](/powershell/module/azurerm.network/get-azurermloadbalancer) | Hämtar hello Azure belastningsutjämnare. |
| [Lägg till AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig) | Lägger till en belastningsutjämnare för avsökning configuration tooa.|
| [Get-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/get-azurermloadbalancerprobeconfig) | Hämtar en avsökning konfiguration för en belastningsutjämnare. |
| [Lägg till AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig) | Lägger till en belastningsutjämnare för regeln configuration tooa. |
| [Ange AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer) | Anger hello målet tillstånd för en belastningsutjämnare. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).

Ytterligare Powershell-exempel för Azure Service Fabric kan hittas i hello [Azure PowerShell-exempel](../service-fabric-powershell-samples.md).
