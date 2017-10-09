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
# <a name="manage-packet-captures-with-azure-network-watcher-using-azure-cli-20"></a><span data-ttu-id="080e0-103">Hantera paket insamlingar med Azure Nätverksbevakaren använder Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="080e0-103">Manage packet captures with Azure Network Watcher using Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="080e0-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="080e0-104">Azure portal</span></span>](network-watcher-packet-capture-manage-portal.md)
> - [<span data-ttu-id="080e0-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="080e0-105">PowerShell</span></span>](network-watcher-packet-capture-manage-powershell.md)
> - [<span data-ttu-id="080e0-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="080e0-106">CLI 1.0</span></span>](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [<span data-ttu-id="080e0-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="080e0-107">CLI 2.0</span></span>](network-watcher-packet-capture-manage-cli.md)
> - [<span data-ttu-id="080e0-108">Azure REST-API</span><span class="sxs-lookup"><span data-stu-id="080e0-108">Azure REST API</span></span>](network-watcher-packet-capture-manage-rest.md)

<span data-ttu-id="080e0-109">Nätverket Watcher paketinsamling kan toocreate avbilda sessioner tootrack trafik tooand från en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="080e0-109">Network Watcher packet capture allows you toocreate capture sessions tootrack traffic tooand from a virtual machine.</span></span> <span data-ttu-id="080e0-110">Filter har angetts för hello avbilda session tooensure du fånga in endast hello trafik som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="080e0-110">Filters are provided for hello capture session tooensure you capture only hello traffic you want.</span></span> <span data-ttu-id="080e0-111">Paketinsamling hjälper toodiagnose nätverk avvikelser reaktivt och proaktivt.</span><span class="sxs-lookup"><span data-stu-id="080e0-111">Packet capture helps toodiagnose network anomalies both reactively and proactively.</span></span> <span data-ttu-id="080e0-112">Andra användningsområden omfattar att samla in nätverksstatistik får information om nätverket intrång, toodebug klient / server-kommunikation och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="080e0-112">Other uses include gathering network statistics, gaining information on network intrusions, toodebug client-server communications and much more.</span></span> <span data-ttu-id="080e0-113">Den här funktionen underlättar genom kan tooremotely utlösaren paket insamlingar, hello belastningen körs en paketinsamling manuellt och på hello önskade dator som sparar värdefull tid.</span><span class="sxs-lookup"><span data-stu-id="080e0-113">By being able tooremotely trigger packet captures, this capability eases hello burden of running a packet capture manually and on hello desired machine, which saves valuable time.</span></span>

<span data-ttu-id="080e0-114">Den här artikeln använder våra nästa generations CLI för hello management resursdistributionsmodell, Azure CLI 2.0, som är tillgänglig för Windows, Mac och Linux.</span><span class="sxs-lookup"><span data-stu-id="080e0-114">This article uses our next generation CLI for hello resource management deployment model, Azure CLI 2.0, which is available for Windows, Mac and Linux.</span></span>

<span data-ttu-id="080e0-115">tooperform hello stegen i den här artikeln, måste du för[installerar hello Azure-kommandoradsgränssnittet för Mac, Linux och Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="080e0-115">tooperform hello steps in this article, you need too[install hello Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

<span data-ttu-id="080e0-116">Den här artikeln tar dig igenom hello olika administrativa uppgifter som är tillgängliga för paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="080e0-116">This article takes you through hello different management tasks that are currently available for packet capture.</span></span>

- [<span data-ttu-id="080e0-117">**Starta en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="080e0-117">**Start a packet capture**</span></span>](#start-a-packet-capture)
- [<span data-ttu-id="080e0-118">**Stoppa en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="080e0-118">**Stop a packet capture**</span></span>](#stop-a-packet-capture)
- [<span data-ttu-id="080e0-119">**Ta bort en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="080e0-119">**Delete a packet capture**</span></span>](#delete-a-packet-capture)
- [<span data-ttu-id="080e0-120">**Hämta en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="080e0-120">**Download a packet capture**</span></span>](#download-a-packet-capture)

## <a name="before-you-begin"></a><span data-ttu-id="080e0-121">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="080e0-121">Before you begin</span></span>

<span data-ttu-id="080e0-122">Den här artikeln förutsätter att du har hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="080e0-122">This article assumes you have hello following resources:</span></span>

- <span data-ttu-id="080e0-123">En instans Nätverksbevakaren i hello region som du vill toocreate en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="080e0-123">An instance of Network Watcher in hello region you want toocreate a packet capture</span></span>
- <span data-ttu-id="080e0-124">En virtuell dator med hello paket avbilda tillägget aktiverat.</span><span class="sxs-lookup"><span data-stu-id="080e0-124">A virtual machine with hello packet capture extension enabled.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="080e0-125">Paketinsamling kräver en agent toobe som körs på hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="080e0-125">Packet capture requires an agent toobe running on hello virtual machine.</span></span> <span data-ttu-id="080e0-126">hello Agent installeras som ett tillägg.</span><span class="sxs-lookup"><span data-stu-id="080e0-126">hello Agent is installed as an extension.</span></span> <span data-ttu-id="080e0-127">Anvisningar för VM-tillägg, besök [tillägg för virtuell dator och funktioner](../virtual-machines/windows/extensions-features.md).</span><span class="sxs-lookup"><span data-stu-id="080e0-127">For instructions on VM extensions, visit [Virtual Machine extensions and features](../virtual-machines/windows/extensions-features.md).</span></span>

## <a name="install-vm-extension"></a><span data-ttu-id="080e0-128">Installera tillägg för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="080e0-128">Install VM extension</span></span>

### <a name="step-1"></a><span data-ttu-id="080e0-129">Steg 1</span><span class="sxs-lookup"><span data-stu-id="080e0-129">Step 1</span></span>

<span data-ttu-id="080e0-130">Kör hello `az vm extension set` cmdlet tooinstall hello paket avbilda agent på hello virtuella gästdatorn.</span><span class="sxs-lookup"><span data-stu-id="080e0-130">Run hello `az vm extension set` cmdlet tooinstall hello packet capture agent on hello guest virtual machine.</span></span>

<span data-ttu-id="080e0-131">För Windows-datorer:</span><span class="sxs-lookup"><span data-stu-id="080e0-131">For Windows virtual machines:</span></span>

```azurecli
az vm extension set --resource-group resourceGroupName --vm-name virtualMachineName --publisher Microsoft.Azure.NetworkWatcher --name NetworkWatcherAgentWindows --version 1.4
```

<span data-ttu-id="080e0-132">För Linux virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="080e0-132">For Linux virtual machines:</span></span>

```azurecli
az vm extension set --resource-group resourceGroupName --vm-name virtualMachineName --publisher Microsoft.Azure.NetworkWatcher --name NetworkWatcherAgentLinux--version 1.4
````

### <a name="step-2"></a><span data-ttu-id="080e0-133">Steg 2</span><span class="sxs-lookup"><span data-stu-id="080e0-133">Step 2</span></span>

<span data-ttu-id="080e0-134">tooensure som hello agenten är installerad, kör hello `vm extension get` cmdlet och skickar den hello resursgrupp och namn på virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="080e0-134">tooensure that hello agent is installed, run hello `vm extension get` cmdlet and pass it hello resource group and virtual machine name.</span></span> <span data-ttu-id="080e0-135">Kontrollera hello resulterande listan tooensure hello-agenten är installerad.</span><span class="sxs-lookup"><span data-stu-id="080e0-135">Check hello resulting list tooensure hello agent is installed.</span></span>

```azurecli
az vm extension show -resource-group resourceGroupName --vm-name virtualMachineName --name NetworkWatcherAgentWindows
```

<span data-ttu-id="080e0-136">hello följande exempel är ett exempel på hello svar från att köras`az vm extension show`</span><span class="sxs-lookup"><span data-stu-id="080e0-136">hello following sample is an example of hello response from running `az vm extension show`</span></span>

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

## <a name="start-a-packet-capture"></a><span data-ttu-id="080e0-137">Starta en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="080e0-137">Start a packet capture</span></span>

<span data-ttu-id="080e0-138">När hello föregående steg är klar kan är hello paket avbilda agenten installerad på hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="080e0-138">Once hello preceding steps are complete, hello packet capture agent is installed on hello virtual machine.</span></span>

### <a name="step-1"></a><span data-ttu-id="080e0-139">Steg 1</span><span class="sxs-lookup"><span data-stu-id="080e0-139">Step 1</span></span>

<span data-ttu-id="080e0-140">hello nästa steg är tooretrieve hello Nätverksbevakaren instans.</span><span class="sxs-lookup"><span data-stu-id="080e0-140">hello next step is tooretrieve hello Network Watcher instance.</span></span> <span data-ttu-id="080e0-141">Den här namnet på hello Nätverksbevakaren skickas toohello `az network watcher show` cmdlet i steg 4.</span><span class="sxs-lookup"><span data-stu-id="080e0-141">TThe name of hello Network Watcher is passed toohello `az network watcher show` cmdlet in step 4.</span></span>

```azurecli
az network watcher show -resource-group resourceGroup -name networkWatcherName
```

### <a name="step-2"></a><span data-ttu-id="080e0-142">Steg 2</span><span class="sxs-lookup"><span data-stu-id="080e0-142">Step 2</span></span>

<span data-ttu-id="080e0-143">Hämta ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="080e0-143">Retrieve a storage account.</span></span> <span data-ttu-id="080e0-144">Det här lagringskontot är används toostore hello paket filen.</span><span class="sxs-lookup"><span data-stu-id="080e0-144">This storage account is used toostore hello packet capture file.</span></span>

```azurecli
azure storage account list
```

### <a name="step-3"></a><span data-ttu-id="080e0-145">Steg 3</span><span class="sxs-lookup"><span data-stu-id="080e0-145">Step 3</span></span>

<span data-ttu-id="080e0-146">Filter kan vara används toolimit hello data som lagras av hello paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="080e0-146">Filters can be used toolimit hello data that is stored by hello packet capture.</span></span> <span data-ttu-id="080e0-147">hello ställer följande exempel in en paketinsamling med flera filter.</span><span class="sxs-lookup"><span data-stu-id="080e0-147">hello following example sets up a packet capture with several  filters.</span></span>  <span data-ttu-id="080e0-148">hello tre första filter samlar in utgående TCP-trafik från lokala IP 10.0.0.3 toodestination portarna 20, 80 och 443.</span><span class="sxs-lookup"><span data-stu-id="080e0-148">hello first three filters collect outgoing TCP traffic only from local IP 10.0.0.3 toodestination ports 20, 80 and 443.</span></span>  <span data-ttu-id="080e0-149">hello senaste filter samlar in UDP-trafik.</span><span class="sxs-lookup"><span data-stu-id="080e0-149">hello last filter collects only UDP traffic.</span></span>

```azurecli
az network watcher packet-capture create --resource-group {resoureceurceGroupName} --vm {vmName} --name packetCaptureName --storage-account gwteststorage123abc --filters "[{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"20\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"80\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"443\"},{\"protocol\":\"UDP\"}]"
```

<span data-ttu-id="080e0-150">hello följande exempel är hello förväntades utdata från kör hello `az network watcher packet-capture create` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="080e0-150">hello following example is hello expected output from running hello `az network watcher packet-capture create` cmdlet.</span></span>

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

## <a name="get-a-packet-capture"></a><span data-ttu-id="080e0-151">Hämta en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="080e0-151">Get a packet capture</span></span>

<span data-ttu-id="080e0-152">Kör hello `az network watcher packet-capture show` cmdlet, hämtar hello status för en paketinsamling som körs eller har slutförts.</span><span class="sxs-lookup"><span data-stu-id="080e0-152">Running hello `az network watcher packet-capture show` cmdlet, retrieves hello status of a currently running, or completed packet capture.</span></span>

```azurecli
az network watcher packet-capture show --name packetCaptureName --location westcentralus
```

<span data-ttu-id="080e0-153">hello följande exempel är hello utdata från hello `az network watcher packet-capture show` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="080e0-153">hello following example is hello output from hello `az network watcher packet-capture show` cmdlet.</span></span> <span data-ttu-id="080e0-154">hello följande exempel är när hello hämtningen är klar.</span><span class="sxs-lookup"><span data-stu-id="080e0-154">hello following example is after hello capture is complete.</span></span> <span data-ttu-id="080e0-155">Hej PacketCaptureStatus värdet stoppas med en StopReason TimeExceeded.</span><span class="sxs-lookup"><span data-stu-id="080e0-155">hello PacketCaptureStatus value is Stopped, with a StopReason of TimeExceeded.</span></span> <span data-ttu-id="080e0-156">Det här värdet visar att hello paketinsamling lyckades och kördes tiden.</span><span class="sxs-lookup"><span data-stu-id="080e0-156">This value shows that hello packet capture was successful and ran its time.</span></span>

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

## <a name="stop-a-packet-capture"></a><span data-ttu-id="080e0-157">Stoppa en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="080e0-157">Stop a packet capture</span></span>

<span data-ttu-id="080e0-158">Genom att köra hello `az network watcher packet-capture stop` cmdlet, om en avbildningssessionen pågår den har stoppats.</span><span class="sxs-lookup"><span data-stu-id="080e0-158">By running hello `az network watcher packet-capture stop` cmdlet, if a capture session is in progress it is stopped.</span></span>

```azurecli
az network watcher packet-capture stop --name packetCaptureName --location westcentralus
```

> [!NOTE]
> <span data-ttu-id="080e0-159">hello cmdlet returnerar inget svar när kördes på en pågående avbildningssessionen eller en befintlig session som redan har stoppats.</span><span class="sxs-lookup"><span data-stu-id="080e0-159">hello cmdlet returns no response when ran on a currently running capture session or an existing session that has already stopped.</span></span>

## <a name="delete-a-packet-capture"></a><span data-ttu-id="080e0-160">Ta bort en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="080e0-160">Delete a packet capture</span></span>

```azurecli
az network watcher packet-capture delete --name packetCaptureName --location westcentralus
```

> [!NOTE]
> <span data-ttu-id="080e0-161">Om du tar bort en paketinsamling tar inte bort hello-filen i hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="080e0-161">Deleting a packet capture does not delete hello file in hello storage account.</span></span>

## <a name="download-a-packet-capture"></a><span data-ttu-id="080e0-162">Hämta en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="080e0-162">Download a packet capture</span></span>

<span data-ttu-id="080e0-163">När paketet avbildningssessionen är klar kan hello avbilda filen vara överförda tooblob lagring eller tooa lokal fil på hello VM.</span><span class="sxs-lookup"><span data-stu-id="080e0-163">Once your packet capture session has completed, hello capture file can be uploaded tooblob storage or tooa local file on hello VM.</span></span> <span data-ttu-id="080e0-164">hello lagringsplats för hello paketinsamling har definierats vid skapande av hello.</span><span class="sxs-lookup"><span data-stu-id="080e0-164">hello storage location of hello packet capture is defined at creation of hello session.</span></span> <span data-ttu-id="080e0-165">En lämplig verktyget tooaccess dessa avbilda filer sparade tooa storage-konto är Microsoft Azure Lagringsutforskaren, som kan hämtas här: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="080e0-165">A convenient tool tooaccess these capture files saved tooa storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="080e0-166">Om ett lagringskonto har angetts sparas paket avbilda filer tooa storage-konto på hello följande plats:</span><span class="sxs-lookup"><span data-stu-id="080e0-166">If a storage account is specified, packet capture files are saved tooa storage account at hello following location:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a><span data-ttu-id="080e0-167">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="080e0-167">Next steps</span></span>

<span data-ttu-id="080e0-168">Lär dig hur fångar tooautomate paket med virtuella aviseringar genom att visa [skapar en avisering utlösta paketinsamling](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="080e0-168">Learn how tooautomate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="080e0-169">Hitta om vissa trafik tillåts i eller utanför den virtuella datorn genom att besöka [Kontrollera Kontrollera IP-flöde](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="080e0-169">Find if certain traffic is allowed in or out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<!-- Image references -->
