---
title: "aaaIntroduction tooPacket avbildning i Azure-Nätverksbevakaren | Microsoft Docs"
description: "Den här sidan innehåller en översikt över hello Nätverksbevakaren paket avbilda kapaciteten"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 3a81afaa-ecd9-4004-b68e-69ab56913356
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 2ce01b391b9c1a1c19aa29c8620628c55586df03
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toovariable-packet-capture-in-azure-network-watcher"></a><span data-ttu-id="83f19-103">Introduktion toovariable paketinsamling i Azure Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="83f19-103">Introduction toovariable packet capture in Azure Network Watcher</span></span>

<span data-ttu-id="83f19-104">Nätverket Watcher variabeln paketinsamling kan toocreate paket avbilda sessioner tootrack trafik tooand från en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="83f19-104">Network Watcher variable packet capture allows you toocreate packet capture sessions tootrack traffic tooand from a virtual machine.</span></span> <span data-ttu-id="83f19-105">Paketinsamling hjälper toodiagnose nätverk avvikelser båda reaktivt och proactivity.</span><span class="sxs-lookup"><span data-stu-id="83f19-105">Packet capture helps toodiagnose network anomalies both reactively and proactivity.</span></span> <span data-ttu-id="83f19-106">Andra användningsområden omfattar att samla in nätverksstatistik får information om nätverket intrång, toodebug klient / server-kommunikation och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="83f19-106">Other uses include gathering network statistics, gaining information on network intrusions, toodebug client-server communications and much more.</span></span>

