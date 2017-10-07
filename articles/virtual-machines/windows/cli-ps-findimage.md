---
title: aaaSelect Windows VM bilder i Azure | Microsoft Docs
description: "Lär dig hur toouse Azure PowerSHell toodetermine hello utgivare, erbjudande, SKU och version för Marketplace VM-avbildningar."
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 188b8974-fabd-4cd3-b7dc-559cbb86b98a
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/12/2017
ms.author: danlep
ms.openlocfilehash: 752edcd0935f5141832e49503ae800ea0145e219
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toofind-windows-vm-images-in-hello-azure-marketplace-with-azure-powershell"></a>Hur toofind Windows VM bilder i hello Azure Marketplace med Azure PowerShell

Det här avsnittet beskrivs hur toouse Azure PowerShell toofind VM bilder i hello Azure Marketplace. Använd den här informationen toospecify Marketplace-avbildning när du skapar en virtuell Windows-dator.

Kontrollera att du har installerat och konfigurerat hello senaste [Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).



## <a name="table-of-commonly-used-windows-images"></a>Tabell med vanliga Windows-avbildningar
| PublisherName | Erbjudande | Sku |
|:--- |:--- |:--- |:--- |
| MicrosoftWindowsServer |WindowsServer |2016 Datacenter |
| MicrosoftWindowsServer |WindowsServer |2016-Datacenter-Server-Core |
| MicrosoftWindowsServer |WindowsServer |2016 Datacenter med behållare |
| MicrosoftWindowsServer |WindowsServer |2016 Nano Server |
| MicrosoftWindowsServer |WindowsServer |2012-R2-Datacenter |
| MicrosoftWindowsServer |WindowsServer |2008 R2 SP1 |
| MicrosoftDynamicsNAV |DynamicsNAV |2017 |
| MicrosoftSharePoint |MicrosoftSharePointServer |2016 |
| MicrosoftSQLServer |SQL2016 WS2016 |Enterprise |
| MicrosoftSQLServer |SQL2014SP2 WS2012R2 |Enterprise |
| MicrosoftWindowsServerHPCPack |WindowsServerHPCPack |2012R2 |
| MicrosoftWindowsServerEssentials |WindowsServerEssentials |WindowsServerEssentials |

## <a name="find-specific-images"></a>Söka efter specifika avbildningar


När du skapar en ny virtuell dator med Azure Resource Manager kan i vissa fall behöver du toospecify en bild med hello kombination av hello följande egenskaper:

* Utgivare
* Erbjudande
* SKU

Till exempel använda dessa värden med hello [Set AzureRMVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) PowerShell-cmdlet eller med en mall för gruppen av resurs som du måste ange hello typ av VM-toobe skapas.

Om du behöver toodetermine dessa värden, kan du köra hello [Get-AzureRMVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher), [Get-AzureRMVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer), och [Get-AzureRMVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) cmdlets toonavigate hello bilder. Du kan bestämma dessa värden.

1. Lista hello avbildningen utgivare.
2. Visa en lista över erbjudanden från en viss utgivare.
3. Visa en lista över SKU:er för ett visst erbjudande.

Först lista hello utgivare med hello följande kommandon:

```powershell
$locName="<Azure location, such as West US>"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName
```

Fyll i din valda utgivarens namn och kör hello följande kommandon:

```powershell
$pubName="<publisher>"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

Fyll i namnet på din valda erbjudandet och kör följande kommandon hello:

```powershell
$offerName="<offer>"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

Från hello utdata från hello `Get-AzureRMVMImageSku` kommando du har alla hello information du behöver toospecify hello avbildning för en ny virtuell dator.

hello följande visar ett fullständigt exempel:

```powershell
$locName="West US"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName

```

Resultat:

```
PublisherName
-------------
a10networks
aiscaler-cache-control-ddos-and-url-rewriting-
alertlogic
AlertLogic.Extension
Barracuda.Azure.ConnectivityAgent
barracudanetworks
basho
boxless
bssw
Canonical
...
```

För hello ”Microsoft Windows Server” publisher:

```powershell
$pubName="MicrosoftWindowsServer"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

Resultat:

```
Offer
-----
Windows-HUB
WindowsServer
WindowsServer-HUB
```

För hello ”Windows Server”-erbjudande:

```powershell
$offerName="WindowsServer"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

Resultat:

```
Skus
----
2008-R2-SP1
2008-R2-SP1-smalldisk
2012-Datacenter
2012-Datacenter-smalldisk
2012-R2-Datacenter
2012-R2-Datacenter-smalldisk
2016-Datacenter
2016-Datacenter-Server-Core
2016-Datacenter-Server-Core-smalldisk
2016-Datacenter-smalldisk
2016-Datacenter-with-Containers
2016-Nano-Server
```

Kopiera hello valt SKU namn från den här listan, och du har alla hello-information för hello `Set-AzureRMVMSourceImage` PowerShell-cmdlet eller för en resurs Gruppmall.

## <a name="next-steps"></a>Nästa steg
Nu kan du välja exakt hello bild du toouse. toocreate finns i en virtuell dator snabbt med hjälp av hello avbildningen information som du har hittat [skapar en virtuell Windows-dator med PowerShell](quick-create-powershell.md).
