---
title: "aaaCreate Azure Analysis Services-servern med hjälp av PowerShell | Microsoft Docs"
description: "Lär dig hur toocreate ett Azure Analysis Services-server med hjälp av PowerShell"
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 08/01/2017
ms.author: owend
ms.custom: mvc
ms.openlocfilehash: 269b78983410f773d47c4cea34d6d353b19f9e91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-analysis-services-server-by-using-powershell"></a>Skapa en Azure Analysis Services-server med PowerShell

Denna Snabbstart beskriver hur du använder PowerShell från hello kommandoraden toocreate en Azure Analysis Services-server i en [Azure-resursgrupp](../azure-resource-manager/resource-group-overview.md) i din Azure-prenumeration.

Den här uppgiften kräver Azure PowerShell-modul version 4.0 eller senare. toofind hello version, kör ` Get-Module -ListAvailable AzureRM`. tooinstall eller uppgradering, se [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps). 

> [!NOTE]
> Att skapa en server kan resultera i en ny fakturerbar tjänst. Det finns fler toolearn [priser för Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/).

## <a name="prerequisites"></a>Krav
toocomplete Snabbstart, behöver du:

* **Azure-prenumeration**: Besök [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/offers/ms-azr-0044p/) toocreate ett konto.
* **Azure Active Directory**: Prenumerationen måste vara kopplad till en Azure Active Directory-klientorganisation och du måste ha ett konto i den katalogen. Det finns fler toolearn [autentisering och användarbehörigheter](analysis-services-manage-users.md).

## <a name="import-azurermanalysisservices-module"></a>Importera AzureRm.AnalysisServices modul
toocreate en server i din prenumeration som du använder hello [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices) komponent modulen. Läsa hello AzureRm.AnalysisServices modulen i PowerShell-sessionen.

```powershell
Import-Module AzureRM.AnalysisServices
```

## <a name="sign-in-tooazure"></a>Logga in tooAzure

Logga in på tooyour Azure-prenumeration med hello [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) kommando. Följ hello på skärmen riktningar.

```powershell
Add-AzureRmAccount
```

## <a name="create-a-resource-group"></a>Skapa en resursgrupp
 
En [Azure-resursgrupp](../azure-resource-manager/resource-group-overview.md) är en logisk behållare där Azure-resurser distribueras och hanteras som en grupp. När du skapar servern måste du ange en resursgrupp i prenumerationen. Om du inte redan har en resursgrupp kan du skapa en ny genom att använda hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) kommando. hello följande exempel skapar en resursgrupp med namnet `myResourceGroup` i hello västra USA region.

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "West US"
```

## <a name="create-a-server"></a>Skapa en server

Skapa en ny server med hjälp av hello [ny AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) kommando. hello följande exempel skapas en server med namnet minserver i myResourceGroup i hello västra USA region på hello D1 lager, och anger philipc@adventureworks.com som en serveradministratör.

```powershell
New-AzureRmAnalysisServicesServer -ResourceGroupName "myResourceGroup" -Name "myServer" -Location West US -Sku D1 -Administrator "philipc@adventure-works.com"
```

## <a name="clean-up-resources"></a>Rensa resurser

Du kan ta bort hello server från prenumerationen med hello [ta bort AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) kommando. Ta inte bort servern om du fortsätter med andra snabbstartsguider och självstudier i den här samlingen. hello följande exempel tar bort hello server skapade i föregående steg i hello.


```powershell
Remove-AzureRmAnalysisServicesServer -Name "myServer" -ResourceGroupName "myResourceGroup"
```

## <a name="next-steps"></a>Nästa steg
[Hantera Azure Analysis Services med PowerShell](analysis-services-powershell.md)   
[Distribuera en modell från SSDT](analysis-services-deploy.md)   
[Skapa en modell i Azure Portal](analysis-services-create-model-portal.md)