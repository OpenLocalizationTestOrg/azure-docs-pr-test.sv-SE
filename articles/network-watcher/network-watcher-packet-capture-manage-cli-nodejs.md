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
# <a name="manage-packet-captures-with-azure-network-watcher-using-azure-cli-10"></a><span data-ttu-id="ae1f0-103">Hantera paket insamlingar med Azure Nätverksbevakaren använder Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="ae1f0-103">Manage packet captures with Azure Network Watcher using Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="ae1f0-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ae1f0-104">Azure portal</span></span>](network-watcher-packet-capture-manage-portal.md)
> - [<span data-ttu-id="ae1f0-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ae1f0-105">PowerShell</span></span>](network-watcher-packet-capture-manage-powershell.md)
> - [<span data-ttu-id="ae1f0-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="ae1f0-106">CLI 1.0</span></span>](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [<span data-ttu-id="ae1f0-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="ae1f0-107">CLI 2.0</span></span>](network-watcher-packet-capture-manage-cli.md)
> - [<span data-ttu-id="ae1f0-108">Azure REST-API</span><span class="sxs-lookup"><span data-stu-id="ae1f0-108">Azure REST API</span></span>](network-watcher-packet-capture-manage-rest.md)

<span data-ttu-id="ae1f0-109">Nätverket Watcher paketinsamling kan toocreate avbilda sessioner tootrack trafik tooand från en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ae1f0-109">Network Watcher packet capture allows you toocreate capture sessions tootrack traffic tooand from a virtual machine.</span></span> <span data-ttu-id="ae1f0-110">Filter har angetts för hello avbilda session tooensure du fånga in endast hello trafik som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="ae1f0-110">Filters are provided for hello capture session tooensure you capture only hello traffic you want.</span></span> <span data-ttu-id="ae1f0-111">Paketinsamling hjälper toodiagnose nätverk avvikelser reaktivt och proaktivt.</span><span class="sxs-lookup"><span data-stu-id="ae1f0-111">Packet capture helps toodiagnose network anomalies both reactively and proactively.</span></span> <span data-ttu-id="ae1f0-112">Andra användningsområden omfattar att samla in nätverksstatistik får information om nätverket intrång, toodebug klient / server-kommunikation och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="ae1f0-112">Other uses include gathering network statistics, gaining information on network intrusions, toodebug client-server communications and much more.</span></span> <span data-ttu-id="ae1f0-113">Den här funktionen underlättar genom kan tooremotely utlösaren paket insamlingar, hello belastningen körs en paketinsamling manuellt och på hello önskade dator som sparar värdefull tid.</span><span class="sxs-lookup"><span data-stu-id="ae1f0-113">By being able tooremotely trigger packet captures, this capability eases hello burden of running a packet capture manually and on hello desired machine, which saves valuable time.</span></span>

<span data-ttu-id="ae1f0-114">Den här artikeln använder plattformsoberoende Azure CLI version 1.0, som är tillgänglig för Windows, Mac och Linux.</span><span class="sxs-lookup"><span data-stu-id="ae1f0-114">This article uses cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span>

<span data-ttu-id="ae1f0-115">Den här artikeln tar dig igenom hello olika administrativa uppgifter som är tillgängliga för paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="ae1f0-115">This article takes you through hello different management tasks that are currently available for packet capture.</span></span>

