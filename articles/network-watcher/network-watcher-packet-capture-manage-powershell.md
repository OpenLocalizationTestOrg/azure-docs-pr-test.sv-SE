---
title: "aaaManage paket som samlar in med Azure Nätverksbevakaren - PowerShell | Microsoft Docs"
description: "Den här sidan förklarar hur toomanage hello paket avbilda funktion i Nätverksbevakaren med hjälp av PowerShell"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 04d82085-c9ea-4ea1-b050-a3dd4960f3aa
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 77a522a1b05e020a73ba7140c1410615eb8761da
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-powershell"></a>Hantera paket insamlingar med Azure Nätverksbevakaren med hjälp av PowerShell

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-packet-capture-manage-portal.md)
> - [PowerShell](network-watcher-packet-capture-manage-powershell.md)
> - [CLI 1.0](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [CLI 2.0](network-watcher-packet-capture-manage-cli.md)
> - [Azure REST-API](network-watcher-packet-capture-manage-rest.md)

Nätverket Watcher paketinsamling kan toocreate avbilda sessioner tootrack trafik tooand från en virtuell dator. Filter har angetts för hello avbilda session tooensure du fånga in endast hello trafik som du vill använda. Paketinsamling hjälper toodiagnose nätverk avvikelser reaktivt och proaktivt. Andra användningsområden omfattar att samla in nätverksstatistik får information om nätverket intrång, toodebug klient / server-kommunikation och mycket mer. Den här funktionen underlättar genom kan tooremotely utlösaren paket insamlingar, hello belastningen körs en paketinsamling manuellt och på hello önskade dator som sparar värdefull tid.

Den här artikeln tar dig igenom hello olika administrativa uppgifter som är tillgängliga för paketinsamling.

- [**Starta en paketinsamling**](#start-a-packet-capture)
- [**Stoppa en paketinsamling**](#stop-a-packet-capture)
- [**Ta bort en paketinsamling**](#delete-a-packet-capture)
- [**Hämta en paketinsamling**](#download-a-packet-capture)

## <a name="before-you-begin"></a>Innan du börjar

Den här artikeln förutsätter att du har hello följande resurser:

* En instans Nätverksbevakaren i hello region som du vill toocreate en paketinsamling

* En virtuell dator med hello paket avbilda tillägget aktiverat.

> [!IMPORTANT]
> Paketinsamling kräver ett tillägg för virtuell dator `AzureNetworkWatcherExtension`. Installera hello tillägg på en Windows VM finns [tillägg för virtuell dator i Azure Network Watcher Agent för Windows](../virtual-machines/windows/extensions-nwa.md) och för Linux VM besöka [tillägg för virtuell dator i Azure Network Watcher Agent för Linux](../virtual-machines/linux/extensions-nwa.md).

## <a name="install-vm-extension"></a>Installera tillägg för virtuell dator

### <a name="step-1"></a>Steg 1

```powershell
$VM = Get-AzureRmVM -ResourceGroupName testrg -Name VM1
```

### <a name="step-2"></a>Steg 2

hello följande exempel hämtar hello tillägget information behövs toorun hello `Set-AzureRmVMExtension` cmdlet. Denna cmdlet installerar hello paket avbilda agent på hello virtuella gästdatorn.

> [!NOTE]
> Hej `Set-AzureRmVMExtension` cmdlet kan ta flera minuter toocomplete.

För Windows-datorer:

```powershell
$AzureNetworkWatcherExtension = Get-AzureRmVMExtensionImage -Location WestCentralUS -PublisherName Microsoft.Azure.NetworkWatcher -Type NetworkWatcherAgentWindows -Version 1.4.13.0
$ExtensionName = "AzureNetworkWatcherExtension"
Set-AzureRmVMExtension -ResourceGroupName $VM.ResourceGroupName  -Location $VM.Location -VMName $VM.Name -Name $ExtensionName -Publisher $AzureNetworkWatcherExtension.PublisherName -ExtensionType $AzureNetworkWatcherExtension.Type -TypeHandlerVersion $AzureNetworkWatcherExtension.Version.Substring(0,3)
```

För Linux virtuella datorer:

```powershell
$AzureNetworkWatcherExtension = Get-AzureRmVMExtensionImage -Location WestCentralUS -PublisherName Microsoft.Azure.NetworkWatcher -Type NetworkWatcherAgentLinux -Version 1.4.13.0
$ExtensionName = "AzureNetworkWatcherExtension"
Set-AzureRmVMExtension -ResourceGroupName $VM.ResourceGroupName  -Location $VM.Location -VMName $VM.Name -Name $ExtensionName -Publisher $AzureNetworkWatcherExtension.PublisherName -ExtensionType $AzureNetworkWatcherExtension.Type -TypeHandlerVersion $AzureNetworkWatcherExtension.Version.Substring(0,3)
````

hello följande exempel är ett lyckat svar när du har kört hello `Set-AzureRmVMExtension` cmdlet.

```
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   
```

### <a name="step-3"></a>Steg 3

tooensure som hello agenten är installerad, kör hello `Get-AzureRmVMExtension` cmdlet och skickar den hello virtuella datornamn och hello Tilläggsnamn.

```powershell
Get-AzureRmVMExtension -ResourceGroupName $VM.ResourceGroupName  -VMName $VM.Name -Name $ExtensionName
```

hello följande exempel är ett exempel på hello svar från att köras`Get-AzureRmVMExtension`

```
ResourceGroupName       : testrg
VMName                  : testvm1
Name                    : AzureNetworkWatcherExtension
Location                : westcentralus
Etag                    : null
Publisher               : Microsoft.Azure.NetworkWatcher
ExtensionType           : NetworkWatcherAgentWindows
TypeHandlerVersion      : 1.4
Id                      : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Compute/virtualMachines/testvm1/
                          extensions/AzureNetworkWatcherExtension
PublicSettings          : 
ProtectedSettings       : 
ProvisioningState       : Succeeded
Statuses                : 
SubStatuses             : 
AutoUpgradeMinorVersion : True
ForceUpdateTag          : 
```

## <a name="start-a-packet-capture"></a>Starta en paketinsamling

När hello föregående steg är klar kan är hello paket avbilda agenten installerad på hello virtuell dator.

### <a name="step-1"></a>Steg 1

hello nästa steg är tooretrieve hello Nätverksbevakaren instans. Den här variabeln skickas toohello `New-AzureRmNetworkWatcherPacketCapture` cmdlet i steg 4.

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName  
```

### <a name="step-2"></a>Steg 2

Hämta ett lagringskonto. Det här lagringskontot är används toostore hello paket filen.

```powershell
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName testrg -Name testrgsa123
```

### <a name="step-3"></a>Steg 3

Filter kan vara används toolimit hello data som lagras av hello paketinsamling. hello ställer följande exempel in två filter.  Ett filter samlar in utgående TCP-trafik från lokala IP 10.0.0.3 toodestination portarna 20, 80 och 443.  hello andra filtret samlar in UDP-trafik.

```powershell
$filter1 = New-AzureRmPacketCaptureFilterConfig -Protocol TCP -RemoteIPAddress "1.1.1.1-255.255.255" -LocalIPAddress "10.0.0.3" -LocalPort "1-65535" -RemotePort "20;80;443"
$filter2 = New-AzureRmPacketCaptureFilterConfig -Protocol UDP
```

> [!NOTE]
> Flera filter kan definieras för en paketinsamling.

### <a name="step-4"></a>Steg 4

Kör hello `New-AzureRmNetworkWatcherPacketCapture` cmdlet toostart hello paket processen, skicka hello krävs värden hämtas i hello föregående steg.
```powershell

New-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -TargetVirtualMachineId $vm.Id -PacketCaptureName "PacketCaptureTest" -StorageAccountId $storageAccount.id -TimeLimitInSeconds 60 -Filters $filter1, $filter2
```

hello följande exempel är hello förväntades utdata från kör hello `New-AzureRmNetworkWatcherPacketCapture` cmdlet.

```
Name                    : PacketCaptureTest
Id                      : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatcher
                          s/NetworkWatcher_westcentralus/packetCaptures/PacketCaptureTest
Etag                    : W/"3bf27278-8251-4651-9546-c7f369855e4e"
ProvisioningState       : Succeeded
Target                  : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Compute/virtualMachines/testvm1
BytesToCapturePerPacket : 0
TotalBytesPerSession    : 1073741824
TimeLimitInSeconds      : 60
StorageLocation         : {
                            "StorageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Storage/storageA
                          ccounts/examplestorage",
                            "StoragePath": "https://examplestorage.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-00000
                          0000000/resourcegroups/testrg/providers/microsoft.compute/virtualmachines/testvm1/2017/02/01/packetcapture_22_42_48_238.cap"
                          }
Filters                 : [
                            {
                              "Protocol": "TCP",
                              "RemoteIPAddress": "1.1.1.1-255.255.255",
                              "LocalIPAddress": "10.0.0.3",
                              "LocalPort": "1-65535",
                              "RemotePort": "20;80;443"
                            },
                            {
                              "Protocol": "UDP",
                              "RemoteIPAddress": "",
                              "LocalIPAddress": "",
                              "LocalPort": "",
                              "RemotePort": ""
                            }
                          ]


```

## <a name="get-a-packet-capture"></a>Hämta en paketinsamling

Kör hello `Get-AzureRmNetworkWatcherPacketCapture` cmdlet, hämtar hello status för en paketinsamling som körs eller har slutförts.

```powershell
Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
```

hello följande exempel är hello utdata från hello `Get-AzureRmNetworkWatcherPacketCapture` cmdlet. hello följande exempel är när hello hämtningen är klar. Hej PacketCaptureStatus värdet stoppas med en StopReason TimeExceeded. Det här värdet visar att hello paketinsamling lyckades och kördes tiden.
```
Name                    : PacketCaptureTest
Id                      : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatcher
                          s/NetworkWatcher_westcentralus/packetCaptures/PacketCaptureTest
Etag                    : W/"4b9a81ed-dc63-472e-869e-96d7166ccb9b"
ProvisioningState       : Succeeded
Target                  : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Compute/virtualMachines/testvm1
BytesToCapturePerPacket : 0
TotalBytesPerSession    : 1073741824
TimeLimitInSeconds      : 60
StorageLocation         : {
                            "StorageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Storage/storageA
                          ccounts/examplestorage",
                            "StoragePath": "https://examplestorage.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-00000
                          0000000/resourcegroups/testrg/providers/microsoft.compute/virtualmachines/testvm1/2017/02/01/packetcapture_22_42_48_238.cap"
                          }
Filters                 : [
                            {
                              "Protocol": "TCP",
                              "RemoteIPAddress": "1.1.1.1-255.255.255",
                              "LocalIPAddress": "10.0.0.3",
                              "LocalPort": "1-65535",
                              "RemotePort": "20;80;443"
                            },
                            {
                              "Protocol": "UDP",
                              "RemoteIPAddress": "",
                              "LocalIPAddress": "",
                              "LocalPort": "",
                              "RemotePort": ""
                            }
                          ]
CaptureStartTime        : 2/1/2017 10:43:01 PM
PacketCaptureStatus     : Stopped
StopReason              : TimeExceeded
PacketCaptureError      : []
```

## <a name="stop-a-packet-capture"></a>Stoppa en paketinsamling

Genom att köra hello `Stop-AzureRmNetworkWatcherPacketCapture` cmdlet, om en avbildningssessionen pågår den har stoppats.

```powershell
Stop-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
```

> [!NOTE]
> hello cmdlet returnerar inget svar när kördes på en pågående avbildningssessionen eller en befintlig session som redan har stoppats.

## <a name="delete-a-packet-capture"></a>Ta bort en paketinsamling

```powershell
Remove-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
```

> [!NOTE]
> Om du tar bort en paketinsamling tar inte bort hello-filen i hello storage-konto.

## <a name="download-a-packet-capture"></a>Hämta en paketinsamling

När paketet avbildningssessionen är klar kan hello avbilda filen vara överförda tooblob lagring eller tooa lokal fil på hello VM. hello lagringsplats för hello paketinsamling har definierats vid skapande av hello. En lämplig verktyget tooaccess dessa avbilda filer sparade tooa storage-konto är Microsoft Azure Lagringsutforskaren, som kan hämtas här: http://storageexplorer.com/

Om ett lagringskonto har angetts sparas paket avbilda filer tooa storage-konto på hello följande plats:

```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a>Nästa steg

Lär dig hur fångar tooautomate paket med virtuella aviseringar genom att visa [skapar en avisering utlösta paketinsamling](network-watcher-alert-triggered-packet-capture.md)

Hitta om vissa trafik tillåts i orr utanför den virtuella datorn genom att besöka [Kontrollera Kontrollera IP-flöde](network-watcher-check-ip-flow-verify-portal.md)

<!-- Image references -->














