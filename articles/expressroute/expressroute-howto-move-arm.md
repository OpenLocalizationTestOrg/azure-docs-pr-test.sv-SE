---
title: "Flytta ExpressRoute-kretsar från klassiska tooResource Manager: PowerShell: Azure | Microsoft Docs"
description: "Den här sidan beskrivs hur toomove klassiska kretsen-toohello Resource Manager distribution modellen med hjälp av PowerShell."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 08152836-23e7-42d1-9a56-8306b341cd91
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/03/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: 8dcadafca5e4f40773902cec5786eba1dbe133eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-expressroute-circuits-from-hello-classic-toohello-resource-manager-deployment-model-using-powershell"></a>Flytta ExpressRoute-kretsar från hello klassiska toohello Resource Manager-distributionsmodellen med hjälp av PowerShell

toouse en ExpressRoute-krets för både klassiska hello och Resource Manager distributionsmodellerna, måste du flytta hello krets toohello Resource Manager-modellen. hello hjälper följande avsnitt dig att flytta kretsen med hjälp av PowerShell.

## <a name="before-you-begin"></a>Innan du börjar

* Kontrollera att du har hello senaste versionen av hello Azure PowerShell-moduler (minst version 1.0). Mer information finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).
* Kontrollera att du har granskat hello [krav](expressroute-prerequisites.md), [routningskrav](expressroute-routing.md), och [arbetsflöden](expressroute-workflows.md) innan du börjar konfigurera.
* Granska hello information som tillhandahålls under [flytta en ExpressRoute-krets från klassiska tooResource Manager](expressroute-move.md). Kontrollera att du förstår hello gränser och begränsningar.
* Kontrollera att hello kretsen är fullt fungerande i hello klassiska distributionsmodellen.
* Se till att du har en resursgrupp som skapades i hello Resource Manager-distributionsmodellen.

## <a name="move-an-expressroute-circuit"></a>Flytta en ExpressRoute-krets

### <a name="step-1-gather-circuit-details-from-hello-classic-deployment-model"></a>Steg 1: Samla in krets information från hello klassiska distributionsmodellen

Logga in toohello klassiska Azure-miljön och samla in hello nyckel.

1. Logga in tooyour Azure-konto.

  ```powershell
  Add-AzureAccount
  ```

2. Välj lämplig Azure hello-prenumeration.

  ```powershell
  Select-AzureSubscription "<Enter Subscription Name here>"
  ```

3. Importera hello PowerShell-moduler för Azure och ExpressRoute.

  ```powershell
  Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
  Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'
  ```

4. Använd hello-cmdlet nedan för för tooget hello-tjänsten för alla ExpressRoute-kretsar. Efter att du hämtar hello nycklar, kopiera hello **tjänstnyckeln** av hello krets som du vill toomove toohello Resource Manager-modellen.

  ```powershell
  Get-AzureDedicatedCircuit
  ```

### <a name="step-2-sign-in-and-create-a-resource-group"></a>Steg 2: Logga in och skapa en resursgrupp

Logga in toohello Resource Manager-miljön och skapa en ny resursgrupp.

1. Logga in tooyour Azure Resource Manager-miljön.

  ```powershell
  Login-AzureRmAccount
  ```

2. Välj lämplig Azure hello-prenumeration.

  ```powershell
  Get-AzureRmSubscription -SubscriptionName "<Enter Subscription Name here>" | Select-AzureRmSubscription
  ```

3. Ändra hello fragment nedan toocreate en ny resursgrupp om du inte redan har en resursgrupp.

  ```powershell
  New-AzureRmResourceGroup -Name "DemoRG" -Location "West US"
  ```

### <a name="step-3-move-hello-expressroute-circuit-toohello-resource-manager-deployment-model"></a>Steg 3: Flytta hello ExpressRoute-kretsen toohello Resource Manager-modellen

Du är nu redo toomove ExpressRoute-krets från hello klassisk distribution modellen toohello Resource Manager-modellen. Innan du fortsätter bör du granska hello informationen i [flytta en ExpressRoute-krets från hello klassiska toohello Resource Manager-distributionsmodellen](expressroute-move.md).

toomove kretsen, ändra och kör följande fragment hello:

```powershell
Move-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "DemoRG" -Location "West US" -ServiceKey "<Service-key>"
```

> [!NOTE]
> När hello flyttningen är klar kommer hello nya namnet som anges i hello föregående cmdlet att använda tooaddress hello resurs. hello krets att i stort sett ändras.
> 

## <a name="modify-circuit-access"></a>Ändra krets åtkomst

### <a name="tooenable-expressroute-circuit-access-for-both-deployment-models"></a>tooenable ExpressRoute-kretsen åtkomst för båda distributionsmodellerna

När du har flyttat din klassiska distributionsmodellen för ExpressRoute-kretsen toohello Resource Manager kan du aktivera åtkomst tooboth distributionsmodeller. Kör följande cmdlets tooenable åtkomst tooboth distributionsmodeller hello:

1. Hämta hello krets information.

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"
  ```

2. Ange ”Tillåt klassiska åtgärder” tooTRUE.

  ```powershell
  $ckt.AllowClassicOperations = $true
  ```

3. Uppdatera hello krets. När den här åtgärden har slutförts, kommer du att kunna tooview hello krets i hello klassiska distributionsmodellen.

  ```powershell
  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

4. Kör följande cmdlet tooget hello information av hello ExpressRoute-krets hello. Du måste vara kan toosee hello nyckeln för tjänsten i listan.

  ```powershell
  get-azurededicatedcircuit
  ```

5. Du kan nu hantera länkar toohello ExpressRoute-krets kommandona hello klassisk distribution modellen för klassiska Vnet och hello Resource Manager-kommandon för Resource Manager VNets. hello hjälper följande artiklar dig att hantera länkar toohello ExpressRoute-krets:

    * [Länka ditt virtuella nätverk tooyour ExpressRoute-krets i hello Resource Manager-distributionsmodellen](expressroute-howto-linkvnet-arm.md)
    * [Länka ditt virtuella nätverk tooyour ExpressRoute-krets i hello klassiska distributionsmodellen](expressroute-howto-linkvnet-classic.md)

### <a name="toodisable-expressroute-circuit-access-toohello-classic-deployment-model"></a>toodisable ExpressRoute-kretsen åtkomst toohello klassiska distributionsmodellen

Kör följande cmdlets toodisable klassiska distributionsmodellen för åtkomst toohello hello.

1. Hämta information om hello ExpressRoute-kretsen.

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"
  ```

2. Ange ”Tillåt klassiska åtgärder” tooFALSE.

  ```powershell
  $ckt.AllowClassicOperations = $false
  ```

3. Uppdatera hello krets. När den här åtgärden har slutförts, kommer inte att kunna tooview hello krets i hello klassiska distributionsmodellen.

  ```powershell
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

## <a name="next-steps"></a>Nästa steg

* [Skapa och ändra routning för ExpressRoute-krets](expressroute-howto-routing-arm.md)
* [Länka ditt virtuella nätverk tooyour ExpressRoute-krets](expressroute-howto-linkvnet-arm.md)