- [<span data-ttu-id="ae1f0-116">**Starta en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="ae1f0-116">**Start a packet capture**</span></span>](#start-a-packet-capture)
- [<span data-ttu-id="ae1f0-117">**Stoppa en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="ae1f0-117">**Stop a packet capture**</span></span>](#stop-a-packet-capture)
- [<span data-ttu-id="ae1f0-118">**Ta bort en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="ae1f0-118">**Delete a packet capture**</span></span>](#delete-a-packet-capture)
- [<span data-ttu-id="ae1f0-119">**Hämta en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="ae1f0-119">**Download a packet capture**</span></span>](#download-a-packet-capture)

## <a name="before-you-begin"></a><span data-ttu-id="ae1f0-120">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="ae1f0-120">Before you begin</span></span>

<span data-ttu-id="ae1f0-121">Den här artikeln förutsätter att du har hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="ae1f0-121">This article assumes you have hello following resources:</span></span>

- <span data-ttu-id="ae1f0-122">En instans Nätverksbevakaren i hello region som du vill toocreate en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="ae1f0-122">An instance of Network Watcher in hello region you want toocreate a packet capture</span></span>
- <span data-ttu-id="ae1f0-123">En virtuell dator med hello paket avbilda tillägget aktiverat.</span><span class="sxs-lookup"><span data-stu-id="ae1f0-123">A virtual machine with hello packet capture extension enabled.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ae1f0-124">Paketinsamling kräver en agent toobe som körs på hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ae1f0-124">Packet capture requires an agent toobe running on hello virtual machine.</span></span> <span data-ttu-id="ae1f0-125">hello Agent installeras som ett tillägg.</span><span class="sxs-lookup"><span data-stu-id="ae1f0-125">hello Agent is installed as an extension.</span></span> <span data-ttu-id="ae1f0-126">Anvisningar för VM-tillägg, besök [tillägg för virtuell dator och funktioner](../virtual-machines/windows/extensions-features.md).</span><span class="sxs-lookup"><span data-stu-id="ae1f0-126">For instructions on VM extensions, visit [Virtual Machine extensions and features](../virtual-machines/windows/extensions-features.md).</span></span>

## <a name="install-vm-extension"></a><span data-ttu-id="ae1f0-127">Installera tillägg för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="ae1f0-127">Install VM extension</span></span>

### <a name="step-1"></a><span data-ttu-id="ae1f0-128">Steg 1</span><span class="sxs-lookup"><span data-stu-id="ae1f0-128">Step 1</span></span>

<span data-ttu-id="ae1f0-129">Kör hello `azure vm extension set` cmdlet tooinstall hello paket avbilda agent på hello virtuella gästdatorn.</span><span class="sxs-lookup"><span data-stu-id="ae1f0-129">Run hello `azure vm extension set` cmdlet tooinstall hello packet capture agent on hello guest virtual machine.</span></span>

<span data-ttu-id="ae1f0-130">För Windows-datorer:</span><span class="sxs-lookup"><span data-stu-id="ae1f0-130">For Windows virtual machines:</span></span>

```azurecli
azure vm extension set -g resourceGroupName -m virtualMachineName -p Microsoft.Azure.NetworkWatcher -r AzureNetworkWatcherExtension -n NetworkWatcherAgentWindows -o 1.4
```

<span data-ttu-id="ae1f0-131">För Linux virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="ae1f0-131">For Linux virtual machines:</span></span>

```azurecli
azure vm extension set -g resourceGroupName -m virtualMachineName -p Microsoft.Azure.NetworkWatcher -r AzureNetworkWatcherExtension -n NetworkWatcherAgentLinux -o 1.4
````

### <a name="step-2"></a><span data-ttu-id="ae1f0-132">Steg 2</span><span class="sxs-lookup"><span data-stu-id="ae1f0-132">Step 2</span></span>

<span data-ttu-id="ae1f0-133">tooensure som hello agenten är installerad, kör hello `vm extension get` cmdlet och skickar den hello resursgrupp och namn på virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ae1f0-133">tooensure that hello agent is installed, run hello `vm extension get` cmdlet and pass it hello resource group and virtual machine name.</span></span> <span data-ttu-id="ae1f0-134">Kontrollera hello resulterande listan tooensure hello-agenten är installerad.</span><span class="sxs-lookup"><span data-stu-id="ae1f0-134">Check hello resulting list tooensure hello agent is installed.</span></span>

```azurecli
azure vm extension get -g resourceGroupName -m virtualMachineName
```

<span data-ttu-id="ae1f0-135">hello följande exempel är ett exempel på hello svar från att köras`azure vm extension get`</span><span class="sxs-lookup"><span data-stu-id="ae1f0-135">hello following sample is an example of hello response from running `azure vm extension get`</span></span>

```
info:    Executing command vm extension get
+ Looking up hello VM "virtualMachineName"
data:    Publisher                       Name                        Version  State
data:    ------------------------------  -----------------------     -------  ---------
data:    Microsoft.Azure.NetworkWatcher  NetworkWatcherAgentWindows  1.4      Succeeded
info:    vm extension get command OK
```

## <a name="start-a-packet-capture"></a><span data-ttu-id="ae1f0-136">Starta en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="ae1f0-136">Start a packet capture</span></span>

<span data-ttu-id="ae1f0-137">När hello föregående steg är klar kan är hello paket avbilda agenten installerad på hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ae1f0-137">Once hello preceding steps are complete, hello packet capture agent is installed on hello virtual machine.</span></span>

### <a name="step-1"></a><span data-ttu-id="ae1f0-138">Steg 1</span><span class="sxs-lookup"><span data-stu-id="ae1f0-138">Step 1</span></span>

<span data-ttu-id="ae1f0-139">hello nästa steg är tooretrieve hello Nätverksbevakaren instans.</span><span class="sxs-lookup"><span data-stu-id="ae1f0-139">hello next step is tooretrieve hello Network Watcher instance.</span></span> <span data-ttu-id="ae1f0-140">Den här variabeln skickas toohello `network watcher show` cmdlet i steg 4.</span><span class="sxs-lookup"><span data-stu-id="ae1f0-140">This variable is passed toohello `network watcher show` cmdlet in step 4.</span></span>

```azurecli
azure network watcher show -g resourceGroup -n networkWatcherName
```

### <a name="step-2"></a><span data-ttu-id="ae1f0-141">Steg 2</span><span class="sxs-lookup"><span data-stu-id="ae1f0-141">Step 2</span></span>

<span data-ttu-id="ae1f0-142">Hämta ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="ae1f0-142">Retrieve a storage account.</span></span> <span data-ttu-id="ae1f0-143">Det här lagringskontot är används toostore hello paket filen.</span><span class="sxs-lookup"><span data-stu-id="ae1f0-143">This storage account is used toostore hello packet capture file.</span></span>

```azurecli
azure storage account list
```

### <a name="step-3"></a><span data-ttu-id="ae1f0-144">Steg 3</span><span class="sxs-lookup"><span data-stu-id="ae1f0-144">Step 3</span></span>

<span data-ttu-id="ae1f0-145">Filter kan vara används toolimit hello data som lagras av hello paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="ae1f0-145">Filters can be used toolimit hello data that is stored by hello packet capture.</span></span> <span data-ttu-id="ae1f0-146">hello ställer följande exempel in en paketinsamling med flera filter.</span><span class="sxs-lookup"><span data-stu-id="ae1f0-146">hello following example sets up a packet capture with several  filters.</span></span>  <span data-ttu-id="ae1f0-147">hello tre första filter samlar in utgående TCP-trafik från lokala IP 10.0.0.3 toodestination portarna 20, 80 och 443.</span><span class="sxs-lookup"><span data-stu-id="ae1f0-147">hello first three filters collect outgoing TCP traffic only from local IP 10.0.0.3 toodestination ports 20, 80 and 443.</span></span>  <span data-ttu-id="ae1f0-148">hello senaste filter samlar in UDP-trafik.</span><span class="sxs-lookup"><span data-stu-id="ae1f0-148">hello last filter collects only UDP traffic.</span></span>

```azurecli
azure network watcher packet-capture create -g resourceGroupName -w networkWatcherName -n packetCaptureName -t targetResourceId -o storageAccountResourceId -f "[{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"20\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"80\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"443\"},{\"protocol\":\"UDP\"}]"
```

<span data-ttu-id="ae1f0-149">Flera filter kan definieras för en paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="ae1f0-149">Multiple filters can be defined for a packet capture.</span></span> <span data-ttu-id="ae1f0-150">Om du använder en komplexa filter struktur är bättre toouse filter som ett json-fil tooavoid syntaxfel.</span><span class="sxs-lookup"><span data-stu-id="ae1f0-150">If you are using a complex filter structure, it is better toouse filters as a json file tooavoid syntax errors.</span></span> <span data-ttu-id="ae1f0-151">Till exempel använda hello flaggan ”-r” (i stället för ”-f”) och skicka hello platsen för en json-fil som innehåller hello följande filter:</span><span class="sxs-lookup"><span data-stu-id="ae1f0-151">For example, use hello flag "-r" (instead of "-f") and pass hello location of a json file containing hello following filters:</span></span>

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


<span data-ttu-id="ae1f0-152">hello följande exempel är hello förväntades utdata från kör hello `network watcher packet-capture create` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="ae1f0-152">hello following example is hello expected output from running hello `network watcher packet-capture create` cmdlet.</span></span>

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

## <a name="get-a-packet-capture"></a><span data-ttu-id="ae1f0-153">Hämta en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="ae1f0-153">Get a packet capture</span></span>

<span data-ttu-id="ae1f0-154">Kör hello `network watcher packet-capture show` cmdlet, hämtar hello status för en paketinsamling som körs eller har slutförts.</span><span class="sxs-lookup"><span data-stu-id="ae1f0-154">Running hello `network watcher packet-capture show` cmdlet, retrieves hello status of a currently running, or completed packet capture.</span></span>

```azurecli
azure network watcher packet-capture show -g resourceGroupName -w networkWatcherName -n packetCaptureName
```

<span data-ttu-id="ae1f0-155">hello följande exempel är hello utdata från hello `network watcher packet-capture show` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="ae1f0-155">hello following example is hello output from hello `network watcher packet-capture show` cmdlet.</span></span> <span data-ttu-id="ae1f0-156">hello följande exempel är när hello hämtningen är klar.</span><span class="sxs-lookup"><span data-stu-id="ae1f0-156">hello following example is after hello capture is complete.</span></span> <span data-ttu-id="ae1f0-157">Hej PacketCaptureStatus värdet stoppas med en StopReason TimeExceeded.</span><span class="sxs-lookup"><span data-stu-id="ae1f0-157">hello PacketCaptureStatus value is Stopped, with a StopReason of TimeExceeded.</span></span> <span data-ttu-id="ae1f0-158">Det här värdet visar att hello paketinsamling lyckades och kördes tiden.</span><span class="sxs-lookup"><span data-stu-id="ae1f0-158">This value shows that hello packet capture was successful and ran its time.</span></span>

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

## <a name="stop-a-packet-capture"></a><span data-ttu-id="ae1f0-159">Stoppa en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="ae1f0-159">Stop a packet capture</span></span>

<span data-ttu-id="ae1f0-160">Genom att köra hello `network watcher packet-capture stop` cmdlet, om en avbildningssessionen pågår den har stoppats.</span><span class="sxs-lookup"><span data-stu-id="ae1f0-160">By running hello `network watcher packet-capture stop` cmdlet, if a capture session is in progress it is stopped.</span></span>

```azurecli
azure network watcher packet-capture stop -g resourceGroupName -w networkWatcherName -n packetCaptureName
```

> [!NOTE]
> <span data-ttu-id="ae1f0-161">hello cmdlet returnerar inget svar när kördes på en pågående avbildningssessionen eller en befintlig session som redan har stoppats.</span><span class="sxs-lookup"><span data-stu-id="ae1f0-161">hello cmdlet returns no response when ran on a currently running capture session or an existing session that has already stopped.</span></span>

## <a name="delete-a-packet-capture"></a><span data-ttu-id="ae1f0-162">Ta bort en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="ae1f0-162">Delete a packet capture</span></span>

```azurecli
azure network watcher packet-capture delete -g resourceGroupName -w networkWatcherName -n packetCaptureName
```

> [!NOTE]
> <span data-ttu-id="ae1f0-163">Om du tar bort en paketinsamling tar inte bort hello-filen i hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="ae1f0-163">Deleting a packet capture does not delete hello file in hello storage account.</span></span>

## <a name="download-a-packet-capture"></a><span data-ttu-id="ae1f0-164">Hämta en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="ae1f0-164">Download a packet capture</span></span>

<span data-ttu-id="ae1f0-165">När paketet avbildningssessionen är klar kan hello avbilda filen vara överförda tooblob lagring eller tooa lokal fil på hello VM.</span><span class="sxs-lookup"><span data-stu-id="ae1f0-165">Once your packet capture session has completed, hello capture file can be uploaded tooblob storage or tooa local file on hello VM.</span></span> <span data-ttu-id="ae1f0-166">hello lagringsplats för hello paketinsamling har definierats vid skapande av hello.</span><span class="sxs-lookup"><span data-stu-id="ae1f0-166">hello storage location of hello packet capture is defined at creation of hello session.</span></span> <span data-ttu-id="ae1f0-167">En lämplig verktyget tooaccess dessa avbilda filer sparade tooa storage-konto är Microsoft Azure Lagringsutforskaren, som kan hämtas här: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="ae1f0-167">A convenient tool tooaccess these capture files saved tooa storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="ae1f0-168">Om ett lagringskonto har angetts sparas paket avbilda filer tooa storage-konto på hello följande plats:</span><span class="sxs-lookup"><span data-stu-id="ae1f0-168">If a storage account is specified, packet capture files are saved tooa storage account at hello following location:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a><span data-ttu-id="ae1f0-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ae1f0-169">Next steps</span></span>

<span data-ttu-id="ae1f0-170">Lär dig hur fångar tooautomate paket med virtuella aviseringar genom att visa [skapar en avisering utlösta paketinsamling](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="ae1f0-170">Learn how tooautomate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="ae1f0-171">Hitta om vissa trafik tillåts i eller utanför den virtuella datorn genom att besöka [Kontrollera Kontrollera IP-flöde](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="ae1f0-171">Find if certain traffic is allowed in or out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<!-- Image references -->
