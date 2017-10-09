---
title: "aaaManage paket som samlar in med Azure Nätverksbevakaren - Azure CLI 2.0 | Microsoft Docs"
description: "Den här sidan förklarar hur toomanage hello paket avbilda funktion i Nätverksbevakaren som använder Azure CLI 2.0"
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
ms.openlocfilehash: d19cb7d0ca3b7a9bc0546859e07ef6d4df4f42d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-azure-cli-20"></a>Hantera paket insamlingar med Azure Nätverksbevakaren använder Azure CLI 2.0

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-packet-capture-manage-portal.md)
> - [PowerShell](network-watcher-packet-capture-manage-powershell.md)
> - [CLI 1.0](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [CLI 2.0](network-watcher-packet-capture-manage-cli.md)
> - [Azure REST-API](network-watcher-packet-capture-manage-rest.md)

Nätverket Watcher paketinsamling kan toocreate avbilda sessioner tootrack trafik tooand från en virtuell dator. Filter har angetts för hello avbilda session tooensure du fånga in endast hello trafik som du vill använda. Paketinsamling hjälper toodiagnose nätverk avvikelser reaktivt och proaktivt. Andra användningsområden omfattar att samla in nätverksstatistik får information om nätverket intrång, toodebug klient / server-kommunikation och mycket mer. Den här funktionen underlättar genom kan tooremotely utlösaren paket insamlingar, hello belastningen körs en paketinsamling manuellt och på hello önskade dator som sparar värdefull tid.

Den här artikeln använder våra nästa generations CLI för hello management resursdistributionsmodell, Azure CLI 2.0, som är tillgänglig för Windows, Mac och Linux.

tooperform hello stegen i den här artikeln, måste du för[installerar hello Azure-kommandoradsgränssnittet för Mac, Linux och Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

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

Kör hello `az vm extension set` cmdlet tooinstall hello paket avbilda agent på hello virtuella gästdatorn.

För Windows-datorer:

```azurecli
az vm extension set --resource-group resourceGroupName --vm-name virtualMachineName --publisher Microsoft.Azure.NetworkWatcher --name NetworkWatcherAgentWindows --version 1.4
```

För Linux virtuella datorer:

```azurecli
az vm extension set --resource-group resourceGroupName --vm-name virtualMachineName --publisher Microsoft.Azure.NetworkWatcher --name NetworkWatcherAgentLinux--version 1.4
````

### <a name="step-2"></a>Steg 2

tooensure som hello agenten är installerad, kör hello `vm extension get` cmdlet och skickar den hello resursgrupp och namn på virtuell dator. Kontrollera hello resulterande listan tooensure hello-agenten är installerad.

```azurecli
az vm extension show -resource-group resourceGroupName --vm-name virtualMachineName --name NetworkWatcherAgentWindows
```

hello följande exempel är ett exempel på hello svar från att köras`az vm extension show`

```json
{
  "autoUpgradeMinorVersion": true,
  "forceUpdateTag": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}/extensions/NetworkWatcherAgentWindows",
  "instanceView": null,
  "location": "westcentralus",
  "name": "NetworkWatcherAgentWindows",
  "protectedSettings": null,
  "provisioningState": "Succeeded",
  "publisher": "Microsoft.Azure.NetworkWatcher",
  "resourceGroup": "{resourceGroupName}",
  "settings": null,
  "tags": null,
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "typeHandlerVersion": "1.4",
  "virtualMachineExtensionType": "NetworkWatcherAgentWindows"
}
```

## <a name="start-a-packet-capture"></a>Starta en paketinsamling

När hello föregående steg är klar kan är hello paket avbilda agenten installerad på hello virtuell dator.

### <a name="step-1"></a>Steg 1

hello nästa steg är tooretrieve hello Nätverksbevakaren instans. Den här namnet på hello Nätverksbevakaren skickas toohello `az network watcher show` cmdlet i steg 4.

```azurecli
az network watcher show -resource-group resourceGroup -name networkWatcherName
```

### <a name="step-2"></a>Steg 2

Hämta ett lagringskonto. Det här lagringskontot är används toostore hello paket filen.

```azurecli
azure storage account list
```

### <a name="step-3"></a>Steg 3

Filter kan vara används toolimit hello data som lagras av hello paketinsamling. hello ställer följande exempel in en paketinsamling med flera filter.  hello tre första filter samlar in utgående TCP-trafik från lokala IP 10.0.0.3 toodestination portarna 20, 80 och 443.  hello senaste filter samlar in UDP-trafik.

```azurecli
az network watcher packet-capture create --resource-group {resoureceurceGroupName} --vm {vmName} --name packetCaptureName --storage-account gwteststorage123abc --filters "[{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"20\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"80\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"443\"},{\"protocol\":\"UDP\"}]"
```

hello följande exempel är hello förväntades utdata från kör hello `az network watcher packet-capture create` cmdlet.

```json
{
  "bytesToCapturePerPacket": 0,
  "etag": "W/\"b8cf3528-2e14-45cb-a7f3-5712ffb687ac\"",
  "filters": [
    {
      "localIpAddress": "10.0.0.3",
      "localPort": "",
      "protocol": "TCP",
      "remoteIpAddress": "1.1.1.1-255.255.255",
      "remotePort": "20"
    },
    {
      "localIpAddress": "10.0.0.3",
      "localPort": "",
      "protocol": "TCP",
      "remoteIpAddress": "1.1.1.1-255.255.255",
      "remotePort": "80"
    },
    {
      "localIpAddress": "10.0.0.3",
      "localPort": "",
      "protocol": "TCP",
      "remoteIpAddress": "1.1.1.1-255.255.255",
      "remotePort": "443"
    },
    {
      "localIpAddress": "",
      "localPort": "",
      "protocol": "UDP",
      "remoteIpAddress": "",
      "remotePort": ""
    }
  ],
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/pa
cketCaptures/packetCaptureName",
  "name": "packetCaptureName",
  "provisioningState": "Succeeded",
  "resourceGroup": "NetworkWatcherRG",
  "storageLocation": {
    "filePath": null,
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/gwteststorage123abc",
    "storagePath": "https://gwteststorage123abc.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/{resourceGroupName}/p
roviders/microsoft.compute/virtualmachines/{vmName}/2017/05/25/packetcapture_16_22_34_630.cap"
  },
  "target": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}",
  "timeLimitInSeconds": 18000,
  "totalBytesPerSession": 1073741824
}
```

## <a name="get-a-packet-capture"></a>Hämta en paketinsamling

Kör hello `az network watcher packet-capture show` cmdlet, hämtar hello status för en paketinsamling som körs eller har slutförts.

```azurecli
az network watcher packet-capture show --name packetCaptureName --location westcentralus
```

hello följande exempel är hello utdata från hello `az network watcher packet-capture show` cmdlet. hello följande exempel är när hello hämtningen är klar. Hej PacketCaptureStatus värdet stoppas med en StopReason TimeExceeded. Det här värdet visar att hello paketinsamling lyckades och kördes tiden.

```
{
  "bytesToCapturePerPacket": 0,
  "etag": "W/\"b8cf3528-2e14-45cb-a7f3-5712ffb687ac\"",
  "filters": [
    {
      "localIpAddress": "10.0.0.3",
      "localPort": "",
      "protocol": "TCP",
      "remoteIpAddress": "1.1.1.1-255.255.255",
      "remotePort": "20"
    },
    {
      "localIpAddress": "10.0.0.3",
      "localPort": "",
      "protocol": "TCP",
      "remoteIpAddress": "1.1.1.1-255.255.255",
      "remotePort": "80"
    },
    {
      "localIpAddress": "10.0.0.3",
      "localPort": "",
      "protocol": "TCP",
      "remoteIpAddress": "1.1.1.1-255.255.255",
      "remotePort": "443"
    },
    {
      "localIpAddress": "",
      "localPort": "",
      "protocol": "UDP",
      "remoteIpAddress": "",
      "remotePort": ""
    }
  ],
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/packetCaptures/packetCaptureName",
  "name": "packetCaptureName",
  "provisioningState": "Succeeded",
  "resourceGroup": "NetworkWatcherRG",
  "storageLocation": {
    "filePath": null,
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/gwteststorage123abc",
    "storagePath": "https://gwteststorage123abc.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/{resourceGroupName}/providers/microsoft.compute/virtualmachines/{vmName}/2017/05/25/packetcapt
ure_16_22_34_630.cap"
  },
  "target": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}",
  "timeLimitInSeconds": 18000,
  "totalBytesPerSession": 1073741824
}
```

## <a name="stop-a-packet-capture"></a>Stoppa en paketinsamling

Genom att köra hello `az network watcher packet-capture stop` cmdlet, om en avbildningssessionen pågår den har stoppats.

```azurecli
az network watcher packet-capture stop --name packetCaptureName --location westcentralus
```

> [!NOTE]
> hello cmdlet returnerar inget svar när kördes på en pågående avbildningssessionen eller en befintlig session som redan har stoppats.

## <a name="delete-a-packet-capture"></a>Ta bort en paketinsamling

```azurecli
az network watcher packet-capture delete --name packetCaptureName --location westcentralus
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
