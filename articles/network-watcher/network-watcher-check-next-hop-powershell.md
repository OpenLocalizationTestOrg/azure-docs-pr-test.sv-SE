---
title: "aaaFind nästa hopp med Azure Network Watcher nexthop - PowerShell | Microsoft Docs"
description: "Den här artikeln beskriver hur du kan hitta vad hello nästa hopptyp är och IP-adress med hjälp av nästa hopp med hjälp av PowerShell."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 6a656c55-17bd-40f1-905d-90659087639c
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: fdb0b4a02d95fc45c103fe952fc1afa095414c18
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="find-out-what-hello-next-hop-type-is-using-hello-next-hop-capability-in-azure-network-watcher-using-powershell"></a>Ta reda på vilka hello nästa hopptyp med hello nexthop-funktionen i Azure Nätverksbevakaren med hjälp av PowerShell

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-check-next-hop-portal.md)
> - [PowerShell](network-watcher-check-next-hop-powershell.md)
> - [CLI 1.0](network-watcher-check-next-hop-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-next-hop-cli.md)
> - [Azure REST-API](network-watcher-check-next-hop-rest.md)

Nästa hopp är en funktion i Nätverksbevakaren som ger hello möjlighet hämta hello nästa hopptyp och IP-adress baserat på en angiven virtuell dator. Den här funktionen är användbart för att fastställa om trafik som lämnar en virtuell dator som passerar en gateway, internet eller virtuella nätverk tooget tooits mål.

## <a name="before-you-begin"></a>Innan du börjar

I det här scenariot använder du hello Azure portal toofind hello nästa hopptyp och IP-adress.

Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren. hello scenariot förutsätter att en resursgrupp med en giltig virtuell dator finns toobe används.

## <a name="scenario"></a>Scenario

hello-scenario som beskrivs i den här artikeln används nästa hopp, en funktion i Nätverksbevakaren som söker efter hello nästa hopptyp och IP-adress för en resurs. Besök toolearn mer om nästa hopp [nästa hopp översikt](network-watcher-next-hop-overview.md).

## <a name="retrieve-network-watcher"></a>Hämta Nätverksbevakaren

hello första steget är tooretrieve hello Nätverksbevakaren instans. Hej `$networkWatcher` variabel har skickats toohello nästa hopp Kontrollera cmdlet.

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="get-a-virtual-machine"></a>Hämta en virtuell dator

Nästa hopp returnerar hello nästa hopp och hello IP-adressen för nästa hopp för hello från en virtuell dator. Ett Id för en virtuell dator krävs för hello cmdlet. Om du redan vet hello-ID för hello virtuella toouse kan du hoppa över det här steget.

```powershell
$VM = Get-AzurermVM -ResourceGroupName "testrg" -Name "testvm1"
```

> [!NOTE]
> Nästa hopp kräver att hello Virtuella datorresursen fördelas toorun.

## <a name="get-hello-network-interfaces"></a>Hämta hello nätverksgränssnitt

hello IP-adress till ett nätverkskort på den virtuella datorn hello krävs i det här exemplet vi hämta hello nätverkskort på en virtuell dator. Om du redan vet hello IP-adress som du vill tootest på hello virtuell dator kan du hoppa över det här steget.

```powershell
$Nics = Get-AzureRmNetworkInterface | Where {$_.Id -eq $vm.NetworkProfile.NetworkInterfaces.Id.ForEach({$_})}
```

## <a name="get-next-hop"></a>Hämta nästa hopp

Nu vi kallar hello `Get-AzureRmNetworkWatcherNextHop` cmdlet. Vi skickar hello cmdlet hello Nätverksbevakaren, virtuella Id, käll-IP-adress och mål-IP-adress. I det här exemplet är IP-måladress hello tooa VM i ett annat virtuellt nätverk. Det finns en virtuell nätverksgateway mellan hello två virtuella nätverk.

```powershell
Get-AzureRmNetworkWatcherNextHop -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id -SourceIPAddress $nics[0].IpConfigurations[0].PrivateIpAddress  -DestinationIPAddress 10.0.2.4 
```

## <a name="review-results"></a>Granska resultaten

När du är färdig tillhandahålls hello resultat. hello nästa hopp IP-adress returneras samt hello typ av resurs som det är. I det här scenariot är det hello offentliga IP-adress hello virtuell nätverksgateway.

```
NextHopIpAddress NextHopType           RouteTableId 
---------------- -----------           ------------ 
13.78.238.92     VirtualNetworkGateway Gateway Route
```

hello visar följande lista hello tillgängliga NextHopType värden:

**Nästa Hopptyp**

* Internet
* VirtualAppliance
* VirtualNetworkGateway
* VnetLocal
* HyperNetGateway
* VnetPeering
* Ingen

## <a name="next-steps"></a>Nästa steg

Lär dig hur tooreview säkerhet grupp nätverksinställningarna via programmering genom att besöka [NSG granskning med Nätverksbevakaren](network-watcher-nsg-auditing-powershell.md)

















