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
# <a name="manage-packet-captures-with-azure-network-watcher-using-powershell"></a><span data-ttu-id="6cfb6-103">Hantera paket insamlingar med Azure Nätverksbevakaren med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="6cfb6-103">Manage packet captures with Azure Network Watcher using PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="6cfb6-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6cfb6-104">Azure portal</span></span>](network-watcher-packet-capture-manage-portal.md)
> - [<span data-ttu-id="6cfb6-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6cfb6-105">PowerShell</span></span>](network-watcher-packet-capture-manage-powershell.md)
> - [<span data-ttu-id="6cfb6-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="6cfb6-106">CLI 1.0</span></span>](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [<span data-ttu-id="6cfb6-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="6cfb6-107">CLI 2.0</span></span>](network-watcher-packet-capture-manage-cli.md)
> - [<span data-ttu-id="6cfb6-108">Azure REST-API</span><span class="sxs-lookup"><span data-stu-id="6cfb6-108">Azure REST API</span></span>](network-watcher-packet-capture-manage-rest.md)

<span data-ttu-id="6cfb6-109">Nätverket Watcher paketinsamling kan toocreate avbilda sessioner tootrack trafik tooand från en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="6cfb6-109">Network Watcher packet capture allows you toocreate capture sessions tootrack traffic tooand from a virtual machine.</span></span> <span data-ttu-id="6cfb6-110">Filter har angetts för hello avbilda session tooensure du fånga in endast hello trafik som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="6cfb6-110">Filters are provided for hello capture session tooensure you capture only hello traffic you want.</span></span> <span data-ttu-id="6cfb6-111">Paketinsamling hjälper toodiagnose nätverk avvikelser reaktivt och proaktivt.</span><span class="sxs-lookup"><span data-stu-id="6cfb6-111">Packet capture helps toodiagnose network anomalies both reactively and proactively.</span></span> <span data-ttu-id="6cfb6-112">Andra användningsområden omfattar att samla in nätverksstatistik får information om nätverket intrång, toodebug klient / server-kommunikation och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="6cfb6-112">Other uses include gathering network statistics, gaining information on network intrusions, toodebug client-server communications and much more.</span></span> <span data-ttu-id="6cfb6-113">Den här funktionen underlättar genom kan tooremotely utlösaren paket insamlingar, hello belastningen körs en paketinsamling manuellt och på hello önskade dator som sparar värdefull tid.</span><span class="sxs-lookup"><span data-stu-id="6cfb6-113">By being able tooremotely trigger packet captures, this capability eases hello burden of running a packet capture manually and on hello desired machine, which saves valuable time.</span></span>

<span data-ttu-id="6cfb6-114">Den här artikeln tar dig igenom hello olika administrativa uppgifter som är tillgängliga för paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="6cfb6-114">This article takes you through hello different management tasks that are currently available for packet capture.</span></span>

