---
title: "Hantera paket insamlingar med Nätverksbevakaren Azure - Azure-portalen | Microsoft Docs"
description: "Den här sidan förklarar hur du hanterar funktionen paket avbildning i Nätverksbevakaren med hjälp av Azure portal"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 59edd945-34ad-4008-809e-ea904781d918
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 33390532cc4fc1129a4f960d589f41bc95e5a1ff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-the-portal"></a><span data-ttu-id="41048-103">Hantera paket insamlingar med Azure Nätverksbevakaren med hjälp av portalen</span><span class="sxs-lookup"><span data-stu-id="41048-103">Manage packet captures with Azure Network Watcher using the portal</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="41048-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="41048-104">Azure portal</span></span>](network-watcher-packet-capture-manage-portal.md)
> - [<span data-ttu-id="41048-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="41048-105">PowerShell</span></span>](network-watcher-packet-capture-manage-powershell.md)
> - [<span data-ttu-id="41048-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="41048-106">CLI 1.0</span></span>](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [<span data-ttu-id="41048-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="41048-107">CLI 2.0</span></span>](network-watcher-packet-capture-manage-cli.md)
> - [<span data-ttu-id="41048-108">Azure REST-API</span><span class="sxs-lookup"><span data-stu-id="41048-108">Azure REST API</span></span>](network-watcher-packet-capture-manage-rest.md)

<span data-ttu-id="41048-109">Nätverket Watcher paketinsamling kan du skapa avbildning sessioner för att spåra trafik till och från en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="41048-109">Network Watcher packet capture allows you to create capture sessions to track traffic to and from a virtual machine.</span></span> <span data-ttu-id="41048-110">Filter har angetts för hämtningens så du fångar upp trafiken som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="41048-110">Filters are provided for the capture session to ensure you capture only the traffic you want.</span></span> <span data-ttu-id="41048-111">Det hjälper dig för att diagnostisera nätverk avvikelser reaktivt och proaktivt paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="41048-111">Packet capture helps to diagnose network anomalies both reactively and proactively.</span></span> <span data-ttu-id="41048-112">Andra användningsområden omfattar att samla in nätverksstatistik får information om nätverket intrång felsöka klient-/ serverkommunikation och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="41048-112">Other uses include gathering network statistics, gaining information on network intrusions, to debug client-server communications and much more.</span></span> <span data-ttu-id="41048-113">Genom att via fjärranslutning utlösa paket insamlingar, underlättar den här funktionen för att köra en paketinsamling manuellt och på den önskade datorn, vilket sparar värdefull tid.</span><span class="sxs-lookup"><span data-stu-id="41048-113">By being able to remotely trigger packet captures, this capability eases the burden of running a packet capture manually and on the desired machine, which saves valuable time.</span></span>

<span data-ttu-id="41048-114">Den här artikeln tar dig igenom de olika administrativa uppgifter som är tillgängliga för paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="41048-114">This article takes you through the different management tasks that are currently available for packet capture.</span></span>

- [<span data-ttu-id="41048-115">**Starta en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="41048-115">**Start a packet capture**</span></span>](#start-a-packet-capture)
- [<span data-ttu-id="41048-116">**Stoppa en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="41048-116">**Stop a packet capture**</span></span>](#stop-a-packet-capture)
- [<span data-ttu-id="41048-117">**Ta bort en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="41048-117">**Delete a packet capture**</span></span>](#delete-a-packet-capture)
- [<span data-ttu-id="41048-118">**Hämta en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="41048-118">**Download a packet capture**</span></span>](#download-a-packet-capture)

## <a name="before-you-begin"></a><span data-ttu-id="41048-119">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="41048-119">Before you begin</span></span>

<span data-ttu-id="41048-120">Den här artikeln förutsätter att du har följande resurser:</span><span class="sxs-lookup"><span data-stu-id="41048-120">This article assumes that you have the following resources:</span></span>

- <span data-ttu-id="41048-121">En instans av Nätverksbevakaren i den region som du vill skapa en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="41048-121">An instance of Network Watcher in the region you want to create a packet capture</span></span>
- <span data-ttu-id="41048-122">En virtuell dator med filnamnstillägget paket avbilda aktiverad.</span><span class="sxs-lookup"><span data-stu-id="41048-122">A virtual machine with the packet capture extension enabled.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="41048-123">Paketinsamling kräver ett tillägg för virtuell dator `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="41048-123">Packet capture requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="41048-124">Installera tillägget på en Windows VM finns [tillägg för virtuell dator i Azure Network Watcher Agent för Windows](../virtual-machines/windows/extensions-nwa.md) och för Linux VM besöka [tillägg för virtuell dator i Azure Network Watcher Agent för Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="41048-124">For installing the extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

### <a name="packet-capture-agent-extension-through-the-portal"></a><span data-ttu-id="41048-125">Tillägget av paketet avbilda agent via portalen</span><span class="sxs-lookup"><span data-stu-id="41048-125">Packet Capture agent extension through the portal</span></span>

<span data-ttu-id="41048-126">För att installera paketet avbilda VM-agenten via portalen, navigerar du till den virtuella datorn, klickar du på **tillägg** > **Lägg till** och Sök efter **nätverk Watcher agenten för Windows**</span><span class="sxs-lookup"><span data-stu-id="41048-126">To install the packet capture VM agent through the portal, navigate to your virtual machine, click **Extensions** > **Add** and search for **Network Watcher Agent for Windows**</span></span>

![Visa Agent][agent]

## <a name="packet-capture-overview"></a><span data-ttu-id="41048-128">Översikt över avbildning av paket</span><span class="sxs-lookup"><span data-stu-id="41048-128">Packet Capture overview</span></span>

<span data-ttu-id="41048-129">Navigera till den [Azure-portalen](https://portal.azure.com) och på **nätverk** > **Nätverksbevakaren** > **paket avbilda**</span><span class="sxs-lookup"><span data-stu-id="41048-129">Navigate to the [Azure portal](https://portal.azure.com) and click **Networking** > **Network Watcher** > **Packet Capture**</span></span>

<span data-ttu-id="41048-130">Översiktssidan visas en lista över alla paket fångar som finns oavsett tillståndet.</span><span class="sxs-lookup"><span data-stu-id="41048-130">The overview page shows a list of all packet captures that exist no matter the state.</span></span>

> [!NOTE]
> <span data-ttu-id="41048-131">Paketinsamling kräver anslutning till lagringskontot via port 443.</span><span class="sxs-lookup"><span data-stu-id="41048-131">Packet capture requires connectivity to the storage account over port 443.</span></span>

![paketet avbilda översiktsskärm][1]

## <a name="start-a-packet-capture"></a><span data-ttu-id="41048-133">Starta en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="41048-133">Start a packet capture</span></span>

<span data-ttu-id="41048-134">Klicka på **Lägg till** att skapa en paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="41048-134">Click **Add** to create a packet capture.</span></span>

<span data-ttu-id="41048-135">De egenskaper som kan definieras på en paketinsamling är:</span><span class="sxs-lookup"><span data-stu-id="41048-135">The properties that can be defined on a packet capture are:</span></span>

<span data-ttu-id="41048-136">**Huvudinställningarna**</span><span class="sxs-lookup"><span data-stu-id="41048-136">**Main settings**</span></span>

- <span data-ttu-id="41048-137">**Prenumerationen** -värdet är den prenumeration som används, krävs en instans av nätverksbevakaren i varje prenumeration.</span><span class="sxs-lookup"><span data-stu-id="41048-137">**Subscription** - This value is the subscription that is used, an instance of network watcher is needed in each subscription.</span></span>
- <span data-ttu-id="41048-138">**Resursgruppen** -resursgruppen för den virtuella datorn som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="41048-138">**Resource group** - The resource group of the virtual machine that is being targeted.</span></span>
- <span data-ttu-id="41048-139">**Rikta virtuella** -den virtuella datorn som kör paketinsamling</span><span class="sxs-lookup"><span data-stu-id="41048-139">**Target virtual machine** - The virtual machine that is running the packet capture</span></span>
- <span data-ttu-id="41048-140">**Avbilda paketnamn** -värdet är namnet på paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="41048-140">**Packet capture name** - This value is the name of the packet capture.</span></span>

<span data-ttu-id="41048-141">**Samla in konfiguration**</span><span class="sxs-lookup"><span data-stu-id="41048-141">**Capture configuration**</span></span>

- <span data-ttu-id="41048-142">**Lagringskontot** -anger om paketinsamling sparas i ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="41048-142">**Storage Account** - Determines if packet capture is saved in a storage account.</span></span>
- <span data-ttu-id="41048-143">**Filen** -avgör om en paketinsamling sparas lokalt på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="41048-143">**File** - Determines if a packet capture is saved locally on the virtual machine.</span></span>
- <span data-ttu-id="41048-144">**Storage-konton** – markerat lagringskonto för att spara paketinsamling i.</span><span class="sxs-lookup"><span data-stu-id="41048-144">**Storage Accounts** - The selected storage account to save the packet capture in.</span></span> <span data-ttu-id="41048-145">Standardplatsen är name}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscription https://{storage konto-id} /resourcegroups/ {resursgruppens name}/providers/microsoft.compute/virtualmachines/{virtual datornamn} / {åå} / {MM} / {DD} / packetcapture_ {HH}_{MM}_{SS} _ {XXX} CAP.</span><span class="sxs-lookup"><span data-stu-id="41048-145">Default location is https://{storage account name}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscription id}/resourcegroups/{resource group name}/providers/microsoft.compute/virtualmachines/{virtual machine name}/{YY}/{MM}/{DD}/packetcapture_{HH}_{MM}_{SS}_{XXX}.cap.</span></span> <span data-ttu-id="41048-146">(Endast aktiverad om **lagring** har valts)</span><span class="sxs-lookup"><span data-stu-id="41048-146">(Only enabled if **Storage** is selected)</span></span>
- <span data-ttu-id="41048-147">**Lokal filsökväg** -lokal sökväg på en virtuell dator för att spara paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="41048-147">**Local file path** - The local path on a virtual machine to save the packet capture.</span></span> <span data-ttu-id="41048-148">(Endast aktiverad om **filen** har valts).</span><span class="sxs-lookup"><span data-stu-id="41048-148">(Only enabled if **File** is selected).</span></span> <span data-ttu-id="41048-149">En giltig sökväg måste anges</span><span class="sxs-lookup"><span data-stu-id="41048-149">A Valid path must be supplied</span></span>
- <span data-ttu-id="41048-150">**Maximalt antal byte per paket** - antalet byte från varje paket som har hämtats avbildas alla byte om värdet är tomt.</span><span class="sxs-lookup"><span data-stu-id="41048-150">**Maximum bytes per packet** - The number of bytes from each packet that are captured, all bytes are captured if left blank.</span></span>
- <span data-ttu-id="41048-151">**Maximalt antal byte per session** – Totalt antal byte som har hämtats när värdet är nådd paket avbilda stoppas.</span><span class="sxs-lookup"><span data-stu-id="41048-151">**Maximum bytes per session** - Total number of bytes that are captured, once the value is reached the packet capture stops.</span></span>
- <span data-ttu-id="41048-152">**Tidsgräns (sekunder)** -anger en tidsgräns för paketinsamling att stoppa.</span><span class="sxs-lookup"><span data-stu-id="41048-152">**Time limit (seconds)** - Sets a time limit for the packet capture to stop.</span></span> <span data-ttu-id="41048-153">Standardvärdet är 18000 sekunder.</span><span class="sxs-lookup"><span data-stu-id="41048-153">Default is 18000 seconds.</span></span>

> [!NOTE]
> <span data-ttu-id="41048-154">Premium-lagringskonton stöds inte för närvarande för lagring av paket avbildas.</span><span class="sxs-lookup"><span data-stu-id="41048-154">Premium storage accounts are currently not supported for storing packet captures.</span></span>

<span data-ttu-id="41048-155">**Filtrering (valfritt)**</span><span class="sxs-lookup"><span data-stu-id="41048-155">**Filtering (Optional)**</span></span>

- <span data-ttu-id="41048-156">**Protokollet** -protokollet för att filtrera för paket-avbildning.</span><span class="sxs-lookup"><span data-stu-id="41048-156">**Protocol** - The protocol to filter for the packet capture.</span></span> <span data-ttu-id="41048-157">Tillgängliga värden är TCP, UDP och alla.</span><span class="sxs-lookup"><span data-stu-id="41048-157">The available values are TCP, UDP, and Any.</span></span>
- <span data-ttu-id="41048-158">**Lokal IP-adress** -värdet filtrerar paketinsamling att paket där den lokala IP-adressen matchar den här filtervärdet.</span><span class="sxs-lookup"><span data-stu-id="41048-158">**Local IP address** - This value filters the packet capture to packets where the local IP address matches this filter value.</span></span>
- <span data-ttu-id="41048-159">**Lokal port** -värdet filtrerar paketinsamling att paket där den lokala porten matchar filtret värdet.</span><span class="sxs-lookup"><span data-stu-id="41048-159">**Local port** - This value filters the packet capture to packets where the local port matches this filter value.</span></span>
- <span data-ttu-id="41048-160">**IP-Fjärradress** -värdet filtrerar paketinsamling att paket där fjärranslutna IP-Adressen matchar den här filtervärdet.</span><span class="sxs-lookup"><span data-stu-id="41048-160">**Remote IP address** - This value filters the packet capture to packets where the remote IP matches this filter value.</span></span>
- <span data-ttu-id="41048-161">**Fjärrport** -värdet filtrerar paketinsamling att paket där fjärrporten matchar filtret värdet.</span><span class="sxs-lookup"><span data-stu-id="41048-161">**Remote port** - This value filters the packet capture to packets where the remote port matches this filter value.</span></span>

> [!NOTE]
> <span data-ttu-id="41048-162">Värden för porten och IP-adress kan vara ett värde, värdeintervallet eller en uppsättning.</span><span class="sxs-lookup"><span data-stu-id="41048-162">Port and IP address values can be a single value, range of values, or a set.</span></span> <span data-ttu-id="41048-163">(det vill säga 80 1024 för port) Du kan definiera så många filter som du vill.</span><span class="sxs-lookup"><span data-stu-id="41048-163">(that is, 80-1024 for port) You can define as many filters as you want.</span></span>

<span data-ttu-id="41048-164">När värdena fylls, klickar du på **OK** att skapa paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="41048-164">Once the values are filled out, click **OK** to create the packet capture.</span></span>

![Skapa en paketinsamling][2]

<span data-ttu-id="41048-166">När tidsgränsen på paketinsamling har upphört att gälla paketinsamling stoppas och kan granskas.</span><span class="sxs-lookup"><span data-stu-id="41048-166">After the time limit set on the packet capture has expired, the packet capture will stop and can be reviewed.</span></span> <span data-ttu-id="41048-167">Du kan också manuellt stoppa paket insamlingar sessionerna.</span><span class="sxs-lookup"><span data-stu-id="41048-167">You can also manually stop the packet captures sessions.</span></span>

## <a name="delete-a-packet-capture"></a><span data-ttu-id="41048-168">Ta bort en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="41048-168">Delete a packet capture</span></span>

<span data-ttu-id="41048-169">I vyn paket avbildning, klickar du på den **snabbmenyn** (...) eller högerklicka och klicka på **ta bort** att stoppa paketinsamling</span><span class="sxs-lookup"><span data-stu-id="41048-169">In the packet capture view, click the **context menu** (...) or right click, and click **delete** to stop the packet capture</span></span>

![Ta bort en paketinsamling][3]

> [!NOTE]
> <span data-ttu-id="41048-171">Om du tar bort en paketinsamling tar inte bort filen i lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="41048-171">Deleting a packet capture does not delete the file in the storage account.</span></span>

<span data-ttu-id="41048-172">Du uppmanas att bekräfta att du vill ta bort paketinsamling, klickar du på **Ja**</span><span class="sxs-lookup"><span data-stu-id="41048-172">You are asked to confirm you want to delete the packet capture, click **Yes**</span></span>

![Bekräftelse][4]

## <a name="stop-a-packet-capture"></a><span data-ttu-id="41048-174">Stoppa en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="41048-174">Stop a packet capture</span></span>

<span data-ttu-id="41048-175">I vyn paket avbildning, klickar du på den **snabbmenyn** (...) eller högerklicka och klicka på **stoppa** att stoppa paketinsamling</span><span class="sxs-lookup"><span data-stu-id="41048-175">In the packet capture view, click the **context menu** (...) or right click, and click **Stop** to stop the packet capture</span></span>

## <a name="download-a-packet-capture"></a><span data-ttu-id="41048-176">Hämta en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="41048-176">Download a packet capture</span></span>

<span data-ttu-id="41048-177">När paketet avbildningssessionen är klar överförs filen avbildning till blob-lagring eller till en lokal fil på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="41048-177">Once your packet capture session has completed, the capture file is uploaded to blob storage or to a local file on the VM.</span></span> <span data-ttu-id="41048-178">Lagringsplatsen för paketinsamling har definierats vid skapandet av sessionen.</span><span class="sxs-lookup"><span data-stu-id="41048-178">The storage location of the packet capture is defined at creation of the session.</span></span> <span data-ttu-id="41048-179">Ett enkelt verktyg för att komma åt dessa avbilda filer som sparats till ett lagringskonto är Microsoft Azure Lagringsutforskaren, som kan hämtas här: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="41048-179">A convenient tool to access these capture files saved to a storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="41048-180">Om ett storage-konto anges sparas paket avbilda filer till ett lagringskonto på följande plats:</span><span class="sxs-lookup"><span data-stu-id="41048-180">If a storage account is specified, packet capture files are saved to a storage account at the following location:</span></span>
```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a><span data-ttu-id="41048-181">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="41048-181">Next steps</span></span>

<span data-ttu-id="41048-182">Lär dig att automatisera insamlingar paket med virtuella aviseringar genom att visa [skapar en avisering utlösta paketinsamling](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="41048-182">Learn how to automate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="41048-183">Hitta om vissa trafik tillåts i eller utanför den virtuella datorn genom att besöka [Kontrollera Kontrollera IP-flöde](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="41048-183">Find if certain traffic is allowed in or out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<!-- Image references -->
[1]: ./media/network-watcher-packet-capture-manage-portal/figure1.png
[2]: ./media/network-watcher-packet-capture-manage-portal/figure2.png
[3]: ./media/network-watcher-packet-capture-manage-portal/figure3.png
[4]: ./media/network-watcher-packet-capture-manage-portal/figure4.png
[agent]: ./media/network-watcher-packet-capture-manage-portal/agent.png













