---
title: Kontrollera aaaverify trafik med Azure Network Watcher IP - PowerShell | Microsoft Docs
description: "Den här artikeln beskriver hur toocheck om trafik tooor från en virtuell dator tillåts eller nekas med hjälp av PowerShell"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: e1dad757-8c5d-467f-812e-7cc751143207
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 924da1de1bd554e15816886f8e51d7f170f0e7ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-tooor-from-a-vm-with-ip-flow-verify-a-component-of-azure-network-watcher"></a>Kontrollera om trafik som tillåts eller nekas tooor från en virtuell dator med IP-flöde verifiera en komponent i Azure Nätverksbevakaren

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-check-ip-flow-verify-portal.md)
> - [PowerShell](network-watcher-check-ip-flow-verify-powershell.md)
> - [CLI 1.0](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-ip-flow-verify-cli.md)
> - [Azure REST-API](network-watcher-check-ip-flow-verify-rest.md)


IP-flöde verifiera är en funktion i Nätverksbevakaren som gör att du tooverify om trafik tillåts tooor från en virtuell dator. Det här scenariot är användbart tooget aktuella tillstånd om en virtuell dator kan prata tooan extern resurs eller backend. IP-flöde verifiera går att använda tooverify om Nätverkssäkerhetsgrupp (NSG)-regler är korrekt konfigurerade och felsöka flöden som blockeras av NSG-regler. En annan orsak till att använda IP flödet Kontrollera tooensure trafik som du vill blockerade blockeras korrekt av hello NSG.

## <a name="before-you-begin"></a>Innan du börjar

Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren eller har en befintlig instans av Nätverksbevakaren. hello scenariot förutsätter att en resursgrupp med en giltig virtuell dator finns toobe används.

## <a name="scenario"></a>Scenario

Det här scenariot använder IP-flödet verifiera tooverify om en virtuell dator kan prata tooa kända Bing IP-adress. Om hello trafik nekas returnerar hello säkerhetsregel som nekar trafiken. toolearn mer om IP-flöde Kontrollera finns [IP-flöde Kontrollera översikt](network-watcher-ip-flow-verify-overview.md)

## <a name="retrieve-network-watcher"></a>Hämta Nätverksbevakaren

hello första steget är tooretrieve hello Nätverksbevakaren instans. Hej `$networkWatcher` variabel har skickats toohello IP flödet Kontrollera cmdlet.

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="get-a-vm"></a>Hämta en virtuell dator

IP-flöde verifiera tester trafik tooor från en IP-adress på en virtuell dator tooor från en extern enhet. Ett Id för en virtuell dator krävs för hello cmdlet. Om du redan vet hello-ID för hello virtuella toouse kan du hoppa över det här steget.

```powershell
$VM = Get-AzurermVM -ResourceGroupName "testrg" -Name "testvm1"
```

## <a name="get-hello-nics"></a>Hämta hello nätverkskort

hello IP-adress till ett nätverkskort på den virtuella datorn hello krävs i det här exemplet vi hämta hello nätverkskort på en virtuell dator. Om du redan vet hello IP-adress som du vill tootest på hello virtuell dator kan du hoppa över det här steget.

```powershell
$Nics = Get-AzureRmNetworkInterface | Where {$_.Id -eq $vm.NetworkProfile.NetworkInterfaces.Id.ForEach({$_})}
```

## <a name="run-ip-flow-verify"></a>Kontrollera kör IP-flöde

Nu när vi har hello information behövs toorun hello cmdlet vi kör hello `Test-AzureRmNetworkWatcherIPFlow` cmdlet tootest hello trafik. I det här exemplet använder vi hello första IP-adressen på hello första nätverkskortet.

```powershell
Test-AzureRmNetworkWatcherIPFlow -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id `
-Direction Outbound -Protocol TCP `
-LocalIPAddress $nics[0].IpConfigurations[0].PrivateIpAddress -LocalPort 6895 -RemoteIPAddress 204.79.197.200 -RemotePort 80
```

> [!NOTE]
> IP-flöde Kontrollera kräver att hello Virtuella datorresursen fördelas toorun.

## <a name="review-results"></a>Granska resultaten

När du har kört `Test-AzureRmNetworkWatcherIPFlow` hello resultaten returneras, hello följande exempel är hello resultaten som returnerades från hello föregående steg.

```
Access RuleName                                  
------ --------                                  
Allow  defaultSecurityRules/AllowInternetOutBound
```

## <a name="next-steps"></a>Nästa steg

Om trafik blockeras och får inte vara, se [hantera Nätverkssäkerhetsgrupper](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack hello network security grupp och säkerhet regler som har definierats.

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png













