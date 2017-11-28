---
title: "Hantera paket insamlingar med Nätverksbevakaren Azure - Azure CLI 2.0 | Microsoft Docs"
description: "Den här sidan förklarar hur du hanterar funktionen paket avbildning i Nätverksbevakaren som använder Azure CLI 2.0"
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
ms.openlocfilehash: c94eb46f31f2f19b843ccd7bf77b8a39943a07d4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-azure-cli-20"></a><span data-ttu-id="f88d7-103">Hantera paket insamlingar med Azure Nätverksbevakaren använder Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="f88d7-103">Manage packet captures with Azure Network Watcher using Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="f88d7-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f88d7-104">Azure portal</span></span>](network-watcher-packet-capture-manage-portal.md)
> - [<span data-ttu-id="f88d7-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f88d7-105">PowerShell</span></span>](network-watcher-packet-capture-manage-powershell.md)
> - [<span data-ttu-id="f88d7-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="f88d7-106">CLI 1.0</span></span>](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [<span data-ttu-id="f88d7-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="f88d7-107">CLI 2.0</span></span>](network-watcher-packet-capture-manage-cli.md)
> - [<span data-ttu-id="f88d7-108">Azure REST-API</span><span class="sxs-lookup"><span data-stu-id="f88d7-108">Azure REST API</span></span>](network-watcher-packet-capture-manage-rest.md)

<span data-ttu-id="f88d7-109">Nätverket Watcher paketinsamling kan du skapa avbildning sessioner för att spåra trafik till och från en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="f88d7-109">Network Watcher packet capture allows you to create capture sessions to track traffic to and from a virtual machine.</span></span> <span data-ttu-id="f88d7-110">Filter har angetts för hämtningens så du fångar upp trafiken som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="f88d7-110">Filters are provided for the capture session to ensure you capture only the traffic you want.</span></span> <span data-ttu-id="f88d7-111">Det hjälper dig för att diagnostisera nätverk avvikelser reaktivt och proaktivt paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="f88d7-111">Packet capture helps to diagnose network anomalies both reactively and proactively.</span></span> <span data-ttu-id="f88d7-112">Andra användningsområden omfattar att samla in nätverksstatistik får information om nätverket intrång felsöka klient-/ serverkommunikation och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="f88d7-112">Other uses include gathering network statistics, gaining information on network intrusions, to debug client-server communications and much more.</span></span> <span data-ttu-id="f88d7-113">Genom att via fjärranslutning utlösa paket insamlingar, underlättar den här funktionen för att köra en paketinsamling manuellt och på den önskade datorn, vilket sparar värdefull tid.</span><span class="sxs-lookup"><span data-stu-id="f88d7-113">By being able to remotely trigger packet captures, this capability eases the burden of running a packet capture manually and on the desired machine, which saves valuable time.</span></span>

<span data-ttu-id="f88d7-114">Den här artikeln använder våra nästa generations CLI för hantering av resursdistributionsmodell, Azure CLI 2.0, som är tillgänglig för Windows, Mac och Linux.</span><span class="sxs-lookup"><span data-stu-id="f88d7-114">This article uses our next generation CLI for the resource management deployment model, Azure CLI 2.0, which is available for Windows, Mac and Linux.</span></span>

