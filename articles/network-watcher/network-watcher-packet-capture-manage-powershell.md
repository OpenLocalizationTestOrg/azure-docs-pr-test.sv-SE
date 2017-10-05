---
title: "Hantera paket insamlingar med Azure Nätverksbevakaren - PowerShell | Microsoft Docs"
description: "Den här sidan förklarar hur du hanterar funktionen paket avbildning i Nätverksbevakaren med hjälp av PowerShell"
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
ms.openlocfilehash: abd3b3641da80ee835fac85b4bde68594449e451
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-powershell"></a><span data-ttu-id="4beef-103">Hantera paket insamlingar med Azure Nätverksbevakaren med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="4beef-103">Manage packet captures with Azure Network Watcher using PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="4beef-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4beef-104">Azure portal</span></span>](network-watcher-packet-capture-manage-portal.md)
> - [<span data-ttu-id="4beef-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4beef-105">PowerShell</span></span>](network-watcher-packet-capture-manage-powershell.md)
> - [<span data-ttu-id="4beef-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="4beef-106">CLI 1.0</span></span>](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [<span data-ttu-id="4beef-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="4beef-107">CLI 2.0</span></span>](network-watcher-packet-capture-manage-cli.md)
> - [<span data-ttu-id="4beef-108">Azure REST-API</span><span class="sxs-lookup"><span data-stu-id="4beef-108">Azure REST API</span></span>](network-watcher-packet-capture-manage-rest.md)

<span data-ttu-id="4beef-109">Nätverket Watcher paketinsamling kan du skapa avbildning sessioner för att spåra trafik till och från en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="4beef-109">Network Watcher packet capture allows you to create capture sessions to track traffic to and from a virtual machine.</span></span> <span data-ttu-id="4beef-110">Filter har angetts för hämtningens så du fångar upp trafiken som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="4beef-110">Filters are provided for the capture session to ensure you capture only the traffic you want.</span></span> <span data-ttu-id="4beef-111">Det hjälper dig för att diagnostisera nätverk avvikelser reaktivt och proaktivt paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="4beef-111">Packet capture helps to diagnose network anomalies both reactively and proactively.</span></span> <span data-ttu-id="4beef-112">Andra användningsområden omfattar att samla in nätverksstatistik får information om nätverket intrång felsöka klient-/ serverkommunikation och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="4beef-112">Other uses include gathering network statistics, gaining information on network intrusions, to debug client-server communications and much more.</span></span> <span data-ttu-id="4beef-113">Genom att via fjärranslutning utlösa paket insamlingar, underlättar den här funktionen för att köra en paketinsamling manuellt och på den önskade datorn, vilket sparar värdefull tid.</span><span class="sxs-lookup"><span data-stu-id="4beef-113">By being able to remotely trigger packet captures, this capability eases the burden of running a packet capture manually and on the desired machine, which saves valuable time.</span></span>

<span data-ttu-id="4beef-114">Den här artikeln tar dig igenom de olika administrativa uppgifter som är tillgängliga för paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="4beef-114">This article takes you through the different management tasks that are currently available for packet capture.</span></span>

