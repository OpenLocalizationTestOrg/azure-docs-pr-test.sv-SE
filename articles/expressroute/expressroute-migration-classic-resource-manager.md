---
title: "Migrera ExpressRoute associerade virtuella nätverk från klassiska tooResource Manager: Azure: PowerShell | Microsoft Docs"
description: "Den här sidan beskrivs hur toomigrate associerade virtuella nätverk tooResource Manager när du har flyttat kretsen."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/06/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: e64506c6909296f98c5dd23b1437bc0b81f31c75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-expressroute-associated-virtual-networks-from-classic-tooresource-manager"></a>Migrera ExpressRoute associerade virtuella nätverk från klassiska tooResource Manager

Den här artikeln förklarar hur toomigrate Azure ExpressRoute associerade virtuella nätverk från hello klassisk distribution modellen toohello Azure Resource Manager-modellen när du har flyttat ExpressRoute-kretsen. 


## <a name="before-you-begin"></a>Innan du börjar
* Kontrollera att du har hello senaste versionen av hello Azure PowerShell-moduler. Mer information finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).
* Kontrollera att du har granskat hello [krav](expressroute-prerequisites.md), [routningskrav](expressroute-routing.md), och [arbetsflöden](expressroute-workflows.md) innan du börjar konfigurera.
* Granska hello information som tillhandahålls under [flytta en ExpressRoute-krets från klassiska tooResource Manager](expressroute-move.md). Kontrollera att du förstår hello gränser och begränsningar.
* Kontrollera att hello kretsen är fullt fungerande i hello klassiska distributionsmodellen.
* Se till att du har en resursgrupp som skapades i hello Resource Manager-distributionsmodellen.
* Granska hello efter resurs-dokumentationen:

    * [Plattform som stöds migrering av IaaS-resurser från klassiska tooAzure Resource Manager](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
    * [Tekniska ingående om plattformen stöds från klassiska tooAzure Resource Manager](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
    * [Vanliga frågor och svar: Plattform som stöds migrering av IaaS-resurser från klassiska tooAzure Resource Manager](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
    * [Granska de vanligaste migreringsfel och åtgärder](../virtual-machines/windows/migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="supported-and-unsupported-scenarios"></a>Scenarier som stöds och som inte stöds

* En ExpressRoute-krets kan flyttas från hello klassiska toohello Resource Manager-miljö utan driftavbrott. Du kan flytta ExpressRoute-krets från hello klassiska toohello Resource Manager-miljö utan avbrott. Följ anvisningarna för hello i [flyttar ExpressRoute-kretsar från hello klassiska toohello Resource Manager-distributionsmodellen med hjälp av PowerShell](expressroute-howto-move-arm.md). Det här är ett virtuellt nätverk för nödvändiga toomove resurser anslutna toohello.
* Virtuella nätverk, gatewayenheter och associerade distributioner inom hello virtuellt nätverk som är anslutna tooan ExpressRoute-krets i samma prenumeration kan vara hello migreras toohello Resource Manager-miljö utan driftavbrott. Du kan följa hello steg beskrivs senare toomigrate resurser, till exempel virtuella nätverk, gateways och virtuella datorer distribueras inom hello virtuellt nätverk. Du måste se till att hello virtuella nätverk är korrekt konfigurerade innan de migreras. 
* Virtuella nätverk, gatewayenheter och associerade distributioner inom hello virtuellt nätverk som inte ingår i hello samma prenumeration som hello ExpressRoute-krets kräver vissa driftstopp toocomplete hello migrering. hello sista avsnittet i hello dokumentet beskriver hello steg toobe följt toomigrate resurser.
* Ett virtuellt nätverk med både ExpressRoute-Gateway och VPN-Gateway kan inte migreras.

## <a name="move-an-expressroute-circuit-from-classic-tooresource-manager"></a>Flytta en ExpressRoute-krets från klassiska tooResource Manager
Du måste flytta en ExpressRoute-krets från hello klassiska toohello Resource Manager-miljön innan du försöker toomigrate resurser som är anslutna toohello ExpressRoute-kretsen. tooaccomplish detta uppgift, se hello följande artiklar:

* Granska hello information som tillhandahålls under [flytta en ExpressRoute-krets från klassiska tooResource Manager](expressroute-move.md).
* [Flytta en krets från klassiska tooResource Manager med Azure PowerShell](expressroute-howto-move-arm.md).
* Använda hello Azure Service Management portal. Du kan följa hello arbetsflöde för[skapa en ny ExpressRoute-krets](expressroute-howto-circuit-portal-resource-manager.md) och välj alternativet för hello import. 

Den här åtgärden innebär inte någon avbrottstid. Du kan fortsätta tootransfer data mellan din lokala och Microsoft medan hello migrering pågår.

## <a name="migrate-virtual-networks-gateways-and-associated-deployments"></a>Migrera associerade distributioner, gateways och virtuella nätverk

hello steg du följer toomigrate beror på om dina resurser finns i hello samma prenumeration, olika prenumerationer eller båda.

### <a name="migrate-virtual-networks-gateways-and-associated-deployments-in-hello-same-subscription-as-hello-expressroute-circuit"></a>Migrera virtuella nätverk, gateways, och associerade distributioner i hello samma prenumeration som hello ExpressRoute-krets
Det här avsnittet beskrivs hello steg toobe följt toomigrate ett virtuellt nätverk, -gateway och associerade distributioner i hello samma prenumeration som hello ExpressRoute-kretsen. Inget driftstopp är associerad med den här migreringen. Du kan fortsätta toouse alla resurser via hello migreringsprocessen. hello management plan är låst medan hello migrering pågår. 

1. Se till att hello ExpressRoute-kretsen har flyttats från hello klassiska toohello Resource Manager-miljön.
2. Se till att det virtuella nätverket hello har förberetts på rätt sätt för hello migrering.
3. Registrera prenumerationen för resursmigrering. tooregister prenumerationen för resursmigrering, Använd hello följande PowerShell-fragment:

  ```powershell 
  Select-AzureRmSubscription -SubscriptionName <Your Subscription Name>
  Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
  Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
  ```
4. Validera, förbereda och migrera. toomove hello virtuellt nätverk, Använd hello följande PowerShell-fragment:

  ```powershell
  Move-AzureVirtualNetwork -Prepare $vnetName  
  Move-AzureVirtualNetwork -Commit $vnetName
  ```

  Du kan också avbryta migreringen genom att köra följande PowerShell-cmdleten hello:

  ```powershell
  Move-AzureVirtualNetwork -Abort $vnetName
  ```

## <a name="next-steps"></a>Nästa steg
* [Plattform som stöds migrering av IaaS-resurser från klassiska tooAzure Resource Manager](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
* [Tekniska ingående om plattformen stöds från klassiska tooAzure Resource Manager](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
* [Vanliga frågor och svar: Plattform som stöds migrering av IaaS-resurser från klassiska tooAzure Resource Manager](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
* [Granska de vanligaste migreringsfel och åtgärder](../virtual-machines/windows/migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
