---
title: "aaaAzure PowerShell skriptexempel - skala ett webbprogram över hela världen med en hög tillgänglighet-arkitektur | Microsoft Docs"
description: "Azure PowerShell skriptexempel - skala en webbapp över hela världen med en arkitektur med hög tillgänglighet"
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 470f0129-1efe-462c-a029-5c66e04158a8
ms.service: app-service
ms.devlang: multiple
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 03/20/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 1fcda23250efe4966d63c5dfa744b76c26f3762a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scale-a-web-app-worldwide-with-a-high-availability-architecture"></a>Skala ett webbprogram över hela världen med en arkitektur med hög tillgänglighet

I det här scenariot skapar du en resursgrupp, två apptjänstplaner, två webbappar, en traffic manager-profil och två traffic manager-slutpunkter. När hello övningen är klar har du en hög tillgänglighet arkitektur som gör att tillhandahåller globala tillgängligheten hos ditt webbprogram baserat på hello lägsta Nätverksfördröjningen.

Om det behövs installerar du Azure PowerShell med hjälp av hello-instruktion finns i hello hello [Azure PowerShell guiden](/powershell/azure/overview), och kör sedan `Login-AzureRmAccount` toocreate en anslutning till Azure.

## <a name="sample-script"></a>Exempelskript

[!code-powershell[main](../../../powershell_scripts/app-service/scale-geographic/scale-geographic.ps1 "Scale a web app worldwide with a high-availability architecture")]

## <a name="clean-up-deployment"></a>Rensa distribution 

Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgrupp, webbprogram och alla relaterade resurser.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder hello följande kommandon. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Ny AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Ny AzureRMTrafficManagerProfile](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerprofile) | Skapar en Traffic Manager-profilen. |
| [Ny AzureRmAppServicePlan](/powershell/module/azurerm.websites/new-azurermappserviceplan) | Skapar en App Service-plan. |
| [Ny AzureRmWebApp](/powershell/module/azurerm.websites/new-azurermwebapp) | Skapar ett webbprogram. |
| [Ny AzureRMTrafficManagerEndpoint](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerendpoint) | Skapar en slutpunkt i Traffic Manager-profilen. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).

Ytterligare Azure Powershell-exempel för Azure App Service Web Apps finns i hello [Azure PowerShell-exempel](../app-service-powershell-samples.md).
