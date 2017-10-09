---
title: "aaaDownload hello mall för en virtuell dator i Azure | Microsoft Docs"
description: "Hämta hello templatefor en VM toohelp automatisera distributioner i hello Resource Manager-distributionsmodellen"
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 51ef4f51-0942-4249-afea-4a3f87ce1ff8
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/22/2017
ms.author: cynthn
ms.openlocfilehash: 86fd05f67409019b5e5c9023881745047860eee1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="download-hello-template-for-a-vm"></a>Hämta hello mall för en virtuell dator
När du skapar en virtuell dator i Azure skapas med hello portal eller PowerShell, en resurshanterare mall automatiskt åt dig. Du kan använda den här mallen tooquickly dubblett en distribution. hello mallen innehåller information om alla hello resurser i en resursgrupp. För en virtuell dator, innebär detta att hello mallen innehåller allt som har skapats för att stödja hello VM i den resursgrupp, inklusive hello nätverksresurser.

## <a name="download-hello-template-using-hello-portal"></a>Hämta hello mallen med hjälp av hello portal
1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. En hello navmenyn väljer **virtuella datorer**.
3. Välj hello virtuella hello-listan.
4. Välj **automatiseringsskriptet**.
5. Välj **hämta** och spara hello ZIP-filen tooyour lokal dator.
6. Öppna hello ZIP-filen och extrahera hello tooa-mappen. hello ZIP-filen innehåller:
   
   * Deploy.ps1
   * Deploy.SH 
   * deployer.RB
   * DeploymentHelper.cs
   * parameters.JSON
   * Template.JSON

Hej template.json filen är hello mallen.

## <a name="download-hello-template-using-powershell"></a>Hämta hello mallen med hjälp av PowerShell
Du kan också hämta hello JSON mallfil med hjälp av hello [Export-AzureRMResourceGroup](https://msdn.microsoft.com/library/mt715427.aspx) cmdlet. Du kan använda hello `-path` parametern tooprovide hello filnamnet och sökvägen för hello JSON-fil. Det här exemplet illustrerar hur toodownload hello mallen för resursgruppen hello namnet **myResourceGroup** toohello **C:\users\public\downloads** mapp på den lokala datorn.

```powershell
    Export-AzureRmResourceGroup -ResourceGroupName "myResourceGroup" -Path "C:\users\public\downloads"
```

## <a name="next-steps"></a>Nästa steg
toolearn mer information om hur du distribuerar resurser med hjälp av mallar, se [genomgång av Resource Manager-mall](../../azure-resource-manager/resource-manager-template-walkthrough.md).