- [<span data-ttu-id="4beef-115">**Starta en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="4beef-115">**Start a packet capture**</span></span>](#start-a-packet-capture)
- [<span data-ttu-id="4beef-116">**Stoppa en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="4beef-116">**Stop a packet capture**</span></span>](#stop-a-packet-capture)
- [<span data-ttu-id="4beef-117">**Ta bort en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="4beef-117">**Delete a packet capture**</span></span>](#delete-a-packet-capture)
- [<span data-ttu-id="4beef-118">**Hämta en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="4beef-118">**Download a packet capture**</span></span>](#download-a-packet-capture)

## <a name="before-you-begin"></a><span data-ttu-id="4beef-119">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="4beef-119">Before you begin</span></span>

<span data-ttu-id="4beef-120">Den här artikeln förutsätter att du har följande resurser:</span><span class="sxs-lookup"><span data-stu-id="4beef-120">This article assumes you have the following resources:</span></span>

* <span data-ttu-id="4beef-121">En instans av Nätverksbevakaren i den region som du vill skapa en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="4beef-121">An instance of Network Watcher in the region you want to create a packet capture</span></span>

* <span data-ttu-id="4beef-122">En virtuell dator med filnamnstillägget paket avbilda aktiverad.</span><span class="sxs-lookup"><span data-stu-id="4beef-122">A virtual machine with the packet capture extension enabled.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4beef-123">Paketinsamling kräver ett tillägg för virtuell dator `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="4beef-123">Packet capture requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="4beef-124">Installera tillägget på en Windows VM finns [tillägg för virtuell dator i Azure Network Watcher Agent för Windows](../virtual-machines/windows/extensions-nwa.md) och för Linux VM besöka [tillägg för virtuell dator i Azure Network Watcher Agent för Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="4beef-124">For installing the extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

## <a name="install-vm-extension"></a><span data-ttu-id="4beef-125">Installera tillägg för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="4beef-125">Install VM extension</span></span>

### <a name="step-1"></a><span data-ttu-id="4beef-126">Steg 1</span><span class="sxs-lookup"><span data-stu-id="4beef-126">Step 1</span></span>

```powershell
$VM = Get-AzureRmVM -ResourceGroupName testrg -Name VM1
```

### <a name="step-2"></a><span data-ttu-id="4beef-127">Steg 2</span><span class="sxs-lookup"><span data-stu-id="4beef-127">Step 2</span></span>

<span data-ttu-id="4beef-128">I följande exempel hämtar tillägget information som behövs för att köra den `Set-AzureRmVMExtension` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="4beef-128">The following example retrieves the extension information needed to run the `Set-AzureRmVMExtension` cmdlet.</span></span> <span data-ttu-id="4beef-129">Denna cmdlet installerar paketet avbilda agent på den virtuella gästdatorn.</span><span class="sxs-lookup"><span data-stu-id="4beef-129">This cmdlet installs the packet capture agent on the guest virtual machine.</span></span>

> [!NOTE]
> <span data-ttu-id="4beef-130">Den `Set-AzureRmVMExtension` cmdlet kan ta flera minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="4beef-130">The `Set-AzureRmVMExtension` cmdlet may take several minutes to complete.</span></span>

<span data-ttu-id="4beef-131">För Windows-datorer:</span><span class="sxs-lookup"><span data-stu-id="4beef-131">For Windows virtual machines:</span></span>

```powershell
$AzureNetworkWatcherExtension = Get-AzureRmVMExtensionImage -Location WestCentralUS -PublisherName Microsoft.Azure.NetworkWatcher -Type NetworkWatcherAgentWindows -Version 1.4.13.0
$ExtensionName = "AzureNetworkWatcherExtension"
Set-AzureRmVMExtension -ResourceGroupName $VM.ResourceGroupName  -Location $VM.Location -VMName $VM.Name -Name $ExtensionName -Publisher $AzureNetworkWatcherExtension.PublisherName -ExtensionType $AzureNetworkWatcherExtension.Type -TypeHandlerVersion $AzureNetworkWatcherExtension.Version.Substring(0,3)
```

<span data-ttu-id="4beef-132">För Linux virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="4beef-132">For Linux virtual machines:</span></span>

```powershell
$AzureNetworkWatcherExtension = Get-AzureRmVMExtensionImage -Location WestCentralUS -PublisherName Microsoft.Azure.NetworkWatcher -Type NetworkWatcherAgentLinux -Version 1.4.13.0
$ExtensionName = "AzureNetworkWatcherExtension"
Set-AzureRmVMExtension -ResourceGroupName $VM.ResourceGroupName  -Location $VM.Location -VMName $VM.Name -Name $ExtensionName -Publisher $AzureNetworkWatcherExtension.PublisherName -ExtensionType $AzureNetworkWatcherExtension.Type -TypeHandlerVersion $AzureNetworkWatcherExtension.Version.Substring(0,3)
````

<span data-ttu-id="4beef-133">Följande exempel är ett lyckat svar när du har kört den `Set-AzureRmVMExtension` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="4beef-133">The following example is a successful response after running the `Set-AzureRmVMExtension` cmdlet.</span></span>

```
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   
```

### <a name="step-3"></a><span data-ttu-id="4beef-134">Steg 3</span><span class="sxs-lookup"><span data-stu-id="4beef-134">Step 3</span></span>

<span data-ttu-id="4beef-135">För att säkerställa att agenten är installerad, kör den `Get-AzureRmVMExtension` cmdlet och överför den virtuella datornamn och namnet.</span><span class="sxs-lookup"><span data-stu-id="4beef-135">To ensure that the agent is installed, run the `Get-AzureRmVMExtension` cmdlet and pass it the virtual machine name and the extension name.</span></span>

```powershell
Get-AzureRmVMExtension -ResourceGroupName $VM.ResourceGroupName  -VMName $VM.Name -Name $ExtensionName
```

<span data-ttu-id="4beef-136">I följande exempel är ett exempel på svar från att köras`Get-AzureRmVMExtension`</span><span class="sxs-lookup"><span data-stu-id="4beef-136">The following sample is an example of the response from running `Get-AzureRmVMExtension`</span></span>

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

## <a name="start-a-packet-capture"></a><span data-ttu-id="4beef-137">Starta en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="4beef-137">Start a packet capture</span></span>

<span data-ttu-id="4beef-138">När de föregående stegen är klar kan är paket avbilda agenten installerad på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="4beef-138">Once the preceding steps are complete, the packet capture agent is installed on the virtual machine.</span></span>

### <a name="step-1"></a><span data-ttu-id="4beef-139">Steg 1</span><span class="sxs-lookup"><span data-stu-id="4beef-139">Step 1</span></span>

<span data-ttu-id="4beef-140">Nästa steg är att hämta Nätverksbevakaren-instans.</span><span class="sxs-lookup"><span data-stu-id="4beef-140">The next step is to retrieve the Network Watcher instance.</span></span> <span data-ttu-id="4beef-141">Den här variabeln har överförts till den `New-AzureRmNetworkWatcherPacketCapture` cmdlet i steg 4.</span><span class="sxs-lookup"><span data-stu-id="4beef-141">This variable is passed to the `New-AzureRmNetworkWatcherPacketCapture` cmdlet in step 4.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName  
```

### <a name="step-2"></a><span data-ttu-id="4beef-142">Steg 2</span><span class="sxs-lookup"><span data-stu-id="4beef-142">Step 2</span></span>

<span data-ttu-id="4beef-143">Hämta ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="4beef-143">Retrieve a storage account.</span></span> <span data-ttu-id="4beef-144">Det här lagringskontot används för att spara filen i paketet.</span><span class="sxs-lookup"><span data-stu-id="4beef-144">This storage account is used to store the packet capture file.</span></span>

```powershell
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName testrg -Name testrgsa123
```

### <a name="step-3"></a><span data-ttu-id="4beef-145">Steg 3</span><span class="sxs-lookup"><span data-stu-id="4beef-145">Step 3</span></span>

<span data-ttu-id="4beef-146">Filter kan användas för att begränsa de data som lagras med paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="4beef-146">Filters can be used to limit the data that is stored by the packet capture.</span></span> <span data-ttu-id="4beef-147">I följande exempel ställer in två filter.</span><span class="sxs-lookup"><span data-stu-id="4beef-147">The following example sets up two filters.</span></span>  <span data-ttu-id="4beef-148">Ett filter samlar in utgående TCP-trafik från lokala IP 10.0.0.3 för målportar 20, 80 och 443.</span><span class="sxs-lookup"><span data-stu-id="4beef-148">One filter collects outgoing TCP traffic only from local IP 10.0.0.3 to destination ports 20, 80 and 443.</span></span>  <span data-ttu-id="4beef-149">Det andra filtret samlar in UDP-trafik.</span><span class="sxs-lookup"><span data-stu-id="4beef-149">The second filter collects only UDP traffic.</span></span>

```powershell
$filter1 = New-AzureRmPacketCaptureFilterConfig -Protocol TCP -RemoteIPAddress "1.1.1.1-255.255.255" -LocalIPAddress "10.0.0.3" -LocalPort "1-65535" -RemotePort "20;80;443"
$filter2 = New-AzureRmPacketCaptureFilterConfig -Protocol UDP
```

> [!NOTE]
> <span data-ttu-id="4beef-150">Flera filter kan definieras för en paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="4beef-150">Multiple filters can be defined for a packet capture.</span></span>

### <a name="step-4"></a><span data-ttu-id="4beef-151">Steg 4</span><span class="sxs-lookup"><span data-stu-id="4beef-151">Step 4</span></span>

<span data-ttu-id="4beef-152">Kör den `New-AzureRmNetworkWatcherPacketCapture` att starta avbildningsprocessen paket skickas nödvändiga värden hämtas i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="4beef-152">Run the `New-AzureRmNetworkWatcherPacketCapture` cmdlet to start the packet capture process, passing the required values retrieved in the preceding steps.</span></span>
```powershell

New-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -TargetVirtualMachineId $vm.Id -PacketCaptureName "PacketCaptureTest" -StorageAccountId $storageAccount.id -TimeLimitInSeconds 60 -Filters $filter1, $filter2
```

<span data-ttu-id="4beef-153">Följande är exempel på utdata som förväntas från att köras i `New-AzureRmNetworkWatcherPacketCapture` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="4beef-153">The following example is the expected output from running the `New-AzureRmNetworkWatcherPacketCapture` cmdlet.</span></span>

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

## <a name="get-a-packet-capture"></a><span data-ttu-id="4beef-154">Hämta en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="4beef-154">Get a packet capture</span></span>

<span data-ttu-id="4beef-155">Kör den `Get-AzureRmNetworkWatcherPacketCapture` cmdlet, hämtar status för en paketinsamling som körs eller har slutförts.</span><span class="sxs-lookup"><span data-stu-id="4beef-155">Running the `Get-AzureRmNetworkWatcherPacketCapture` cmdlet, retrieves the status of a currently running, or completed packet capture.</span></span>

```powershell
Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
```

<span data-ttu-id="4beef-156">Följande är exempel på utdata från den `Get-AzureRmNetworkWatcherPacketCapture` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="4beef-156">The following example is the output from the `Get-AzureRmNetworkWatcherPacketCapture` cmdlet.</span></span> <span data-ttu-id="4beef-157">I följande exempel är när avbildningen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="4beef-157">The following example is after the capture is complete.</span></span> <span data-ttu-id="4beef-158">Värdet för PacketCaptureStatus stoppas med en StopReason TimeExceeded.</span><span class="sxs-lookup"><span data-stu-id="4beef-158">The PacketCaptureStatus value is Stopped, with a StopReason of TimeExceeded.</span></span> <span data-ttu-id="4beef-159">Det här värdet visar att paketinsamling lyckades och kördes tiden.</span><span class="sxs-lookup"><span data-stu-id="4beef-159">This value shows that the packet capture was successful and ran its time.</span></span>
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

## <a name="stop-a-packet-capture"></a><span data-ttu-id="4beef-160">Stoppa en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="4beef-160">Stop a packet capture</span></span>

<span data-ttu-id="4beef-161">Genom att köra den `Stop-AzureRmNetworkWatcherPacketCapture` cmdlet, om en avbildningssessionen pågår den har stoppats.</span><span class="sxs-lookup"><span data-stu-id="4beef-161">By running the `Stop-AzureRmNetworkWatcherPacketCapture` cmdlet, if a capture session is in progress it is stopped.</span></span>

```powershell
Stop-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
```

> [!NOTE]
> <span data-ttu-id="4beef-162">Cmdleten returnerar inga svar när kördes på en pågående avbildningssessionen eller en befintlig session som redan har stoppats.</span><span class="sxs-lookup"><span data-stu-id="4beef-162">The cmdlet returns no response when ran on a currently running capture session or an existing session that has already stopped.</span></span>

## <a name="delete-a-packet-capture"></a><span data-ttu-id="4beef-163">Ta bort en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="4beef-163">Delete a packet capture</span></span>

```powershell
Remove-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
```

> [!NOTE]
> <span data-ttu-id="4beef-164">Om du tar bort en paketinsamling tar inte bort filen i lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="4beef-164">Deleting a packet capture does not delete the file in the storage account.</span></span>

## <a name="download-a-packet-capture"></a><span data-ttu-id="4beef-165">Hämta en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="4beef-165">Download a packet capture</span></span>

<span data-ttu-id="4beef-166">När paketet avbildningssessionen är klar, kan avbilda filen laddas upp till blob-lagring eller till en lokal fil på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="4beef-166">Once your packet capture session has completed, the capture file can be uploaded to blob storage or to a local file on the VM.</span></span> <span data-ttu-id="4beef-167">Lagringsplatsen för paketinsamling har definierats vid skapandet av sessionen.</span><span class="sxs-lookup"><span data-stu-id="4beef-167">The storage location of the packet capture is defined at creation of the session.</span></span> <span data-ttu-id="4beef-168">Ett enkelt verktyg för att komma åt dessa avbilda filer som sparats till ett lagringskonto är Microsoft Azure Lagringsutforskaren, som kan hämtas här: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="4beef-168">A convenient tool to access these capture files saved to a storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="4beef-169">Om ett storage-konto anges sparas paket avbilda filer till ett lagringskonto på följande plats:</span><span class="sxs-lookup"><span data-stu-id="4beef-169">If a storage account is specified, packet capture files are saved to a storage account at the following location:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a><span data-ttu-id="4beef-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4beef-170">Next steps</span></span>

<span data-ttu-id="4beef-171">Lär dig att automatisera insamlingar paket med virtuella aviseringar genom att visa [skapar en avisering utlösta paketinsamling](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="4beef-171">Learn how to automate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="4beef-172">Hitta om vissa trafik tillåts i orr utanför den virtuella datorn genom att besöka [Kontrollera Kontrollera IP-flöde](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="4beef-172">Find if certain traffic is allowed in orr out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<!-- Image references -->














