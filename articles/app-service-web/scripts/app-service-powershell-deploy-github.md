---
title: "aaaAzure PowerShell skriptexempel - skapa en webbapp och distribuera kod från GitHub | Microsoft Docs"
description: "Azure PowerShell skriptexempel - skapa en webbapp och distribuera kod från GitHub"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 0f9c8bc5-3789-4eb3-8deb-ae6e2200795a
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 9a28f9cb01b71c86fa0a3f1d0a6761fc3d45d43b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-and-deploy-code-from-github"></a>Skapa en webbapp och distribuera kod från GitHub

Det här exempelskriptet skapar en webbapp i App Service med dess relaterade resurser och distribuerar sedan web app-kod från en offentlig GitHub-databas (utan kontinuerlig distribution). GitHub-distribution med kontinuerlig distribution finns [skapa ett webbprogram med kontinuerlig distribution från GitHub](app-service-powershell-continuous-deployment-github.md).

Om det behövs installerar du Azure PowerShell med hjälp av hello-instruktion finns i hello hello [Azure PowerShell guiden](/powershell/azure/overview). Du måste även en länk tooGitHub databas som innehåller hello web app-kod.

## <a name="sample-script"></a>Exempelskript

[!code-powershell[main](../../../powershell_scripts/app-service/deploy-github/deploy-github.ps1?highlight=1-2 "Create a web app and deploy code from GitHub")]

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
| [Ange AzureRmResource](/powershell/module/azurerm.resources/set-azurermresource) | Ändrar en resurs i en resursgrupp. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).

Ytterligare Azure Powershell-exempel för Azure App Service Web Apps finns i hello [Azure PowerShell-exempel](../app-service-powershell-samples.md).