<span data-ttu-id="83f19-107">Paketinsamling är ett tillägg för virtuell dator som startas från en fjärrdator via Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="83f19-107">Packet capture is a virtual machine extension that is remotely started through Network Watcher.</span></span> <span data-ttu-id="83f19-108">Den här funktionen underlättar hello belastningen för körs en paketinsamling manuellt på hello önskade virtuella datorn, vilket sparar värdefull tid.</span><span class="sxs-lookup"><span data-stu-id="83f19-108">This capability eases hello burden of running a packet capture manually on hello desired virtual machine, which saves valuable time.</span></span> <span data-ttu-id="83f19-109">Paketinsamling kan aktiveras via hello portal, PowerShell, CLI eller REST API.</span><span class="sxs-lookup"><span data-stu-id="83f19-109">Packet capture can be triggered through hello portal, PowerShell, CLI, or REST API.</span></span> <span data-ttu-id="83f19-110">Ett exempel på hur paketinsamling kan utlösas är med virtuella aviseringar.</span><span class="sxs-lookup"><span data-stu-id="83f19-110">One example of how packet capture can be triggered is with Virtual Machine alerts.</span></span> <span data-ttu-id="83f19-111">Filter som är angivna för hello avbilda session tooensure du fånga in trafik som du vill toomonitor.</span><span class="sxs-lookup"><span data-stu-id="83f19-111">Filters are provided for hello capture session tooensure you capture traffic you want toomonitor.</span></span> <span data-ttu-id="83f19-112">Filter baseras på 5-tuppel (protokoll, lokal IP-adress, fjärranslutna IP-adress, lokal port och Fjärrport) information.</span><span class="sxs-lookup"><span data-stu-id="83f19-112">Filters are based on 5-tuple (protocol, local IP address, remote IP address, local port, and remote port) information.</span></span> <span data-ttu-id="83f19-113">hello infångade data lagras i hello lokal disk eller en lagringsblob.</span><span class="sxs-lookup"><span data-stu-id="83f19-113">hello captured data is stored in hello local disk or a storage blob.</span></span> <span data-ttu-id="83f19-114">Det finns en begränsning på 10 paket avbilda sessioner per region per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="83f19-114">There is a limit of 10 packet capture sessions per region per subscription.</span></span> <span data-ttu-id="83f19-115">Den här gränsen gäller endast toohello sessioner och gäller inte toohello spara paketet avbilda filer lokalt på hello VM eller i ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="83f19-115">This limit applies only toohello sessions and does not apply toohello saved packet capture files either locally on hello VM or in a storage account.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="83f19-116">Paketinsamling kräver ett tillägg för virtuell dator `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="83f19-116">Packet capture requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="83f19-117">Installera hello tillägg på en Windows VM finns [tillägg för virtuell dator i Azure Network Watcher Agent för Windows](../virtual-machines/windows/extensions-nwa.md) och för Linux VM besöka [tillägg för virtuell dator i Azure Network Watcher Agent för Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="83f19-117">For installing hello extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

<span data-ttu-id="83f19-118">tooreduce hello information du avbildar tooonly hello information som du vill, hello följande alternativ är tillgängliga för en paket-avbildningssessionen:</span><span class="sxs-lookup"><span data-stu-id="83f19-118">tooreduce hello information you capture tooonly hello information you want, hello following options are available for a packet capture session:</span></span>

<span data-ttu-id="83f19-119">**Samla in konfiguration**</span><span class="sxs-lookup"><span data-stu-id="83f19-119">**Capture configuration**</span></span>

|<span data-ttu-id="83f19-120">Egenskap</span><span class="sxs-lookup"><span data-stu-id="83f19-120">Property</span></span>|<span data-ttu-id="83f19-121">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="83f19-121">Description</span></span>|
|---|---|
|<span data-ttu-id="83f19-122">**Maximalt antal byte per paket (byte)**</span><span class="sxs-lookup"><span data-stu-id="83f19-122">**Maximum bytes per packet (bytes)**</span></span> | <span data-ttu-id="83f19-123">Hej antalet byte från varje paket som har hämtats avbildas alla byte om värdet är tomt.</span><span class="sxs-lookup"><span data-stu-id="83f19-123">hello number of bytes from each packet that are captured, all bytes are captured if left blank.</span></span> <span data-ttu-id="83f19-124">Hej antalet byte från varje paket som har hämtats avbildas alla byte om värdet är tomt.</span><span class="sxs-lookup"><span data-stu-id="83f19-124">hello number of bytes from each packet that are captured, all bytes are captured if left blank.</span></span> <span data-ttu-id="83f19-125">Om du behöver endast hello IPv4-huvud – ange 34 här</span><span class="sxs-lookup"><span data-stu-id="83f19-125">If you need only hello IPv4 header – indicate 34 here</span></span> |
|<span data-ttu-id="83f19-126">**Maximalt antal byte per session (byte)**</span><span class="sxs-lookup"><span data-stu-id="83f19-126">**Maximum bytes per session (bytes)**</span></span> | <span data-ttu-id="83f19-127">Sammanlagt antal byte som fångas när hello-värdet har nåtts hello konsolsessionen avslutas.</span><span class="sxs-lookup"><span data-stu-id="83f19-127">Total number of bytes in that are captured, once hello value is reached hello session ends.</span></span>|
|<span data-ttu-id="83f19-128">**Tidsgräns (sekunder)**</span><span class="sxs-lookup"><span data-stu-id="83f19-128">**Time limit (seconds)**</span></span> | <span data-ttu-id="83f19-129">Anger tidsbegränsning för hello paket avbilda session.</span><span class="sxs-lookup"><span data-stu-id="83f19-129">Sets a time constraint on hello packet capture session.</span></span> <span data-ttu-id="83f19-130">hello standardvärdet är 18000 sekunder eller 5 timmar.</span><span class="sxs-lookup"><span data-stu-id="83f19-130">hello default value is 18000 seconds or 5 hours.</span></span>|

<span data-ttu-id="83f19-131">**Filtrering (valfritt)**</span><span class="sxs-lookup"><span data-stu-id="83f19-131">**Filtering (optional)**</span></span>

|<span data-ttu-id="83f19-132">Egenskap</span><span class="sxs-lookup"><span data-stu-id="83f19-132">Property</span></span>|<span data-ttu-id="83f19-133">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="83f19-133">Description</span></span>|
|---|---|
|<span data-ttu-id="83f19-134">**Protokoll**</span><span class="sxs-lookup"><span data-stu-id="83f19-134">**Protocol**</span></span> | <span data-ttu-id="83f19-135">Avbilda hello protokollet toofilter för hello-paket.</span><span class="sxs-lookup"><span data-stu-id="83f19-135">hello protocol toofilter for hello packet capture.</span></span> <span data-ttu-id="83f19-136">hello tillgängliga värden är TCP, UDP och alla.</span><span class="sxs-lookup"><span data-stu-id="83f19-136">hello available values are TCP, UDP, and All.</span></span>|
|<span data-ttu-id="83f19-137">**Lokal IP-adress**</span><span class="sxs-lookup"><span data-stu-id="83f19-137">**Local IP address**</span></span> | <span data-ttu-id="83f19-138">Det här värdet filtrerar hello paket avbilda toopackets där hello lokala IP-adressen matchar den här filtervärdet.</span><span class="sxs-lookup"><span data-stu-id="83f19-138">This value filters hello packet capture toopackets where hello local IP address matches this filter value.</span></span>|
|<span data-ttu-id="83f19-139">**Lokal port**</span><span class="sxs-lookup"><span data-stu-id="83f19-139">**Local port**</span></span> | <span data-ttu-id="83f19-140">Det här värdet filter hello paketet avbilda toopackets där hello lokal port matchar filtret värdet.</span><span class="sxs-lookup"><span data-stu-id="83f19-140">This value filters hello packet capture toopackets where hello local port matches this filter value.</span></span>|
|<span data-ttu-id="83f19-141">**Fjärranslutna IP-adress**</span><span class="sxs-lookup"><span data-stu-id="83f19-141">**Remote IP address**</span></span> | <span data-ttu-id="83f19-142">Det här värdet filter hello paketet avbilda toopackets där hello fjärr-IP-matchar filtret värdet.</span><span class="sxs-lookup"><span data-stu-id="83f19-142">This value filters hello packet capture toopackets where hello remote IP matches this filter value.</span></span>|
|<span data-ttu-id="83f19-143">**Fjärrport**</span><span class="sxs-lookup"><span data-stu-id="83f19-143">**Remote port**</span></span> | <span data-ttu-id="83f19-144">Det här värdet filter hello paketet avbilda toopackets där hello Fjärrport matchar filtret värdet.</span><span class="sxs-lookup"><span data-stu-id="83f19-144">This value filters hello packet capture toopackets where hello remote port matches this filter value.</span></span>|

### <a name="next-steps"></a><span data-ttu-id="83f19-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="83f19-145">Next steps</span></span>

<span data-ttu-id="83f19-146">Lär dig hur du kan hantera paket insamlingar hello-portalen genom att besöka [hantera paketinsamling i hello Azure-portalen](network-watcher-packet-capture-manage-portal.md) eller med PowerShell genom att besöka [hantera paket avbilda med PowerShell](network-watcher-packet-capture-manage-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="83f19-146">Learn how you can manage packet captures through hello portal by visiting [Manage packet capture in hello Azure portal](network-watcher-packet-capture-manage-portal.md) or with PowerShell by visiting [Manage Packet Capture with PowerShell](network-watcher-packet-capture-manage-powershell.md).</span></span>

<span data-ttu-id="83f19-147">Läs hur toocreate proaktiv paket samlar in baserat på virtuella aviseringar genom att besöka [skapar en avisering utlösta paketinsamling](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="83f19-147">Learn how toocreate proactive packet captures based on virtual machine alerts by visiting [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<!--Image references-->
[1]: ./media/network-watcher-packet-capture-overview/figure1.png













