---
title: "aaaCheck anslutningen till Azure Nätverksbevakaren - Azure-portalen | Microsoft Docs"
description: "Den här sidan förklarar hur toouse anslutningen kontrolleras med Nätverksbevakaren med hello Azure-portalen"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/03/2017
ms.author: gwallace
ms.openlocfilehash: ef6ecccd688f06f70003a5b59771c15bcbe8f3e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="check-connectivity-with-azure-network-watcher-using-hello-azure-portal"></a><span data-ttu-id="80d43-103">Kontrollera anslutningen med Azure Nätverksbevakaren med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="80d43-103">Check connectivity with Azure Network Watcher using hello Azure portal</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="80d43-104">Portal</span><span class="sxs-lookup"><span data-stu-id="80d43-104">Portal</span></span>](network-watcher-connectivity-portal.md)
> - [<span data-ttu-id="80d43-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="80d43-105">PowerShell</span></span>](network-watcher-connectivity-powershell.md)
> - [<span data-ttu-id="80d43-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="80d43-106">CLI 2.0</span></span>](network-watcher-connectivity-cli.md)
> - [<span data-ttu-id="80d43-107">Azure REST-API</span><span class="sxs-lookup"><span data-stu-id="80d43-107">Azure REST API</span></span>](network-watcher-connectivity-rest.md)

<span data-ttu-id="80d43-108">Lär dig hur toouse anslutning tooverify om en direkt TCP-anslutning från en virtuell dator tooa angivna slutpunkten kan upprättas.</span><span class="sxs-lookup"><span data-stu-id="80d43-108">Learn how toouse connectivity tooverify if a direct TCP connection from a virtual machine tooa given endpoint can be established.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="80d43-109">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="80d43-109">Before you begin</span></span>

<span data-ttu-id="80d43-110">Den här artikeln förutsätter att du har hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="80d43-110">This article assumes you have hello following resources:</span></span>

* <span data-ttu-id="80d43-111">En instans Nätverksbevakaren i hello region som du vill toocheck anslutning.</span><span class="sxs-lookup"><span data-stu-id="80d43-111">An instance of Network Watcher in hello region you want toocheck connectivity.</span></span>

* <span data-ttu-id="80d43-112">Virtuella datorer toocheck anslutningsmöjligheter med.</span><span class="sxs-lookup"><span data-stu-id="80d43-112">Virtual machines toocheck connectivity with.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="80d43-113">Kontrollera anslutningen kräver ett tillägg för virtuell dator `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="80d43-113">Connectivity check requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="80d43-114">Installera hello tillägg på en Windows VM finns [tillägg för virtuell dator i Azure Network Watcher Agent för Windows](../virtual-machines/windows/extensions-nwa.md) och för Linux VM besöka [tillägg för virtuell dator i Azure Network Watcher Agent för Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="80d43-114">For installing hello extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

## <a name="check-connectivity-tooa-virtual-machine"></a><span data-ttu-id="80d43-115">Kontrollera anslutningen tooa virtuell dator</span><span class="sxs-lookup"><span data-stu-id="80d43-115">Check connectivity tooa virtual machine</span></span>

<span data-ttu-id="80d43-116">Det här exemplet kontrollerar anslutningen tooa virtuella måldatorn via port 80.</span><span class="sxs-lookup"><span data-stu-id="80d43-116">This example checks connectivity tooa destination virtual machine over port 80.</span></span>

<span data-ttu-id="80d43-117">Navigera tooyour Nätverksbevakaren och klicka på **anslutningen Kontrollera (förhandsgranskning)**.</span><span class="sxs-lookup"><span data-stu-id="80d43-117">Navigate tooyour Network Watcher and click **Connectivity check (Preview)**.</span></span> <span data-ttu-id="80d43-118">Välj hello virtuella toocheck anslutningen från.</span><span class="sxs-lookup"><span data-stu-id="80d43-118">Select hello virtual machine toocheck connectivity from.</span></span> <span data-ttu-id="80d43-119">I hello **mål** väljer **väljer en virtuell dator** och välj hello rätt virtuell dator och port tootest.</span><span class="sxs-lookup"><span data-stu-id="80d43-119">In hello **Destination** section choose **Select a virtual machine** and choose hello correct virtual machine and port tootest.</span></span>

<span data-ttu-id="80d43-120">När du klickar på **Kontrollera**, hello anslutningar mellan hello virtuella datorer på angivna hello-porten är markerade.</span><span class="sxs-lookup"><span data-stu-id="80d43-120">Once you click **Check**, hello connectivity between hello virtual machines on hello port specified are checked.</span></span> <span data-ttu-id="80d43-121">I exemplet hello hello mål VM kan inte nås, visas en lista över hopp.</span><span class="sxs-lookup"><span data-stu-id="80d43-121">In hello example, hello destination VM is unreachable, a listing of hops are shown.</span></span>

![Resultat av anslutningen för en virtuell dator][1]

## <a name="check-remote-endpoint-connectivity"></a><span data-ttu-id="80d43-123">Kontrollera att fjärrslutpunkten anslutning</span><span class="sxs-lookup"><span data-stu-id="80d43-123">Check remote endpoint connectivity</span></span>

<span data-ttu-id="80d43-124">toocheck hello anslutning och svarstid tooa fjärrslutpunkten, Välj hello **ange manuellt** alternativknapp i hello **mål** avsnittet, ange hello url och hello porten och klicka på **Kontrollera** .</span><span class="sxs-lookup"><span data-stu-id="80d43-124">toocheck hello connectivity and latency tooa remote endpoint, choose hello **Specify manually** radio button in hello **Destination** section, input hello url and hello port and click **Check**.</span></span>  <span data-ttu-id="80d43-125">Det här används för fjärråtkomst-slutpunkter som slutpunkter för webbplatser och lagring.</span><span class="sxs-lookup"><span data-stu-id="80d43-125">This is used for remote endpoints like websites and storage endpoints.</span></span>

![Resultat av anslutningen för en webbplats][2]

## <a name="next-steps"></a><span data-ttu-id="80d43-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="80d43-127">Next steps</span></span>

<span data-ttu-id="80d43-128">Lär dig hur fångar tooautomate paket med virtuella aviseringar genom att visa [skapar en avisering utlösta paketinsamling](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="80d43-128">Learn how tooautomate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="80d43-129">Hitta om vissa trafik tillåts i eller utanför den virtuella datorn genom att besöka [Kontrollera Kontrollera IP-flöde](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="80d43-129">Find if certain traffic is allowed in or out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

[1]: ./media/network-watcher-connectivity-portal/figure1.png
[2]: ./media/network-watcher-connectivity-portal/figure2.png
