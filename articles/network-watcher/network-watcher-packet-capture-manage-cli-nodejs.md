---
title: "Hantera paket insamlingar med Nätverksbevakaren Azure - Azure CLI 1.0 | Microsoft Docs"
description: "Den här sidan förklarar hur du hanterar funktionen paket avbildning i Nätverksbevakaren som använder Azure CLI 1.0"
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
ms.openlocfilehash: 91588910334859c1ea77186674d5bfb31b311b36
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-azure-cli-10"></a><span data-ttu-id="207fb-103">Hantera paket insamlingar med Azure Nätverksbevakaren använder Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="207fb-103">Manage packet captures with Azure Network Watcher using Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="207fb-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="207fb-104">Azure portal</span></span>](network-watcher-packet-capture-manage-portal.md)
> - [<span data-ttu-id="207fb-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="207fb-105">PowerShell</span></span>](network-watcher-packet-capture-manage-powershell.md)
> - [<span data-ttu-id="207fb-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="207fb-106">CLI 1.0</span></span>](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [<span data-ttu-id="207fb-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="207fb-107">CLI 2.0</span></span>](network-watcher-packet-capture-manage-cli.md)
> - [<span data-ttu-id="207fb-108">Azure REST-API</span><span class="sxs-lookup"><span data-stu-id="207fb-108">Azure REST API</span></span>](network-watcher-packet-capture-manage-rest.md)

<span data-ttu-id="207fb-109">Nätverket Watcher paketinsamling kan du skapa avbildning sessioner för att spåra trafik till och från en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="207fb-109">Network Watcher packet capture allows you to create capture sessions to track traffic to and from a virtual machine.</span></span> <span data-ttu-id="207fb-110">Filter har angetts för hämtningens så du fångar upp trafiken som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="207fb-110">Filters are provided for the capture session to ensure you capture only the traffic you want.</span></span> <span data-ttu-id="207fb-111">Det hjälper dig för att diagnostisera nätverk avvikelser reaktivt och proaktivt paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="207fb-111">Packet capture helps to diagnose network anomalies both reactively and proactively.</span></span> <span data-ttu-id="207fb-112">Andra användningsområden omfattar att samla in nätverksstatistik får information om nätverket intrång felsöka klient-/ serverkommunikation och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="207fb-112">Other uses include gathering network statistics, gaining information on network intrusions, to debug client-server communications and much more.</span></span> <span data-ttu-id="207fb-113">Genom att via fjärranslutning utlösa paket insamlingar, underlättar den här funktionen för att köra en paketinsamling manuellt och på den önskade datorn, vilket sparar värdefull tid.</span><span class="sxs-lookup"><span data-stu-id="207fb-113">By being able to remotely trigger packet captures, this capability eases the burden of running a packet capture manually and on the desired machine, which saves valuable time.</span></span>

<span data-ttu-id="207fb-114">Den här artikeln använder plattformsoberoende Azure CLI version 1.0, som är tillgänglig för Windows, Mac och Linux.</span><span class="sxs-lookup"><span data-stu-id="207fb-114">This article uses cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span>

<span data-ttu-id="207fb-115">Den här artikeln tar dig igenom de olika administrativa uppgifter som är tillgängliga för paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="207fb-115">This article takes you through the different management tasks that are currently available for packet capture.</span></span>

- [<span data-ttu-id="207fb-116">**Starta en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="207fb-116">**Start a packet capture**</span></span>](#start-a-packet-capture)
- [<span data-ttu-id="207fb-117">**Stoppa en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="207fb-117">**Stop a packet capture**</span></span>](#stop-a-packet-capture)
- [<span data-ttu-id="207fb-118">**Ta bort en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="207fb-118">**Delete a packet capture**</span></span>](#delete-a-packet-capture)
- [<span data-ttu-id="207fb-119">**Hämta en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="207fb-119">**Download a packet capture**</span></span>](#download-a-packet-capture)

## <a name="before-you-begin"></a><span data-ttu-id="207fb-120">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="207fb-120">Before you begin</span></span>

<span data-ttu-id="207fb-121">Den här artikeln förutsätter att du har följande resurser:</span><span class="sxs-lookup"><span data-stu-id="207fb-121">This article assumes you have the following resources:</span></span>

- <span data-ttu-id="207fb-122">En instans av Nätverksbevakaren i den region som du vill skapa en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="207fb-122">An instance of Network Watcher in the region you want to create a packet capture</span></span>
- <span data-ttu-id="207fb-123">En virtuell dator med filnamnstillägget paket avbilda aktiverad.</span><span class="sxs-lookup"><span data-stu-id="207fb-123">A virtual machine with the packet capture extension enabled.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="207fb-124">Paketinsamling kräver en agent körs på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="207fb-124">Packet capture requires an agent to be running on the virtual machine.</span></span> <span data-ttu-id="207fb-125">Agenten är installerad som ett tillägg.</span><span class="sxs-lookup"><span data-stu-id="207fb-125">The Agent is installed as an extension.</span></span> <span data-ttu-id="207fb-126">Anvisningar för VM-tillägg, besök [tillägg för virtuell dator och funktioner](../virtual-machines/windows/extensions-features.md).</span><span class="sxs-lookup"><span data-stu-id="207fb-126">For instructions on VM extensions, visit [Virtual Machine extensions and features](../virtual-machines/windows/extensions-features.md).</span></span>

## <a name="install-vm-extension"></a><span data-ttu-id="207fb-127">Installera tillägg för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="207fb-127">Install VM extension</span></span>

### <a name="step-1"></a><span data-ttu-id="207fb-128">Steg 1</span><span class="sxs-lookup"><span data-stu-id="207fb-128">Step 1</span></span>

<span data-ttu-id="207fb-129">Kör den `azure vm extension set` för att installera paketet avbilda agenten på den virtuella gästdatorn.</span><span class="sxs-lookup"><span data-stu-id="207fb-129">Run the `azure vm extension set` cmdlet to install the packet capture agent on the guest virtual machine.</span></span>

<span data-ttu-id="207fb-130">För Windows-datorer:</span><span class="sxs-lookup"><span data-stu-id="207fb-130">For Windows virtual machines:</span></span>

```azurecli
azure vm extension set -g resourceGroupName -m virtualMachineName -p Microsoft.Azure.NetworkWatcher -r AzureNetworkWatcherExtension -n NetworkWatcherAgentWindows -o 1.4
```

<span data-ttu-id="207fb-131">För Linux virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="207fb-131">For Linux virtual machines:</span></span>

```azurecli
azure vm extension set -g resourceGroupName -m virtualMachineName -p Microsoft.Azure.NetworkWatcher -r AzureNetworkWatcherExtension -n NetworkWatcherAgentLinux -o 1.4
````

### <a name="step-2"></a><span data-ttu-id="207fb-132">Steg 2</span><span class="sxs-lookup"><span data-stu-id="207fb-132">Step 2</span></span>

<span data-ttu-id="207fb-133">För att säkerställa att agenten är installerad, kör den `vm extension get` cmdlet och skickar den resursnamnet för grupp och den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="207fb-133">To ensure that the agent is installed, run the `vm extension get` cmdlet and pass it the resource group and virtual machine name.</span></span> <span data-ttu-id="207fb-134">Kontrollera listan för att säkerställa att agenten är installerad.</span><span class="sxs-lookup"><span data-stu-id="207fb-134">Check the resulting list to ensure the agent is installed.</span></span>

```azurecli
azure vm extension get -g resourceGroupName -m virtualMachineName
```

<span data-ttu-id="207fb-135">I följande exempel är ett exempel på svar från att köras`azure vm extension get`</span><span class="sxs-lookup"><span data-stu-id="207fb-135">The following sample is an example of the response from running `azure vm extension get`</span></span>

```
info:    Executing command vm extension get
+ Looking up the VM "virtualMachineName"
data:    Publisher                       Name                        Version  State
data:    ------------------------------  -----------------------     -------  ---------
data:    Microsoft.Azure.NetworkWatcher  NetworkWatcherAgentWindows  1.4      Succeeded
info:    vm extension get command OK
```

## <a name="start-a-packet-capture"></a><span data-ttu-id="207fb-136">Starta en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="207fb-136">Start a packet capture</span></span>

<span data-ttu-id="207fb-137">När de föregående stegen är klar kan är paket avbilda agenten installerad på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="207fb-137">Once the preceding steps are complete, the packet capture agent is installed on the virtual machine.</span></span>

### <a name="step-1"></a><span data-ttu-id="207fb-138">Steg 1</span><span class="sxs-lookup"><span data-stu-id="207fb-138">Step 1</span></span>

<span data-ttu-id="207fb-139">Nästa steg är att hämta Nätverksbevakaren-instans.</span><span class="sxs-lookup"><span data-stu-id="207fb-139">The next step is to retrieve the Network Watcher instance.</span></span> <span data-ttu-id="207fb-140">Den här variabeln har överförts till den `network watcher show` cmdlet i steg 4.</span><span class="sxs-lookup"><span data-stu-id="207fb-140">This variable is passed to the `network watcher show` cmdlet in step 4.</span></span>

```azurecli
azure network watcher show -g resourceGroup -n networkWatcherName
```

### <a name="step-2"></a><span data-ttu-id="207fb-141">Steg 2</span><span class="sxs-lookup"><span data-stu-id="207fb-141">Step 2</span></span>

<span data-ttu-id="207fb-142">Hämta ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="207fb-142">Retrieve a storage account.</span></span> <span data-ttu-id="207fb-143">Det här lagringskontot används för att spara filen i paketet.</span><span class="sxs-lookup"><span data-stu-id="207fb-143">This storage account is used to store the packet capture file.</span></span>

```azurecli
azure storage account list
```

### <a name="step-3"></a><span data-ttu-id="207fb-144">Steg 3</span><span class="sxs-lookup"><span data-stu-id="207fb-144">Step 3</span></span>

<span data-ttu-id="207fb-145">Filter kan användas för att begränsa de data som lagras med paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="207fb-145">Filters can be used to limit the data that is stored by the packet capture.</span></span> <span data-ttu-id="207fb-146">I följande exempel ställer in en paketinsamling med flera filter.</span><span class="sxs-lookup"><span data-stu-id="207fb-146">The following example sets up a packet capture with several  filters.</span></span>  <span data-ttu-id="207fb-147">De tre första filter samlar in utgående TCP-trafik endast lokala IP 10.0.0.3 till målportar 20, 80 och 443.</span><span class="sxs-lookup"><span data-stu-id="207fb-147">The first three filters collect outgoing TCP traffic only from local IP 10.0.0.3 to destination ports 20, 80 and 443.</span></span>  <span data-ttu-id="207fb-148">Filtret som samlar in UDP-trafik.</span><span class="sxs-lookup"><span data-stu-id="207fb-148">The last filter collects only UDP traffic.</span></span>

```azurecli
azure network watcher packet-capture create -g resourceGroupName -w networkWatcherName -n packetCaptureName -t targetResourceId -o storageAccountResourceId -f "[{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"20\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"80\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"443\"},{\"protocol\":\"UDP\"}]"
```

<span data-ttu-id="207fb-149">Flera filter kan definieras för en paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="207fb-149">Multiple filters can be defined for a packet capture.</span></span> <span data-ttu-id="207fb-150">Om du använder en komplexa filter-struktur, är det bättre att använda filter som en json-fil för att undvika syntaxfel.</span><span class="sxs-lookup"><span data-stu-id="207fb-150">If you are using a complex filter structure, it is better to use filters as a json file to avoid syntax errors.</span></span> <span data-ttu-id="207fb-151">Till exempel använda flaggan ”-r” (i stället för ”-f”) och ange platsen för en json-fil som innehåller följande filter:</span><span class="sxs-lookup"><span data-stu-id="207fb-151">For example, use the flag "-r" (instead of "-f") and pass the location of a json file containing the following filters:</span></span>

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


<span data-ttu-id="207fb-152">Följande är exempel på utdata som förväntas från att köras i `network watcher packet-capture create` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="207fb-152">The following example is the expected output from running the `network watcher packet-capture create` cmdlet.</span></span>

```
data:    Name                            : packetCaptureName
data:    Etag                            : W/"d59bb2d2-dc95-43da-b740-e0ef8fcacecb"
data:    Target                          : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Compute/virtualMachines/testVM
data:    Bytes To Capture Per Packet     : 0
data:    Total Bytes Per Session         : 1073741824
data:    Time Limit In Seconds           : 18000
data:    Storage Location:
data:      Storage Id                    : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Storage/storageAccounts/testStorage
data:      Storage Path                  : https://testStorage.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/testRG/providers/microsoft.compute/virtualmachines/testVM/2017/02/17/packetcapture_01_21_18_145.cap
data:    Filters                         : []
data:    Provisioning State              : Succeeded
info:    network watcher packet-capture create command OK
```

## <a name="get-a-packet-capture"></a><span data-ttu-id="207fb-153">Hämta en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="207fb-153">Get a packet capture</span></span>

<span data-ttu-id="207fb-154">Kör den `network watcher packet-capture show` cmdlet, hämtar status för en paketinsamling som körs eller har slutförts.</span><span class="sxs-lookup"><span data-stu-id="207fb-154">Running the `network watcher packet-capture show` cmdlet, retrieves the status of a currently running, or completed packet capture.</span></span>

```azurecli
azure network watcher packet-capture show -g resourceGroupName -w networkWatcherName -n packetCaptureName
```

<span data-ttu-id="207fb-155">Följande är exempel på utdata från den `network watcher packet-capture show` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="207fb-155">The following example is the output from the `network watcher packet-capture show` cmdlet.</span></span> <span data-ttu-id="207fb-156">I följande exempel är när avbildningen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="207fb-156">The following example is after the capture is complete.</span></span> <span data-ttu-id="207fb-157">Värdet för PacketCaptureStatus stoppas med en StopReason TimeExceeded.</span><span class="sxs-lookup"><span data-stu-id="207fb-157">The PacketCaptureStatus value is Stopped, with a StopReason of TimeExceeded.</span></span> <span data-ttu-id="207fb-158">Det här värdet visar att paketinsamling lyckades och kördes tiden.</span><span class="sxs-lookup"><span data-stu-id="207fb-158">This value shows that the packet capture was successful and ran its time.</span></span>

```
data:    Name                            : packetCaptureName
data:    Etag                            : W/"d59bb2d2-dc95-43da-b740-e0ef8fcacecb"
data:    Target                          : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Compute/virtualMachines/testVM
data:    Bytes To Capture Per Packet     : 0
data:    Total Bytes Per Session         : 1073741824
data:    Time Limit In Seconds           : 18000
data:    Storage Location:
data:      Storage Id                    : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Storage/storageAccounts/testStorage
data:      Storage Path                  : https://testStorage.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/testRG/providers/microsoft.compute/virtualmachines/testVM/2017/02/17/packetcapture_01_21_18_145.cap
data:    Filters                         : []
data:    Provisioning State              : Succeeded
info:    network watcher packet-capture show command OK
```

## <a name="stop-a-packet-capture"></a><span data-ttu-id="207fb-159">Stoppa en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="207fb-159">Stop a packet capture</span></span>

<span data-ttu-id="207fb-160">Genom att köra den `network watcher packet-capture stop` cmdlet, om en avbildningssessionen pågår den har stoppats.</span><span class="sxs-lookup"><span data-stu-id="207fb-160">By running the `network watcher packet-capture stop` cmdlet, if a capture session is in progress it is stopped.</span></span>

```azurecli
azure network watcher packet-capture stop -g resourceGroupName -w networkWatcherName -n packetCaptureName
```

> [!NOTE]
> <span data-ttu-id="207fb-161">Cmdleten returnerar inga svar när kördes på en pågående avbildningssessionen eller en befintlig session som redan har stoppats.</span><span class="sxs-lookup"><span data-stu-id="207fb-161">The cmdlet returns no response when ran on a currently running capture session or an existing session that has already stopped.</span></span>

## <a name="delete-a-packet-capture"></a><span data-ttu-id="207fb-162">Ta bort en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="207fb-162">Delete a packet capture</span></span>

```azurecli
azure network watcher packet-capture delete -g resourceGroupName -w networkWatcherName -n packetCaptureName
```

> [!NOTE]
> <span data-ttu-id="207fb-163">Om du tar bort en paketinsamling tar inte bort filen i lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="207fb-163">Deleting a packet capture does not delete the file in the storage account.</span></span>

## <a name="download-a-packet-capture"></a><span data-ttu-id="207fb-164">Hämta en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="207fb-164">Download a packet capture</span></span>

<span data-ttu-id="207fb-165">När paketet avbildningssessionen är klar, kan avbilda filen laddas upp till blob-lagring eller till en lokal fil på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="207fb-165">Once your packet capture session has completed, the capture file can be uploaded to blob storage or to a local file on the VM.</span></span> <span data-ttu-id="207fb-166">Lagringsplatsen för paketinsamling har definierats vid skapandet av sessionen.</span><span class="sxs-lookup"><span data-stu-id="207fb-166">The storage location of the packet capture is defined at creation of the session.</span></span> <span data-ttu-id="207fb-167">Ett enkelt verktyg för att komma åt dessa avbilda filer som sparats till ett lagringskonto är Microsoft Azure Lagringsutforskaren, som kan hämtas här: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="207fb-167">A convenient tool to access these capture files saved to a storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="207fb-168">Om ett storage-konto anges sparas paket avbilda filer till ett lagringskonto på följande plats:</span><span class="sxs-lookup"><span data-stu-id="207fb-168">If a storage account is specified, packet capture files are saved to a storage account at the following location:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a><span data-ttu-id="207fb-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="207fb-169">Next steps</span></span>

<span data-ttu-id="207fb-170">Lär dig att automatisera insamlingar paket med virtuella aviseringar genom att visa [skapar en avisering utlösta paketinsamling](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="207fb-170">Learn how to automate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="207fb-171">Hitta om vissa trafik tillåts i eller utanför den virtuella datorn genom att besöka [Kontrollera Kontrollera IP-flöde](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="207fb-171">Find if certain traffic is allowed in or out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<!-- Image references -->
