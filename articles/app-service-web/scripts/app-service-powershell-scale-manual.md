---
title: aaaAzure PowerShell skriptexempel - skala ett webbprogram manuellt | Microsoft Docs
description: "Azure PowerShell-skript exempel – skala ett webbprogram manuellt"
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: de5d4285-9c7d-4735-a695-288264047375
ms.service: app-service
ms.devlang: multiple
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 03/20/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: c749031fbe6c6bcbb25395387b4f32b2ba75cef4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scale-a-web-app-manually"></a>Skala ett webbprogram manuellt

I det här scenariot får du lära dig toocreate en resursgrupp, apptjänst planerings- och webb-app. Du kommer sedan att skala hello App Service-Plan från en enda instans toomultiple instanser.

Om det behövs installerar du Azure PowerShell med hjälp av hello-instruktion finns i hello hello [Azure PowerShell guiden](/powershell/azure/overview), och kör sedan `Login-AzureRmAccount` toocreate en anslutning till Azure.

## <a name="sample-script"></a>Exempelskript

[!code-powershell[main](../../../powershell_scripts/app-service/scale-manual/scale-manual.ps1 "Scale a web app manually")]

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
| [Ny AzureRmAppServicePlan](/powershell/module/azurerm.websites/new-azurermappserviceplan) | Skapar en App Service-plan. |
| [Ny AzureRmWebApp](/powershell/module/azurerm.websites/new-azurermwebapp) | Skapar ett webbprogram. |
| [Ange AzureRmWebApp](/powershell/module/azurerm.websites/set-azurermwebapp) | Ändrar någon konfiguration för ett webbprogram. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).

Ytterligare Azure Powershell-exempel för Azure App Service Web Apps finns i hello [Azure PowerShell-exempel](../app-service-powershell-samples.md).