<span data-ttu-id="f88d7-115">Om du vill utföra stegen i den här artikeln, måste du [installera Azure-kommandoradsgränssnittet för Mac, Linux och Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="f88d7-115">To perform the steps in this article, you need to [install the Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

<span data-ttu-id="f88d7-116">Den här artikeln tar dig igenom de olika administrativa uppgifter som är tillgängliga för paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="f88d7-116">This article takes you through the different management tasks that are currently available for packet capture.</span></span>

- [<span data-ttu-id="f88d7-117">**Starta en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="f88d7-117">**Start a packet capture**</span></span>](#start-a-packet-capture)
- [<span data-ttu-id="f88d7-118">**Stoppa en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="f88d7-118">**Stop a packet capture**</span></span>](#stop-a-packet-capture)
- [<span data-ttu-id="f88d7-119">**Ta bort en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="f88d7-119">**Delete a packet capture**</span></span>](#delete-a-packet-capture)
- [<span data-ttu-id="f88d7-120">**Hämta en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="f88d7-120">**Download a packet capture**</span></span>](#download-a-packet-capture)

## <a name="before-you-begin"></a><span data-ttu-id="f88d7-121">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="f88d7-121">Before you begin</span></span>

<span data-ttu-id="f88d7-122">Den här artikeln förutsätter att du har följande resurser:</span><span class="sxs-lookup"><span data-stu-id="f88d7-122">This article assumes you have the following resources:</span></span>

- <span data-ttu-id="f88d7-123">En instans av Nätverksbevakaren i den region som du vill skapa en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="f88d7-123">An instance of Network Watcher in the region you want to create a packet capture</span></span>
- <span data-ttu-id="f88d7-124">En virtuell dator med filnamnstillägget paket avbilda aktiverad.</span><span class="sxs-lookup"><span data-stu-id="f88d7-124">A virtual machine with the packet capture extension enabled.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f88d7-125">Paketinsamling kräver en agent körs på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="f88d7-125">Packet capture requires an agent to be running on the virtual machine.</span></span> <span data-ttu-id="f88d7-126">Agenten är installerad som ett tillägg.</span><span class="sxs-lookup"><span data-stu-id="f88d7-126">The Agent is installed as an extension.</span></span> <span data-ttu-id="f88d7-127">Anvisningar för VM-tillägg, besök [tillägg för virtuell dator och funktioner](../virtual-machines/windows/extensions-features.md).</span><span class="sxs-lookup"><span data-stu-id="f88d7-127">For instructions on VM extensions, visit [Virtual Machine extensions and features](../virtual-machines/windows/extensions-features.md).</span></span>

## <a name="install-vm-extension"></a><span data-ttu-id="f88d7-128">Installera tillägg för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="f88d7-128">Install VM extension</span></span>

### <a name="step-1"></a><span data-ttu-id="f88d7-129">Steg 1</span><span class="sxs-lookup"><span data-stu-id="f88d7-129">Step 1</span></span>

<span data-ttu-id="f88d7-130">Kör den `az vm extension set` för att installera paketet avbilda agenten på den virtuella gästdatorn.</span><span class="sxs-lookup"><span data-stu-id="f88d7-130">Run the `az vm extension set` cmdlet to install the packet capture agent on the guest virtual machine.</span></span>

<span data-ttu-id="f88d7-131">För Windows-datorer:</span><span class="sxs-lookup"><span data-stu-id="f88d7-131">For Windows virtual machines:</span></span>

```azurecli
az vm extension set --resource-group resourceGroupName --vm-name virtualMachineName --publisher Microsoft.Azure.NetworkWatcher --name NetworkWatcherAgentWindows --version 1.4
```

<span data-ttu-id="f88d7-132">För Linux virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="f88d7-132">For Linux virtual machines:</span></span>

```azurecli
az vm extension set --resource-group resourceGroupName --vm-name virtualMachineName --publisher Microsoft.Azure.NetworkWatcher --name NetworkWatcherAgentLinux--version 1.4
````

### <a name="step-2"></a><span data-ttu-id="f88d7-133">Steg 2</span><span class="sxs-lookup"><span data-stu-id="f88d7-133">Step 2</span></span>

<span data-ttu-id="f88d7-134">För att säkerställa att agenten är installerad, kör den `vm extension get` cmdlet och skickar den resursnamnet för grupp och den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="f88d7-134">To ensure that the agent is installed, run the `vm extension get` cmdlet and pass it the resource group and virtual machine name.</span></span> <span data-ttu-id="f88d7-135">Kontrollera listan för att säkerställa att agenten är installerad.</span><span class="sxs-lookup"><span data-stu-id="f88d7-135">Check the resulting list to ensure the agent is installed.</span></span>

```azurecli
az vm extension show -resource-group resourceGroupName --vm-name virtualMachineName --name NetworkWatcherAgentWindows
```

<span data-ttu-id="f88d7-136">I följande exempel är ett exempel på svar från att köras`az vm extension show`</span><span class="sxs-lookup"><span data-stu-id="f88d7-136">The following sample is an example of the response from running `az vm extension show`</span></span>

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

## <a name="start-a-packet-capture"></a><span data-ttu-id="f88d7-137">Starta en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="f88d7-137">Start a packet capture</span></span>

<span data-ttu-id="f88d7-138">När de föregående stegen är klar kan är paket avbilda agenten installerad på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="f88d7-138">Once the preceding steps are complete, the packet capture agent is installed on the virtual machine.</span></span>

### <a name="step-1"></a><span data-ttu-id="f88d7-139">Steg 1</span><span class="sxs-lookup"><span data-stu-id="f88d7-139">Step 1</span></span>

<span data-ttu-id="f88d7-140">Nästa steg är att hämta Nätverksbevakaren-instans.</span><span class="sxs-lookup"><span data-stu-id="f88d7-140">The next step is to retrieve the Network Watcher instance.</span></span> <span data-ttu-id="f88d7-141">Tdet namnet på Nätverksbevakaren har överförts till den `az network watcher show` cmdlet i steg 4.</span><span class="sxs-lookup"><span data-stu-id="f88d7-141">TThe name of the Network Watcher is passed to the `az network watcher show` cmdlet in step 4.</span></span>

```azurecli
az network watcher show -resource-group resourceGroup -name networkWatcherName
```

### <a name="step-2"></a><span data-ttu-id="f88d7-142">Steg 2</span><span class="sxs-lookup"><span data-stu-id="f88d7-142">Step 2</span></span>

<span data-ttu-id="f88d7-143">Hämta ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="f88d7-143">Retrieve a storage account.</span></span> <span data-ttu-id="f88d7-144">Det här lagringskontot används för att spara filen i paketet.</span><span class="sxs-lookup"><span data-stu-id="f88d7-144">This storage account is used to store the packet capture file.</span></span>

```azurecli
azure storage account list
```

### <a name="step-3"></a><span data-ttu-id="f88d7-145">Steg 3</span><span class="sxs-lookup"><span data-stu-id="f88d7-145">Step 3</span></span>

<span data-ttu-id="f88d7-146">Filter kan användas för att begränsa de data som lagras med paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="f88d7-146">Filters can be used to limit the data that is stored by the packet capture.</span></span> <span data-ttu-id="f88d7-147">I följande exempel ställer in en paketinsamling med flera filter.</span><span class="sxs-lookup"><span data-stu-id="f88d7-147">The following example sets up a packet capture with several  filters.</span></span>  <span data-ttu-id="f88d7-148">De tre första filter samlar in utgående TCP-trafik endast lokala IP 10.0.0.3 till målportar 20, 80 och 443.</span><span class="sxs-lookup"><span data-stu-id="f88d7-148">The first three filters collect outgoing TCP traffic only from local IP 10.0.0.3 to destination ports 20, 80 and 443.</span></span>  <span data-ttu-id="f88d7-149">Filtret som samlar in UDP-trafik.</span><span class="sxs-lookup"><span data-stu-id="f88d7-149">The last filter collects only UDP traffic.</span></span>

```azurecli
az network watcher packet-capture create --resource-group {resoureceurceGroupName} --vm {vmName} --name packetCaptureName --storage-account gwteststorage123abc --filters "[{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"20\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"80\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"443\"},{\"protocol\":\"UDP\"}]"
```

<span data-ttu-id="f88d7-150">Följande är exempel på utdata som förväntas från att köras i `az network watcher packet-capture create` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="f88d7-150">The following example is the expected output from running the `az network watcher packet-capture create` cmdlet.</span></span>

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

## <a name="get-a-packet-capture"></a><span data-ttu-id="f88d7-151">Hämta en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="f88d7-151">Get a packet capture</span></span>

<span data-ttu-id="f88d7-152">Kör den `az network watcher packet-capture show` cmdlet, hämtar status för en paketinsamling som körs eller har slutförts.</span><span class="sxs-lookup"><span data-stu-id="f88d7-152">Running the `az network watcher packet-capture show` cmdlet, retrieves the status of a currently running, or completed packet capture.</span></span>

```azurecli
az network watcher packet-capture show --name packetCaptureName --location westcentralus
```

<span data-ttu-id="f88d7-153">Följande är exempel på utdata från den `az network watcher packet-capture show` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="f88d7-153">The following example is the output from the `az network watcher packet-capture show` cmdlet.</span></span> <span data-ttu-id="f88d7-154">I följande exempel är när avbildningen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="f88d7-154">The following example is after the capture is complete.</span></span> <span data-ttu-id="f88d7-155">Värdet för PacketCaptureStatus stoppas med en StopReason TimeExceeded.</span><span class="sxs-lookup"><span data-stu-id="f88d7-155">The PacketCaptureStatus value is Stopped, with a StopReason of TimeExceeded.</span></span> <span data-ttu-id="f88d7-156">Det här värdet visar att paketinsamling lyckades och kördes tiden.</span><span class="sxs-lookup"><span data-stu-id="f88d7-156">This value shows that the packet capture was successful and ran its time.</span></span>

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

## <a name="stop-a-packet-capture"></a><span data-ttu-id="f88d7-157">Stoppa en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="f88d7-157">Stop a packet capture</span></span>

<span data-ttu-id="f88d7-158">Genom att köra den `az network watcher packet-capture stop` cmdlet, om en avbildningssessionen pågår den har stoppats.</span><span class="sxs-lookup"><span data-stu-id="f88d7-158">By running the `az network watcher packet-capture stop` cmdlet, if a capture session is in progress it is stopped.</span></span>

```azurecli
az network watcher packet-capture stop --name packetCaptureName --location westcentralus
```

> [!NOTE]
> <span data-ttu-id="f88d7-159">Cmdleten returnerar inga svar när kördes på en pågående avbildningssessionen eller en befintlig session som redan har stoppats.</span><span class="sxs-lookup"><span data-stu-id="f88d7-159">The cmdlet returns no response when ran on a currently running capture session or an existing session that has already stopped.</span></span>

## <a name="delete-a-packet-capture"></a><span data-ttu-id="f88d7-160">Ta bort en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="f88d7-160">Delete a packet capture</span></span>

```azurecli
az network watcher packet-capture delete --name packetCaptureName --location westcentralus
```

> [!NOTE]
> <span data-ttu-id="f88d7-161">Om du tar bort en paketinsamling tar inte bort filen i lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="f88d7-161">Deleting a packet capture does not delete the file in the storage account.</span></span>

## <a name="download-a-packet-capture"></a><span data-ttu-id="f88d7-162">Hämta en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="f88d7-162">Download a packet capture</span></span>

<span data-ttu-id="f88d7-163">När paketet avbildningssessionen är klar, kan avbilda filen laddas upp till blob-lagring eller till en lokal fil på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="f88d7-163">Once your packet capture session has completed, the capture file can be uploaded to blob storage or to a local file on the VM.</span></span> <span data-ttu-id="f88d7-164">Lagringsplatsen för paketinsamling har definierats vid skapandet av sessionen.</span><span class="sxs-lookup"><span data-stu-id="f88d7-164">The storage location of the packet capture is defined at creation of the session.</span></span> <span data-ttu-id="f88d7-165">Ett enkelt verktyg för att komma åt dessa avbilda filer som sparats till ett lagringskonto är Microsoft Azure Lagringsutforskaren, som kan hämtas här: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="f88d7-165">A convenient tool to access these capture files saved to a storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="f88d7-166">Om ett storage-konto anges sparas paket avbilda filer till ett lagringskonto på följande plats:</span><span class="sxs-lookup"><span data-stu-id="f88d7-166">If a storage account is specified, packet capture files are saved to a storage account at the following location:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a><span data-ttu-id="f88d7-167">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f88d7-167">Next steps</span></span>

<span data-ttu-id="f88d7-168">Lär dig att automatisera insamlingar paket med virtuella aviseringar genom att visa [skapar en avisering utlösta paketinsamling](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="f88d7-168">Learn how to automate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="f88d7-169">Hitta om vissa trafik tillåts i eller utanför den virtuella datorn genom att besöka [Kontrollera Kontrollera IP-flöde](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="f88d7-169">Find if certain traffic is allowed in or out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<!-- Image references -->