- [<span data-ttu-id="6cfb6-115">**Starta en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="6cfb6-115">**Start a packet capture**</span></span>](#start-a-packet-capture)
- [<span data-ttu-id="6cfb6-116">**Stoppa en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="6cfb6-116">**Stop a packet capture**</span></span>](#stop-a-packet-capture)
- [<span data-ttu-id="6cfb6-117">**Ta bort en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="6cfb6-117">**Delete a packet capture**</span></span>](#delete-a-packet-capture)
- [<span data-ttu-id="6cfb6-118">**Hämta en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="6cfb6-118">**Download a packet capture**</span></span>](#download-a-packet-capture)

## <a name="before-you-begin"></a><span data-ttu-id="6cfb6-119">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="6cfb6-119">Before you begin</span></span>

<span data-ttu-id="6cfb6-120">Den här artikeln förutsätter att du har hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="6cfb6-120">This article assumes you have hello following resources:</span></span>

* <span data-ttu-id="6cfb6-121">En instans Nätverksbevakaren i hello region som du vill toocreate en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="6cfb6-121">An instance of Network Watcher in hello region you want toocreate a packet capture</span></span>

* <span data-ttu-id="6cfb6-122">En virtuell dator med hello paket avbilda tillägget aktiverat.</span><span class="sxs-lookup"><span data-stu-id="6cfb6-122">A virtual machine with hello packet capture extension enabled.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6cfb6-123">Paketinsamling kräver ett tillägg för virtuell dator `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="6cfb6-123">Packet capture requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="6cfb6-124">Installera hello tillägg på en Windows VM finns [tillägg för virtuell dator i Azure Network Watcher Agent för Windows](../virtual-machines/windows/extensions-nwa.md) och för Linux VM besöka [tillägg för virtuell dator i Azure Network Watcher Agent för Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="6cfb6-124">For installing hello extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

## <a name="install-vm-extension"></a><span data-ttu-id="6cfb6-125">Installera tillägg för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="6cfb6-125">Install VM extension</span></span>

### <a name="step-1"></a><span data-ttu-id="6cfb6-126">Steg 1</span><span class="sxs-lookup"><span data-stu-id="6cfb6-126">Step 1</span></span>

```powershell
$VM = Get-AzureRmVM -ResourceGroupName testrg -Name VM1
```

### <a name="step-2"></a><span data-ttu-id="6cfb6-127">Steg 2</span><span class="sxs-lookup"><span data-stu-id="6cfb6-127">Step 2</span></span>

<span data-ttu-id="6cfb6-128">hello följande exempel hämtar hello tillägget information behövs toorun hello `Set-AzureRmVMExtension` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="6cfb6-128">hello following example retrieves hello extension information needed toorun hello `Set-AzureRmVMExtension` cmdlet.</span></span> <span data-ttu-id="6cfb6-129">Denna cmdlet installerar hello paket avbilda agent på hello virtuella gästdatorn.</span><span class="sxs-lookup"><span data-stu-id="6cfb6-129">This cmdlet installs hello packet capture agent on hello guest virtual machine.</span></span>

> [!NOTE]
> <span data-ttu-id="6cfb6-130">Hej `Set-AzureRmVMExtension` cmdlet kan ta flera minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="6cfb6-130">hello `Set-AzureRmVMExtension` cmdlet may take several minutes toocomplete.</span></span>

<span data-ttu-id="6cfb6-131">För Windows-datorer:</span><span class="sxs-lookup"><span data-stu-id="6cfb6-131">For Windows virtual machines:</span></span>

```powershell
$AzureNetworkWatcherExtension = Get-AzureRmVMExtensionImage -Location WestCentralUS -PublisherName Microsoft.Azure.NetworkWatcher -Type NetworkWatcherAgentWindows -Version 1.4.13.0
$ExtensionName = "AzureNetworkWatcherExtension"
Set-AzureRmVMExtension -ResourceGroupName $VM.ResourceGroupName  -Location $VM.Location -VMName $VM.Name -Name $ExtensionName -Publisher $AzureNetworkWatcherExtension.PublisherName -ExtensionType $AzureNetworkWatcherExtension.Type -TypeHandlerVersion $AzureNetworkWatcherExtension.Version.Substring(0,3)
```

<span data-ttu-id="6cfb6-132">För Linux virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="6cfb6-132">For Linux virtual machines:</span></span>

```powershell
$AzureNetworkWatcherExtension = Get-AzureRmVMExtensionImage -Location WestCentralUS -PublisherName Microsoft.Azure.NetworkWatcher -Type NetworkWatcherAgentLinux -Version 1.4.13.0
$ExtensionName = "AzureNetworkWatcherExtension"
Set-AzureRmVMExtension -ResourceGroupName $VM.ResourceGroupName  -Location $VM.Location -VMName $VM.Name -Name $ExtensionName -Publisher $AzureNetworkWatcherExtension.PublisherName -ExtensionType $AzureNetworkWatcherExtension.Type -TypeHandlerVersion $AzureNetworkWatcherExtension.Version.Substring(0,3)
````

<span data-ttu-id="6cfb6-133">hello följande exempel är ett lyckat svar när du har kört hello `Set-AzureRmVMExtension` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="6cfb6-133">hello following example is a successful response after running hello `Set-AzureRmVMExtension` cmdlet.</span></span>

```
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   
```

### <a name="step-3"></a><span data-ttu-id="6cfb6-134">Steg 3</span><span class="sxs-lookup"><span data-stu-id="6cfb6-134">Step 3</span></span>

<span data-ttu-id="6cfb6-135">tooensure som hello agenten är installerad, kör hello `Get-AzureRmVMExtension` cmdlet och skickar den hello virtuella datornamn och hello Tilläggsnamn.</span><span class="sxs-lookup"><span data-stu-id="6cfb6-135">tooensure that hello agent is installed, run hello `Get-AzureRmVMExtension` cmdlet and pass it hello virtual machine name and hello extension name.</span></span>

```powershell
Get-AzureRmVMExtension -ResourceGroupName $VM.ResourceGroupName  -VMName $VM.Name -Name $ExtensionName
```

<span data-ttu-id="6cfb6-136">hello följande exempel är ett exempel på hello svar från att köras`Get-AzureRmVMExtension`</span><span class="sxs-lookup"><span data-stu-id="6cfb6-136">hello following sample is an example of hello response from running `Get-AzureRmVMExtension`</span></span>

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

## <a name="start-a-packet-capture"></a><span data-ttu-id="6cfb6-137">Starta en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="6cfb6-137">Start a packet capture</span></span>

<span data-ttu-id="6cfb6-138">När hello föregående steg är klar kan är hello paket avbilda agenten installerad på hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="6cfb6-138">Once hello preceding steps are complete, hello packet capture agent is installed on hello virtual machine.</span></span>

### <a name="step-1"></a><span data-ttu-id="6cfb6-139">Steg 1</span><span class="sxs-lookup"><span data-stu-id="6cfb6-139">Step 1</span></span>

<span data-ttu-id="6cfb6-140">hello nästa steg är tooretrieve hello Nätverksbevakaren instans.</span><span class="sxs-lookup"><span data-stu-id="6cfb6-140">hello next step is tooretrieve hello Network Watcher instance.</span></span> <span data-ttu-id="6cfb6-141">Den här variabeln skickas toohello `New-AzureRmNetworkWatcherPacketCapture` cmdlet i steg 4.</span><span class="sxs-lookup"><span data-stu-id="6cfb6-141">This variable is passed toohello `New-AzureRmNetworkWatcherPacketCapture` cmdlet in step 4.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName  
```

### <a name="step-2"></a><span data-ttu-id="6cfb6-142">Steg 2</span><span class="sxs-lookup"><span data-stu-id="6cfb6-142">Step 2</span></span>

<span data-ttu-id="6cfb6-143">Hämta ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="6cfb6-143">Retrieve a storage account.</span></span> <span data-ttu-id="6cfb6-144">Det här lagringskontot är används toostore hello paket filen.</span><span class="sxs-lookup"><span data-stu-id="6cfb6-144">This storage account is used toostore hello packet capture file.</span></span>

```powershell
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName testrg -Name testrgsa123
```

### <a name="step-3"></a><span data-ttu-id="6cfb6-145">Steg 3</span><span class="sxs-lookup"><span data-stu-id="6cfb6-145">Step 3</span></span>

<span data-ttu-id="6cfb6-146">Filter kan vara används toolimit hello data som lagras av hello paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="6cfb6-146">Filters can be used toolimit hello data that is stored by hello packet capture.</span></span> <span data-ttu-id="6cfb6-147">hello ställer följande exempel in två filter.</span><span class="sxs-lookup"><span data-stu-id="6cfb6-147">hello following example sets up two filters.</span></span>  <span data-ttu-id="6cfb6-148">Ett filter samlar in utgående TCP-trafik från lokala IP 10.0.0.3 toodestination portarna 20, 80 och 443.</span><span class="sxs-lookup"><span data-stu-id="6cfb6-148">One filter collects outgoing TCP traffic only from local IP 10.0.0.3 toodestination ports 20, 80 and 443.</span></span>  <span data-ttu-id="6cfb6-149">hello andra filtret samlar in UDP-trafik.</span><span class="sxs-lookup"><span data-stu-id="6cfb6-149">hello second filter collects only UDP traffic.</span></span>

```powershell
$filter1 = New-AzureRmPacketCaptureFilterConfig -Protocol TCP -RemoteIPAddress "1.1.1.1-255.255.255" -LocalIPAddress "10.0.0.3" -LocalPort "1-65535" -RemotePort "20;80;443"
$filter2 = New-AzureRmPacketCaptureFilterConfig -Protocol UDP
```

> [!NOTE]
> <span data-ttu-id="6cfb6-150">Flera filter kan definieras för en paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="6cfb6-150">Multiple filters can be defined for a packet capture.</span></span>

### <a name="step-4"></a><span data-ttu-id="6cfb6-151">Steg 4</span><span class="sxs-lookup"><span data-stu-id="6cfb6-151">Step 4</span></span>

<span data-ttu-id="6cfb6-152">Kör hello `New-AzureRmNetworkWatcherPacketCapture` cmdlet toostart hello paket processen, skicka hello krävs värden hämtas i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="6cfb6-152">Run hello `New-AzureRmNetworkWatcherPacketCapture` cmdlet toostart hello packet capture process, passing hello required values retrieved in hello preceding steps.</span></span>
```powershell

New-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -TargetVirtualMachineId $vm.Id -PacketCaptureName "PacketCaptureTest" -StorageAccountId $storageAccount.id -TimeLimitInSeconds 60 -Filters $filter1, $filter2
```

<span data-ttu-id="6cfb6-153">hello följande exempel är hello förväntades utdata från kör hello `New-AzureRmNetworkWatcherPacketCapture` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="6cfb6-153">hello following example is hello expected output from running hello `New-AzureRmNetworkWatcherPacketCapture` cmdlet.</span></span>

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

## <a name="get-a-packet-capture"></a><span data-ttu-id="6cfb6-154">Hämta en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="6cfb6-154">Get a packet capture</span></span>

<span data-ttu-id="6cfb6-155">Kör hello `Get-AzureRmNetworkWatcherPacketCapture` cmdlet, hämtar hello status för en paketinsamling som körs eller har slutförts.</span><span class="sxs-lookup"><span data-stu-id="6cfb6-155">Running hello `Get-AzureRmNetworkWatcherPacketCapture` cmdlet, retrieves hello status of a currently running, or completed packet capture.</span></span>

```powershell
Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
```

<span data-ttu-id="6cfb6-156">hello följande exempel är hello utdata från hello `Get-AzureRmNetworkWatcherPacketCapture` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="6cfb6-156">hello following example is hello output from hello `Get-AzureRmNetworkWatcherPacketCapture` cmdlet.</span></span> <span data-ttu-id="6cfb6-157">hello följande exempel är när hello hämtningen är klar.</span><span class="sxs-lookup"><span data-stu-id="6cfb6-157">hello following example is after hello capture is complete.</span></span> <span data-ttu-id="6cfb6-158">Hej PacketCaptureStatus värdet stoppas med en StopReason TimeExceeded.</span><span class="sxs-lookup"><span data-stu-id="6cfb6-158">hello PacketCaptureStatus value is Stopped, with a StopReason of TimeExceeded.</span></span> <span data-ttu-id="6cfb6-159">Det här värdet visar att hello paketinsamling lyckades och kördes tiden.</span><span class="sxs-lookup"><span data-stu-id="6cfb6-159">This value shows that hello packet capture was successful and ran its time.</span></span>
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

## <a name="stop-a-packet-capture"></a><span data-ttu-id="6cfb6-160">Stoppa en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="6cfb6-160">Stop a packet capture</span></span>

<span data-ttu-id="6cfb6-161">Genom att köra hello `Stop-AzureRmNetworkWatcherPacketCapture` cmdlet, om en avbildningssessionen pågår den har stoppats.</span><span class="sxs-lookup"><span data-stu-id="6cfb6-161">By running hello `Stop-AzureRmNetworkWatcherPacketCapture` cmdlet, if a capture session is in progress it is stopped.</span></span>

```powershell
Stop-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
```

> [!NOTE]
> <span data-ttu-id="6cfb6-162">hello cmdlet returnerar inget svar när kördes på en pågående avbildningssessionen eller en befintlig session som redan har stoppats.</span><span class="sxs-lookup"><span data-stu-id="6cfb6-162">hello cmdlet returns no response when ran on a currently running capture session or an existing session that has already stopped.</span></span>

## <a name="delete-a-packet-capture"></a><span data-ttu-id="6cfb6-163">Ta bort en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="6cfb6-163">Delete a packet capture</span></span>

```powershell
Remove-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
```

> [!NOTE]
> <span data-ttu-id="6cfb6-164">Om du tar bort en paketinsamling tar inte bort hello-filen i hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="6cfb6-164">Deleting a packet capture does not delete hello file in hello storage account.</span></span>

## <a name="download-a-packet-capture"></a><span data-ttu-id="6cfb6-165">Hämta en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="6cfb6-165">Download a packet capture</span></span>

<span data-ttu-id="6cfb6-166">När paketet avbildningssessionen är klar kan hello avbilda filen vara överförda tooblob lagring eller tooa lokal fil på hello VM.</span><span class="sxs-lookup"><span data-stu-id="6cfb6-166">Once your packet capture session has completed, hello capture file can be uploaded tooblob storage or tooa local file on hello VM.</span></span> <span data-ttu-id="6cfb6-167">hello lagringsplats för hello paketinsamling har definierats vid skapande av hello.</span><span class="sxs-lookup"><span data-stu-id="6cfb6-167">hello storage location of hello packet capture is defined at creation of hello session.</span></span> <span data-ttu-id="6cfb6-168">En lämplig verktyget tooaccess dessa avbilda filer sparade tooa storage-konto är Microsoft Azure Lagringsutforskaren, som kan hämtas här: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="6cfb6-168">A convenient tool tooaccess these capture files saved tooa storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="6cfb6-169">Om ett lagringskonto har angetts sparas paket avbilda filer tooa storage-konto på hello följande plats:</span><span class="sxs-lookup"><span data-stu-id="6cfb6-169">If a storage account is specified, packet capture files are saved tooa storage account at hello following location:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a><span data-ttu-id="6cfb6-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6cfb6-170">Next steps</span></span>

<span data-ttu-id="6cfb6-171">Lär dig hur fångar tooautomate paket med virtuella aviseringar genom att visa [skapar en avisering utlösta paketinsamling](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="6cfb6-171">Learn how tooautomate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="6cfb6-172">Hitta om vissa trafik tillåts i orr utanför den virtuella datorn genom att besöka [Kontrollera Kontrollera IP-flöde](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="6cfb6-172">Find if certain traffic is allowed in orr out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<!-- Image references -->














