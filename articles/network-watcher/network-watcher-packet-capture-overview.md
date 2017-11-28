---
title: "Introduktion till paketinsamling i Azure-Nätverksbevakaren | Microsoft Docs"
description: "Den här sidan innehåller en översikt över Nätverksbevakaren paket avbilda kapaciteten"
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
ms.openlocfilehash: 4fdd007c2cfad7b42f26ab2cacfba06d95c8dad3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="introduction-to-variable-packet-capture-in-azure-network-watcher"></a><span data-ttu-id="f398a-103">Introduktion till variabeln paketinsamling i Azure Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="f398a-103">Introduction to variable packet capture in Azure Network Watcher</span></span>

<span data-ttu-id="f398a-104">Nätverket Watcher variabeln paketinsamling kan du skapa paket capture-sessioner för att spåra trafik till och från en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="f398a-104">Network Watcher variable packet capture allows you to create packet capture sessions to track traffic to and from a virtual machine.</span></span> <span data-ttu-id="f398a-105">Paketet avbilda bidrar till att diagnostisera nätverk avvikelser båda reaktivt och proactivity.</span><span class="sxs-lookup"><span data-stu-id="f398a-105">Packet capture helps to diagnose network anomalies both reactively and proactivity.</span></span> <span data-ttu-id="f398a-106">Andra användningsområden omfattar att samla in nätverksstatistik får information om nätverket intrång felsöka klient-/ serverkommunikation och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="f398a-106">Other uses include gathering network statistics, gaining information on network intrusions, to debug client-server communications and much more.</span></span>

