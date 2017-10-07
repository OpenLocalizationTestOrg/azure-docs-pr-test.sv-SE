---
title: "aaaAzure PowerShell skriptexempel - binda ett anpassat webbprogram för SSL-certifikat tooa | Microsoft Docs"
description: "Azure PowerShell skriptexempel - Bind ett anpassat webbprogram för SSL-certifikat tooa"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 23e83b74-614a-49a0-bc08-7542120eeec5
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 6e249ceedb9e2b8872dd0bc8b0aea0d718b28dab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="bind-a-custom-ssl-certificate-tooa-web-app"></a>Bind ett anpassat webbprogram för SSL-certifikat tooa

Det här exempelskriptet skapar en webbapp i App Service med dess relaterade resurser och Binder hello SSL-certifikatet för en anpassad domän namnet tooit. 

Om det behövs installerar du Azure PowerShell med hjälp av hello-instruktion finns i hello hello [Azure PowerShell guiden](/powershell/azure/overview). Se också till att:

- En anslutning med Azure har skapats med hjälp av hello `az login` kommando.
- Du har åtkomst tooyour domän domänregistrators DNS-konfigurationssidan.
- Du har en giltig. PFX-filen och lösenordet för hello SSL-certifikat som du vill tooupload och bind.

## <a name="sample-script"></a>Exempelskript

[!code-powershell[main](../../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom SSL certificate tooa web app")]

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
| [Ange AzureRmAppServicePlan](/powershell/module/azurerm.websites/set-azurermappserviceplan) | Ändrar en App Service-plan toochange dess prisnivå. |
| [Ange AzureRmWebApp](/powershell/module/azurerm.websites/set-azurermwebapp) | Ändrar någon konfiguration för ett webbprogram. |
| [Ny AzureRmWebAppSSLBinding](/powershell/module/azurerm.websites/new-azurermwebappsslbinding) | Skapar en SSL-certifikat-bindning för en webbapp. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).

Ytterligare Azure Powershell-exempel för Azure App Service Web Apps finns i hello [Azure PowerShell-exempel](../app-service-powershell-samples.md).
