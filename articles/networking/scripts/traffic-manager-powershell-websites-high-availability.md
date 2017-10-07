---
title: "aaaAzure skriptexempel PowerShell - vägtrafik för hög tillgänglighet för program | Microsoft Docs"
description: "Azure PowerShell skriptexempel - vägtrafik för hög tillgänglighet för program"
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
manager: timlt
editor: georgewallace
tags: azure-infrastructure
ms.assetid: 
ms.service: traffic-manager
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: traffic-manager
ms.date: 05/16/2017
ms.author: gwallace
ms.openlocfilehash: 11d15780403b4ed79e85d7b3495bc5d674bfdaee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="route-traffic-for-high-availability-of-applications"></a>Vidarebefordra trafik för hög tillgänglighet för program

Det här skriptet skapar en resursgrupp, två apptjänstplaner, två webbappar, en traffic manager-profil och två traffic manager-slutpunkter. Traffic Manager dirigerar trafik toohello programmet i en region som hello primär region och toohello sekundär region när programmet hello i hello primära regionen är inte tillgänglig. Innan du kör skriptet hello måste du ändra hello MyWebApp, MyWebAppL1 och MyWebAppL2 värdena toounique värdena i Azure. Du kan komma åt hello app i hello primär region med hello URL mywebapp.trafficmanager.net när du har kört hello skript.

Om det behövs installerar du Azure PowerShell med hjälp av hello-instruktion finns i hello hello [Azure PowerShell guiden](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), och kör sedan `Login-AzureRmAccount` toocreate en anslutning till Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript

[!code-powershell[main](../../../powershell_scripts/traffic-manager/direct-traffic-for-increased-application-availability/direct-traffic-for-increased-application-availability.ps1 "Route traffic for high availability")]


Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup1
Remove-AzureRmResourceGroup -Name myResourceGroup2
```


## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder följande kommandon toocreate en resursgrupp, webbprogram, trafikhanterarprofilen hello och alla relaterade resurser. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Ny AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)  | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Ny AzureRmAppServicePlan](/powershell/module/azurerm.websites/new-azurermappserviceplan) | Skapar en App Service-plan. Detta påminner om en servergrupp för din Azure webbapp. |
| [Ny AzureRmWebApp](/powershell/module/azurerm.websites/new-azurermwebapp) | Skapar en Azure webbapp i hello App Service-plan. |
| [Ange AzureRmResource](/powershell/module/azurerm.resources/new-azurermresource) | Skapar en Azure webbapp i hello App Service-plan. |
| [Ny AzureRmTrafficManagerProfile](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerprofile) | Skapar en Azure Traffic Manager-profil. |
| [Ny AzureRmTrafficManagerEndpoint](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerendpoint) | Lägger till en slutpunkt tooan Azure Traffic Manager-profilen. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure PowerShell finns [Azure PowerShell dokumentationen](https://docs.microsoft.com/powershell/azure/overview).

Ytterligare nätverk PowerShell-skript-exempel finns i hello [dokumentation för Azure-nätverk – översikt](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).