<span data-ttu-id="f398a-107">Paketinsamling är ett tillägg för virtuell dator som startas från en fjärrdator via Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="f398a-107">Packet capture is a virtual machine extension that is remotely started through Network Watcher.</span></span> <span data-ttu-id="f398a-108">Den här funktionen underlättar för att köra en paketinsamling manuellt på den önskade virtuella datorn, vilket sparar värdefull tid.</span><span class="sxs-lookup"><span data-stu-id="f398a-108">This capability eases the burden of running a packet capture manually on the desired virtual machine, which saves valuable time.</span></span> <span data-ttu-id="f398a-109">Paketinsamling kan aktiveras via portalen, PowerShell, CLI eller REST API.</span><span class="sxs-lookup"><span data-stu-id="f398a-109">Packet capture can be triggered through the portal, PowerShell, CLI, or REST API.</span></span> <span data-ttu-id="f398a-110">Ett exempel på hur paketinsamling kan utlösas är med virtuella aviseringar.</span><span class="sxs-lookup"><span data-stu-id="f398a-110">One example of how packet capture can be triggered is with Virtual Machine alerts.</span></span> <span data-ttu-id="f398a-111">Filter har angetts för hämtningens så du fånga in trafik som du vill övervaka.</span><span class="sxs-lookup"><span data-stu-id="f398a-111">Filters are provided for the capture session to ensure you capture traffic you want to monitor.</span></span> <span data-ttu-id="f398a-112">Filter baseras på 5-tuppel (protokoll, lokal IP-adress, fjärranslutna IP-adress, lokal port och Fjärrport) information.</span><span class="sxs-lookup"><span data-stu-id="f398a-112">Filters are based on 5-tuple (protocol, local IP address, remote IP address, local port, and remote port) information.</span></span> <span data-ttu-id="f398a-113">Fångade data lagras i den lokala disken eller en lagringsblob.</span><span class="sxs-lookup"><span data-stu-id="f398a-113">The captured data is stored in the local disk or a storage blob.</span></span> <span data-ttu-id="f398a-114">Det finns en begränsning på 10 paket avbilda sessioner per region per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f398a-114">There is a limit of 10 packet capture sessions per region per subscription.</span></span> <span data-ttu-id="f398a-115">Den här gränsen gäller bara för sessioner och gäller inte för sparade paket avbilda filer lokalt på den virtuella datorn eller i ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="f398a-115">This limit applies only to the sessions and does not apply to the saved packet capture files either locally on the VM or in a storage account.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f398a-116">Paketinsamling kräver ett tillägg för virtuell dator `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="f398a-116">Packet capture requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="f398a-117">Installera tillägget på en Windows VM finns [tillägg för virtuell dator i Azure Network Watcher Agent för Windows](../virtual-machines/windows/extensions-nwa.md) och för Linux VM besöka [tillägg för virtuell dator i Azure Network Watcher Agent för Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="f398a-117">For installing the extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

<span data-ttu-id="f398a-118">För att minska den information du fånga in till bara den information som du vill, finns följande alternativ för en paket-avbildningssessionen:</span><span class="sxs-lookup"><span data-stu-id="f398a-118">To reduce the information you capture to only the information you want, the following options are available for a packet capture session:</span></span>

<span data-ttu-id="f398a-119">**Samla in konfiguration**</span><span class="sxs-lookup"><span data-stu-id="f398a-119">**Capture configuration**</span></span>

|<span data-ttu-id="f398a-120">Egenskap</span><span class="sxs-lookup"><span data-stu-id="f398a-120">Property</span></span>|<span data-ttu-id="f398a-121">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f398a-121">Description</span></span>|
|---|---|
|<span data-ttu-id="f398a-122">**Maximalt antal byte per paket (byte)**</span><span class="sxs-lookup"><span data-stu-id="f398a-122">**Maximum bytes per packet (bytes)**</span></span> | <span data-ttu-id="f398a-123">Antalet byte från varje paket som har hämtats avbildas alla byte om värdet är tomt.</span><span class="sxs-lookup"><span data-stu-id="f398a-123">The number of bytes from each packet that are captured, all bytes are captured if left blank.</span></span> <span data-ttu-id="f398a-124">Antalet byte från varje paket som har hämtats avbildas alla byte om värdet är tomt.</span><span class="sxs-lookup"><span data-stu-id="f398a-124">The number of bytes from each packet that are captured, all bytes are captured if left blank.</span></span> <span data-ttu-id="f398a-125">Om du behöver endast IPv4-huvud – ange 34 här</span><span class="sxs-lookup"><span data-stu-id="f398a-125">If you need only the IPv4 header – indicate 34 here</span></span> |
|<span data-ttu-id="f398a-126">**Maximalt antal byte per session (byte)**</span><span class="sxs-lookup"><span data-stu-id="f398a-126">**Maximum bytes per session (bytes)**</span></span> | <span data-ttu-id="f398a-127">Sammanlagt antal byte som fångas när värdet är nådd sessionen avslutas.</span><span class="sxs-lookup"><span data-stu-id="f398a-127">Total number of bytes in that are captured, once the value is reached the session ends.</span></span>|
|<span data-ttu-id="f398a-128">**Tidsgräns (sekunder)**</span><span class="sxs-lookup"><span data-stu-id="f398a-128">**Time limit (seconds)**</span></span> | <span data-ttu-id="f398a-129">Anger en tidsbegränsning på paketet avbilda session.</span><span class="sxs-lookup"><span data-stu-id="f398a-129">Sets a time constraint on the packet capture session.</span></span> <span data-ttu-id="f398a-130">Standardvärdet är 18000 sekunder eller 5 timmar.</span><span class="sxs-lookup"><span data-stu-id="f398a-130">The default value is 18000 seconds or 5 hours.</span></span>|

<span data-ttu-id="f398a-131">**Filtrering (valfritt)**</span><span class="sxs-lookup"><span data-stu-id="f398a-131">**Filtering (optional)**</span></span>

|<span data-ttu-id="f398a-132">Egenskap</span><span class="sxs-lookup"><span data-stu-id="f398a-132">Property</span></span>|<span data-ttu-id="f398a-133">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f398a-133">Description</span></span>|
|---|---|
|<span data-ttu-id="f398a-134">**Protokoll**</span><span class="sxs-lookup"><span data-stu-id="f398a-134">**Protocol**</span></span> | <span data-ttu-id="f398a-135">Protokoll att filtrera för paket-avbildning.</span><span class="sxs-lookup"><span data-stu-id="f398a-135">The protocol to filter for the packet capture.</span></span> <span data-ttu-id="f398a-136">Tillgängliga värden är TCP, UDP och alla.</span><span class="sxs-lookup"><span data-stu-id="f398a-136">The available values are TCP, UDP, and All.</span></span>|
|<span data-ttu-id="f398a-137">**Lokal IP-adress**</span><span class="sxs-lookup"><span data-stu-id="f398a-137">**Local IP address**</span></span> | <span data-ttu-id="f398a-138">Det här värdet filtrerar paketinsamling att paket där den lokala IP-adressen matchar den här filtervärdet.</span><span class="sxs-lookup"><span data-stu-id="f398a-138">This value filters the packet capture to packets where the local IP address matches this filter value.</span></span>|
|<span data-ttu-id="f398a-139">**Lokal port**</span><span class="sxs-lookup"><span data-stu-id="f398a-139">**Local port**</span></span> | <span data-ttu-id="f398a-140">Det här värdet filtrerar paketinsamling att paket där den lokala porten matchar filtret värdet.</span><span class="sxs-lookup"><span data-stu-id="f398a-140">This value filters the packet capture to packets where the local port matches this filter value.</span></span>|
|<span data-ttu-id="f398a-141">**Fjärranslutna IP-adress**</span><span class="sxs-lookup"><span data-stu-id="f398a-141">**Remote IP address**</span></span> | <span data-ttu-id="f398a-142">Det här värdet filtrerar paketinsamling att paket där fjärranslutna IP-Adressen matchar den här filtervärdet.</span><span class="sxs-lookup"><span data-stu-id="f398a-142">This value filters the packet capture to packets where the remote IP matches this filter value.</span></span>|
|<span data-ttu-id="f398a-143">**Fjärrport**</span><span class="sxs-lookup"><span data-stu-id="f398a-143">**Remote port**</span></span> | <span data-ttu-id="f398a-144">Det här värdet filtrerar paketinsamling att paket där fjärrporten matchar filtret värdet.</span><span class="sxs-lookup"><span data-stu-id="f398a-144">This value filters the packet capture to packets where the remote port matches this filter value.</span></span>|

### <a name="next-steps"></a><span data-ttu-id="f398a-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f398a-145">Next steps</span></span>

<span data-ttu-id="f398a-146">Lär dig hur du kan hantera paket insamlingar via portalen genom att besöka [hantera paketinsamling i Azure portal](network-watcher-packet-capture-manage-portal.md) eller med PowerShell genom att besöka [hantera paket avbilda med PowerShell](network-watcher-packet-capture-manage-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="f398a-146">Learn how you can manage packet captures through the portal by visiting [Manage packet capture in the Azure portal](network-watcher-packet-capture-manage-portal.md) or with PowerShell by visiting [Manage Packet Capture with PowerShell](network-watcher-packet-capture-manage-powershell.md).</span></span>

<span data-ttu-id="f398a-147">Lär dig hur du skapar proaktiv paket insamlingar baserat på virtuella aviseringar genom att besöka [skapar en avisering utlösta paketinsamling](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="f398a-147">Learn how to create proactive packet captures based on virtual machine alerts by visiting [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<!--Image references-->
[1]: ./media/network-watcher-packet-capture-overview/figure1.png













