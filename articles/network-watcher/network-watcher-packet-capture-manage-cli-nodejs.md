---
title: "aaaManage paket som samlar in med Azure Nätverksbevakaren - Azure CLI 1.0 | Microsoft Docs"
description: "Den här sidan förklarar hur toomanage hello paket avbilda funktion i Nätverksbevakaren som använder Azure CLI 1.0"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: cb0c1d10-f7f2-4c34-b08c-f73452430be8
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: c4b710a8d82ccaaf65876a8c2ef845aa97b5f831
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-azure-cli-10"></a>Hantera paket insamlingar med Azure Nätverksbevakaren använder Azure CLI 1.0

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-packet-capture-manage-portal.md)
> - [PowerShell](network-watcher-packet-capture-manage-powershell.md)
> - [CLI 1.0](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [CLI 2.0](network-watcher-packet-capture-manage-cli.md)
> - [Azure REST-API](network-watcher-packet-capture-manage-rest.md)

Nätverket Watcher paketinsamling kan toocreate avbilda sessioner tootrack trafik tooand från en virtuell dator. Filter har angetts för hello avbilda session tooensure du fånga in endast hello trafik som du vill använda. Paketinsamling hjälper toodiagnose nätverk avvikelser reaktivt och proaktivt. Andra användningsområden omfattar att samla in nätverksstatistik får information om nätverket intrång, toodebug klient / server-kommunikation och mycket mer. Den här funktionen underlättar genom kan tooremotely utlösaren paket insamlingar, hello belastningen körs en paketinsamling manuellt och på hello önskade dator som sparar värdefull tid.

Den här artikeln använder plattformsoberoende Azure CLI version 1.0, som är tillgänglig för Windows, Mac och Linux.

Den här artikeln tar dig igenom hello olika administrativa uppgifter som är tillgängliga för paketinsamling.

- [**Starta en paketinsamling**](#start-a-packet-capture)
- [**Stoppa en paketinsamling**](#stop-a-packet-capture)
- [**Ta bort en paketinsamling**](#delete-a-packet-capture)
- [**Hämta en paketinsamling**](#download-a-packet-capture)

## <a name="before-you-begin"></a>Innan du börjar

Den här artikeln förutsätter att du har hello följande resurser:

- En instans Nätverksbevakaren i hello region som du vill toocreate en paketinsamling
- En virtuell dator med hello paket avbilda tillägget aktiverat.

> [!IMPORTANT]
> Paketinsamling kräver en agent toobe som körs på hello virtuell dator. hello Agent installeras som ett tillägg. Anvisningar för VM-tillägg, besök [tillägg för virtuell dator och funktioner](../virtual-machines/windows/extensions-features.md).

## <a name="install-vm-extension"></a>Installera tillägg för virtuell dator

### <a name="step-1"></a>Steg 1

Kör hello `azure vm extension set` cmdlet tooinstall hello paket avbilda agent på hello virtuella gästdatorn.

För Windows-datorer:

```azurecli
azure vm extension set -g resourceGroupName -m virtualMachineName -p Microsoft.Azure.NetworkWatcher -r AzureNetworkWatcherExtension -n NetworkWatcherAgentWindows -o 1.4
```

För Linux virtuella datorer:

```azurecli
azure vm extension set -g resourceGroupName -m virtualMachineName -p Microsoft.Azure.NetworkWatcher -r AzureNetworkWatcherExtension -n NetworkWatcherAgentLinux -o 1.4
````

### <a name="step-2"></a>Steg 2

tooensure som hello agenten är installerad, kör hello `vm extension get` cmdlet och skickar den hello resursgrupp och namn på virtuell dator. Kontrollera hello resulterande listan tooensure hello-agenten är installerad.

```azurecli
azure vm extension get -g resourceGroupName -m virtualMachineName
```

hello följande exempel är ett exempel på hello svar från att köras`azure vm extension get`

```
info:    Executing command vm extension get
+ Looking up hello VM "virtualMachineName"
data:    Publisher                       Name                        Version  State
data:    ------------------------------  -----------------------     -------  ---------
data:    Microsoft.Azure.NetworkWatcher  NetworkWatcherAgentWindows  1.4      Succeeded
info:    vm extension get command OK
```

## <a name="start-a-packet-capture"></a>Starta en paketinsamling

När hello föregående steg är klar kan är hello paket avbilda agenten installerad på hello virtuell dator.

### <a name="step-1"></a>Steg 1

hello nästa steg är tooretrieve hello Nätverksbevakaren instans. Den här variabeln skickas toohello `network watcher show` cmdlet i steg 4.

```azurecli
azure network watcher show -g resourceGroup -n networkWatcherName
```

### <a name="step-2"></a>Steg 2

Hämta ett lagringskonto. Det här lagringskontot är används toostore hello paket filen.

```azurecli
azure storage account list
```

### <a name="step-3"></a>Steg 3

Filter kan vara används toolimit hello data som lagras av hello paketinsamling. hello ställer följande exempel in en paketinsamling med flera filter.  hello tre första filter samlar in utgående TCP-trafik från lokala IP 10.0.0.3 toodestination portarna 20, 80 och 443.  hello senaste filter samlar in UDP-trafik.

```azurecli
azure network watcher packet-capture create -g resourceGroupName -w networkWatcherName -n packetCaptureName -t targetResourceId -o storageAccountResourceId -f "[{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"20\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"80\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"443\"},{\"protocol\":\"UDP\"}]"
```

Flera filter kan definieras för en paketinsamling. Om du använder en komplexa filter struktur är bättre toouse filter som ett json-fil tooavoid syntaxfel. Till exempel använda hello flaggan ”-r” (i stället för ”-f”) och skicka hello platsen för en json-fil som innehåller hello följande filter:

```json
[
    {
        "protocol":"TCP",
        "remoteIPAddress":"1.1.1.1-255.255.255",
        "localIPAddress":"10.0.0.3",
        "remotePort":"20"
    },
    {
        "protocol":"TCP",
        "remoteIPAddress":"1.1.1.1-255.255.255",
        "localIPAddress":"10.0.0.3",
        "remotePort":"80"
    },
    {
        "protocol":"TCP",
        "remoteIPAddress":"1.1.1.1-255.255.255",
        "localIPAddress":"10.0.0.3",
        "remotePort":"443"
    },
    {
        "protocol":"UDP"
    }
]
```


hello följande exempel är hello förväntades utdata från kör hello `network watcher packet-capture create` cmdlet.

```
data:    Name                            : packetCaptureName
data:    Etag                            : W/"d59bb2d2-dc95-43da-b740-e0ef8fcacecb"
data:    Target                          : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Compute/virtualMachines/testVM
data:    Bytes tooCapture Per Packet     : 0
data:    Total Bytes Per Session         : 1073741824
data:    Time Limit In Seconds           : 18000
data:    Storage Location:
data:      Storage Id                    : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Storage/storageAccounts/testStorage
data:      Storage Path                  : https://testStorage.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/testRG/providers/microsoft.compute/virtualmachines/testVM/2017/02/17/packetcapture_01_21_18_145.cap
data:    Filters                         : []
data:    Provisioning State              : Succeeded
info:    network watcher packet-capture create command OK
```

## <a name="get-a-packet-capture"></a>Hämta en paketinsamling

Kör hello `network watcher packet-capture show` cmdlet, hämtar hello status för en paketinsamling som körs eller har slutförts.

```azurecli
azure network watcher packet-capture show -g resourceGroupName -w networkWatcherName -n packetCaptureName
```

hello följande exempel är hello utdata från hello `network watcher packet-capture show` cmdlet. hello följande exempel är när hello hämtningen är klar. Hej PacketCaptureStatus värdet stoppas med en StopReason TimeExceeded. Det här värdet visar att hello paketinsamling lyckades och kördes tiden.

```
data:    Name                            : packetCaptureName
data:    Etag                            : W/"d59bb2d2-dc95-43da-b740-e0ef8fcacecb"
data:    Target                          : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Compute/virtualMachines/testVM
data:    Bytes tooCapture Per Packet     : 0
data:    Total Bytes Per Session         : 1073741824
data:    Time Limit In Seconds           : 18000
data:    Storage Location:
data:      Storage Id                    : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Storage/storageAccounts/testStorage
data:      Storage Path                  : https://testStorage.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/testRG/providers/microsoft.compute/virtualmachines/testVM/2017/02/17/packetcapture_01_21_18_145.cap
data:    Filters                         : []
data:    Provisioning State              : Succeeded
info:    network watcher packet-capture show command OK
```

## <a name="stop-a-packet-capture"></a>Stoppa en paketinsamling

Genom att köra hello `network watcher packet-capture stop` cmdlet, om en avbildningssessionen pågår den har stoppats.

```azurecli
azure network watcher packet-capture stop -g resourceGroupName -w networkWatcherName -n packetCaptureName
```

> [!NOTE]
> hello cmdlet returnerar inget svar när kördes på en pågående avbildningssessionen eller en befintlig session som redan har stoppats.

## <a name="delete-a-packet-capture"></a>Ta bort en paketinsamling

```azurecli
azure network watcher packet-capture delete -g resourceGroupName -w networkWatcherName -n packetCaptureName
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

Hitta om vissa trafik tillåts i eller utanför den virtuella datorn genom att besöka [Kontrollera Kontrollera IP-flöde](network-watcher-check-ip-flow-verify-portal.md)

<!-- Image references -->
