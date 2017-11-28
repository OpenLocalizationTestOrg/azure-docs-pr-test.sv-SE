---
title: "aaaManage paket som samlar in med Azure Nätverksbevakaren - Azure-portalen | Microsoft Docs"
description: "Den här sidan förklarar hur toomanage hello paket avbilda funktion i Nätverksbevakaren med hjälp av Azure portal"
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
ms.openlocfilehash: 431b145ee215b8d9421fb2aacdce08a0de90b35e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-hello-portal"></a><span data-ttu-id="7fa7c-103">Hantera paket insamlingar med Azure Nätverksbevakaren med hello-portalen</span><span class="sxs-lookup"><span data-stu-id="7fa7c-103">Manage packet captures with Azure Network Watcher using hello portal</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="7fa7c-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7fa7c-104">Azure portal</span></span>](network-watcher-packet-capture-manage-portal.md)
> - [<span data-ttu-id="7fa7c-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="7fa7c-105">PowerShell</span></span>](network-watcher-packet-capture-manage-powershell.md)
> - [<span data-ttu-id="7fa7c-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="7fa7c-106">CLI 1.0</span></span>](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [<span data-ttu-id="7fa7c-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="7fa7c-107">CLI 2.0</span></span>](network-watcher-packet-capture-manage-cli.md)
> - [<span data-ttu-id="7fa7c-108">Azure REST-API</span><span class="sxs-lookup"><span data-stu-id="7fa7c-108">Azure REST API</span></span>](network-watcher-packet-capture-manage-rest.md)

<span data-ttu-id="7fa7c-109">Nätverket Watcher paketinsamling kan toocreate avbilda sessioner tootrack trafik tooand från en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-109">Network Watcher packet capture allows you toocreate capture sessions tootrack traffic tooand from a virtual machine.</span></span> <span data-ttu-id="7fa7c-110">Filter har angetts för hello avbilda session tooensure du fånga in endast hello trafik som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-110">Filters are provided for hello capture session tooensure you capture only hello traffic you want.</span></span> <span data-ttu-id="7fa7c-111">Paketinsamling hjälper toodiagnose nätverk avvikelser reaktivt och proaktivt.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-111">Packet capture helps toodiagnose network anomalies both reactively and proactively.</span></span> <span data-ttu-id="7fa7c-112">Andra användningsområden omfattar att samla in nätverksstatistik får information om nätverket intrång, toodebug klient / server-kommunikation och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-112">Other uses include gathering network statistics, gaining information on network intrusions, toodebug client-server communications and much more.</span></span> <span data-ttu-id="7fa7c-113">Den här funktionen underlättar genom kan tooremotely utlösaren paket insamlingar, hello belastningen körs en paketinsamling manuellt och på hello önskade dator som sparar värdefull tid.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-113">By being able tooremotely trigger packet captures, this capability eases hello burden of running a packet capture manually and on hello desired machine, which saves valuable time.</span></span>

<span data-ttu-id="7fa7c-114">Den här artikeln tar dig igenom hello olika administrativa uppgifter som är tillgängliga för paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-114">This article takes you through hello different management tasks that are currently available for packet capture.</span></span>

- [<span data-ttu-id="7fa7c-115">**Starta en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="7fa7c-115">**Start a packet capture**</span></span>](#start-a-packet-capture)
- [<span data-ttu-id="7fa7c-116">**Stoppa en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="7fa7c-116">**Stop a packet capture**</span></span>](#stop-a-packet-capture)
- [<span data-ttu-id="7fa7c-117">**Ta bort en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="7fa7c-117">**Delete a packet capture**</span></span>](#delete-a-packet-capture)
- [<span data-ttu-id="7fa7c-118">**Hämta en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="7fa7c-118">**Download a packet capture**</span></span>](#download-a-packet-capture)

## <a name="before-you-begin"></a><span data-ttu-id="7fa7c-119">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="7fa7c-119">Before you begin</span></span>

<span data-ttu-id="7fa7c-120">Den här artikeln förutsätter att du har hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="7fa7c-120">This article assumes that you have hello following resources:</span></span>

- <span data-ttu-id="7fa7c-121">En instans Nätverksbevakaren i hello region som du vill toocreate en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="7fa7c-121">An instance of Network Watcher in hello region you want toocreate a packet capture</span></span>
- <span data-ttu-id="7fa7c-122">En virtuell dator med hello paket avbilda tillägget aktiverat.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-122">A virtual machine with hello packet capture extension enabled.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7fa7c-123">Paketinsamling kräver ett tillägg för virtuell dator `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-123">Packet capture requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="7fa7c-124">Installera hello tillägg på en Windows VM finns [tillägg för virtuell dator i Azure Network Watcher Agent för Windows](../virtual-machines/windows/extensions-nwa.md) och för Linux VM besöka [tillägg för virtuell dator i Azure Network Watcher Agent för Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="7fa7c-124">For installing hello extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

### <a name="packet-capture-agent-extension-through-hello-portal"></a><span data-ttu-id="7fa7c-125">Tillägget av paketet avbilda agent hello-portalen</span><span class="sxs-lookup"><span data-stu-id="7fa7c-125">Packet Capture agent extension through hello portal</span></span>

<span data-ttu-id="7fa7c-126">tooinstall hello paketinsamling VM-agenten via hello portal, navigerar tooyour virtuella datorn, klickar på **tillägg** > **Lägg till** och Sök efter **Network Watcher Agent för Windows**</span><span class="sxs-lookup"><span data-stu-id="7fa7c-126">tooinstall hello packet capture VM agent through hello portal, navigate tooyour virtual machine, click **Extensions** > **Add** and search for **Network Watcher Agent for Windows**</span></span>

![Visa Agent][agent]

## <a name="packet-capture-overview"></a><span data-ttu-id="7fa7c-128">Översikt över avbildning av paket</span><span class="sxs-lookup"><span data-stu-id="7fa7c-128">Packet Capture overview</span></span>

<span data-ttu-id="7fa7c-129">Navigera toohello [Azure-portalen](https://portal.azure.com) och på **nätverk** > **Nätverksbevakaren** > **paket avbilda**</span><span class="sxs-lookup"><span data-stu-id="7fa7c-129">Navigate toohello [Azure portal](https://portal.azure.com) and click **Networking** > **Network Watcher** > **Packet Capture**</span></span>

<span data-ttu-id="7fa7c-130">hello översiktssidan visar en lista över alla paket fångar som finns oavsett hello tillstånd.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-130">hello overview page shows a list of all packet captures that exist no matter hello state.</span></span>

> [!NOTE]
> <span data-ttu-id="7fa7c-131">Paketinsamling kräver anslutning toohello storage-konto via port 443.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-131">Packet capture requires connectivity toohello storage account over port 443.</span></span>

![paketet avbilda översiktsskärm][1]

## <a name="start-a-packet-capture"></a><span data-ttu-id="7fa7c-133">Starta en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="7fa7c-133">Start a packet capture</span></span>

<span data-ttu-id="7fa7c-134">Klicka på **Lägg till** toocreate en paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-134">Click **Add** toocreate a packet capture.</span></span>

<span data-ttu-id="7fa7c-135">hello-egenskaper som kan definieras på en paketinsamling är:</span><span class="sxs-lookup"><span data-stu-id="7fa7c-135">hello properties that can be defined on a packet capture are:</span></span>

<span data-ttu-id="7fa7c-136">**Huvudinställningarna**</span><span class="sxs-lookup"><span data-stu-id="7fa7c-136">**Main settings**</span></span>

- <span data-ttu-id="7fa7c-137">**Prenumerationen** -värdet är hello-prenumeration som används, krävs en instans av nätverksbevakaren i varje prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-137">**Subscription** - This value is hello subscription that is used, an instance of network watcher is needed in each subscription.</span></span>
- <span data-ttu-id="7fa7c-138">**Resursgruppen** -hello resursgruppen av hello virtuell dator som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-138">**Resource group** - hello resource group of hello virtual machine that is being targeted.</span></span>
- <span data-ttu-id="7fa7c-139">**Rikta virtuella** -hello virtuell dator som kör hello paketinsamling</span><span class="sxs-lookup"><span data-stu-id="7fa7c-139">**Target virtual machine** - hello virtual machine that is running hello packet capture</span></span>
- <span data-ttu-id="7fa7c-140">**Avbilda paketnamn** -värdet är hello paketinsamling hello namn.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-140">**Packet capture name** - This value is hello name of hello packet capture.</span></span>

<span data-ttu-id="7fa7c-141">**Samla in konfiguration**</span><span class="sxs-lookup"><span data-stu-id="7fa7c-141">**Capture configuration**</span></span>

- <span data-ttu-id="7fa7c-142">**Lagringskontot** -anger om paketinsamling sparas i ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-142">**Storage Account** - Determines if packet capture is saved in a storage account.</span></span>
- <span data-ttu-id="7fa7c-143">**Filen** -avgör om en paketinsamling sparas lokalt på hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-143">**File** - Determines if a packet capture is saved locally on hello virtual machine.</span></span>
- <span data-ttu-id="7fa7c-144">**Storage-konton** -hello valt paketinsamling för storage-konto toosave hello i.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-144">**Storage Accounts** - hello selected storage account toosave hello packet capture in.</span></span> <span data-ttu-id="7fa7c-145">Standardplatsen är name}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscription https://{storage konto-id} /resourcegroups/ {resursgruppens name}/providers/microsoft.compute/virtualmachines/{virtual datornamn} / {åå} / {MM} / {DD} / packetcapture_ {HH}_{MM}_{SS} _ {XXX} CAP.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-145">Default location is https://{storage account name}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscription id}/resourcegroups/{resource group name}/providers/microsoft.compute/virtualmachines/{virtual machine name}/{YY}/{MM}/{DD}/packetcapture_{HH}_{MM}_{SS}_{XXX}.cap.</span></span> <span data-ttu-id="7fa7c-146">(Endast aktiverad om **lagring** har valts)</span><span class="sxs-lookup"><span data-stu-id="7fa7c-146">(Only enabled if **Storage** is selected)</span></span>
- <span data-ttu-id="7fa7c-147">**Lokal filsökväg** -hello lokal sökväg på en virtuell dator toosave hello paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-147">**Local file path** - hello local path on a virtual machine toosave hello packet capture.</span></span> <span data-ttu-id="7fa7c-148">(Endast aktiverad om **filen** har valts).</span><span class="sxs-lookup"><span data-stu-id="7fa7c-148">(Only enabled if **File** is selected).</span></span> <span data-ttu-id="7fa7c-149">En giltig sökväg måste anges</span><span class="sxs-lookup"><span data-stu-id="7fa7c-149">A Valid path must be supplied</span></span>
- <span data-ttu-id="7fa7c-150">**Maximalt antal byte per paket** -hello antalet byte från varje paket som har hämtats avbildas alla byte om värdet är tomt.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-150">**Maximum bytes per packet** - hello number of bytes from each packet that are captured, all bytes are captured if left blank.</span></span>
- <span data-ttu-id="7fa7c-151">**Maximalt antal byte per session** – Totalt antal byte som har hämtats när hello-värdet har nåtts hello paket avbilda stoppas.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-151">**Maximum bytes per session** - Total number of bytes that are captured, once hello value is reached hello packet capture stops.</span></span>
- <span data-ttu-id="7fa7c-152">**Tidsgräns (sekunder)** -anger en tidsgräns för hello paket avbilda toostop.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-152">**Time limit (seconds)** - Sets a time limit for hello packet capture toostop.</span></span> <span data-ttu-id="7fa7c-153">Standardvärdet är 18000 sekunder.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-153">Default is 18000 seconds.</span></span>

> [!NOTE]
> <span data-ttu-id="7fa7c-154">Premium-lagringskonton stöds inte för närvarande för lagring av paket avbildas.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-154">Premium storage accounts are currently not supported for storing packet captures.</span></span>

<span data-ttu-id="7fa7c-155">**Filtrering (valfritt)**</span><span class="sxs-lookup"><span data-stu-id="7fa7c-155">**Filtering (Optional)**</span></span>

- <span data-ttu-id="7fa7c-156">**Protokollet** -hello protokollet toofilter för hello paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-156">**Protocol** - hello protocol toofilter for hello packet capture.</span></span> <span data-ttu-id="7fa7c-157">hello tillgängliga värden är TCP, UDP och alla.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-157">hello available values are TCP, UDP, and Any.</span></span>
- <span data-ttu-id="7fa7c-158">**Lokal IP-adress** -värdet filtrerar hello paket avbilda toopackets där hello lokala IP-adressen matchar den här filtervärdet.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-158">**Local IP address** - This value filters hello packet capture toopackets where hello local IP address matches this filter value.</span></span>
- <span data-ttu-id="7fa7c-159">**Lokal port** -värdet filtrerar hello paket avbilda toopackets där hello lokal port matchar filtret värdet.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-159">**Local port** - This value filters hello packet capture toopackets where hello local port matches this filter value.</span></span>
- <span data-ttu-id="7fa7c-160">**IP-Fjärradress** -värdet filtrerar hello paket avbilda toopackets där hello fjärr-IP-matchar filtret värdet.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-160">**Remote IP address** - This value filters hello packet capture toopackets where hello remote IP matches this filter value.</span></span>
- <span data-ttu-id="7fa7c-161">**Fjärrport** -värdet filtrerar hello paket avbilda toopackets där hello Fjärrport matchar filtret värdet.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-161">**Remote port** - This value filters hello packet capture toopackets where hello remote port matches this filter value.</span></span>

> [!NOTE]
> <span data-ttu-id="7fa7c-162">Värden för porten och IP-adress kan vara ett värde, värdeintervallet eller en uppsättning.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-162">Port and IP address values can be a single value, range of values, or a set.</span></span> <span data-ttu-id="7fa7c-163">(det vill säga 80 1024 för port) Du kan definiera så många filter som du vill.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-163">(that is, 80-1024 for port) You can define as many filters as you want.</span></span>

<span data-ttu-id="7fa7c-164">När hello värdena fylls, klickar du på **OK** toocreate hello paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-164">Once hello values are filled out, click **OK** toocreate hello packet capture.</span></span>

![Skapa en paketinsamling][2]

<span data-ttu-id="7fa7c-166">När tidsgränsen för hello har angett för hello paketinsamling har upphört att gälla, hello paketinsamling stoppas och kan granskas.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-166">After hello time limit set on hello packet capture has expired, hello packet capture will stop and can be reviewed.</span></span> <span data-ttu-id="7fa7c-167">Du kan också manuellt stoppa hello paket insamlingar sessionerna.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-167">You can also manually stop hello packet captures sessions.</span></span>

## <a name="delete-a-packet-capture"></a><span data-ttu-id="7fa7c-168">Ta bort en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="7fa7c-168">Delete a packet capture</span></span>

<span data-ttu-id="7fa7c-169">Samla in vyn i hello-paket, klickar du på hello **snabbmenyn** (...) eller högerklicka och klicka på **ta bort** toostop hello paketinsamling</span><span class="sxs-lookup"><span data-stu-id="7fa7c-169">In hello packet capture view, click hello **context menu** (...) or right click, and click **delete** toostop hello packet capture</span></span>

![Ta bort en paketinsamling][3]

> [!NOTE]
> <span data-ttu-id="7fa7c-171">Om du tar bort en paketinsamling tar inte bort hello-filen i hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-171">Deleting a packet capture does not delete hello file in hello storage account.</span></span>

<span data-ttu-id="7fa7c-172">Du uppmanas tooconfirm som du vill toodelete hello paketinsamling, klicka på **Ja**</span><span class="sxs-lookup"><span data-stu-id="7fa7c-172">You are asked tooconfirm you want toodelete hello packet capture, click **Yes**</span></span>

![Bekräftelse][4]

## <a name="stop-a-packet-capture"></a><span data-ttu-id="7fa7c-174">Stoppa en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="7fa7c-174">Stop a packet capture</span></span>

<span data-ttu-id="7fa7c-175">Samla in vyn i hello-paket, klickar du på hello **snabbmenyn** (...) eller högerklicka och klicka på **stoppa** toostop hello paketinsamling</span><span class="sxs-lookup"><span data-stu-id="7fa7c-175">In hello packet capture view, click hello **context menu** (...) or right click, and click **Stop** toostop hello packet capture</span></span>

## <a name="download-a-packet-capture"></a><span data-ttu-id="7fa7c-176">Hämta en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="7fa7c-176">Download a packet capture</span></span>

<span data-ttu-id="7fa7c-177">När paketet avbildningssessionen är klar hello avbilda filen är överförda tooblob lagring eller tooa lokala på hello VM.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-177">Once your packet capture session has completed, hello capture file is uploaded tooblob storage or tooa local file on hello VM.</span></span> <span data-ttu-id="7fa7c-178">hello lagringsplats för hello paketinsamling har definierats vid skapande av hello.</span><span class="sxs-lookup"><span data-stu-id="7fa7c-178">hello storage location of hello packet capture is defined at creation of hello session.</span></span> <span data-ttu-id="7fa7c-179">En lämplig verktyget tooaccess dessa avbilda filer sparade tooa storage-konto är Microsoft Azure Lagringsutforskaren, som kan hämtas här: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="7fa7c-179">A convenient tool tooaccess these capture files saved tooa storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="7fa7c-180">Om ett lagringskonto har angetts sparas paket avbilda filer tooa storage-konto på hello följande plats:</span><span class="sxs-lookup"><span data-stu-id="7fa7c-180">If a storage account is specified, packet capture files are saved tooa storage account at hello following location:</span></span>
```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a><span data-ttu-id="7fa7c-181">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7fa7c-181">Next steps</span></span>

<span data-ttu-id="7fa7c-182">Lär dig hur fångar tooautomate paket med virtuella aviseringar genom att visa [skapar en avisering utlösta paketinsamling](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="7fa7c-182">Learn how tooautomate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="7fa7c-183">Hitta om vissa trafik tillåts i eller utanför den virtuella datorn genom att besöka [Kontrollera Kontrollera IP-flöde](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="7fa7c-183">Find if certain traffic is allowed in or out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<!-- Image references -->
[1]: ./media/network-watcher-packet-capture-manage-portal/figure1.png
[2]: ./media/network-watcher-packet-capture-manage-portal/figure2.png
[3]: ./media/network-watcher-packet-capture-manage-portal/figure3.png
[4]: ./media/network-watcher-packet-capture-manage-portal/figure4.png
[agent]: ./media/network-watcher-packet-capture-manage-portal/agent.png













