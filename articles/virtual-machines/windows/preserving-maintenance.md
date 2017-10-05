---
title: "VM bevarar Underhåll för virtuella Windows-datorer i Azure | Microsoft Docs"
description: "Direktmigrering VM för minne bevarar uppdateringar."
services: virtual-machines-windows
documentationcenter: 
author: zivr
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/27/2017
ms.author: zivr
ms.openlocfilehash: 8888bafbc3aba24168312b611a9b4fbde25f376d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="vm-preserving-maintenance-with-in-place-vm-migration"></a><span data-ttu-id="07221-103">VM bevarar Underhåll med lokalt VM-migrering</span><span class="sxs-lookup"><span data-stu-id="07221-103">VM preserving maintenance with In-place VM migration</span></span>

<span data-ttu-id="07221-104">Även om majoriteten av uppdateringarna har ingen inverkan på värdbaserade virtuella datorer, finns det fall där uppdateringar av komponenter och tjänster resultera i minimala störningar att köra virtuella datorer (utan en fullständig omstart av den virtuella datorn).</span><span class="sxs-lookup"><span data-stu-id="07221-104">While the majority of updates have no impact to hosted VMs, there are cases where updates to components or services result in minimal interference to running VMs (without a full reboot of the virtual machine).</span></span>

<span data-ttu-id="07221-105">Dessa uppdateringar sker med teknik som gör att live migrering på plats, även kallat ”minne bevara uppdateringen”.</span><span class="sxs-lookup"><span data-stu-id="07221-105">These updates are accomplished with technology that enables in-place live migration, also called "memory-preserving update".</span></span> <span data-ttu-id="07221-106">När du uppdaterar värden, placera den virtuella datorn är i tillståndet ”Pausad” bevaring minnet i RAM, medan värdmiljön (t.ex. det underliggande operativsystemet) gäller nödvändiga uppdateringar och korrigeringar.</span><span class="sxs-lookup"><span data-stu-id="07221-106">When updating the host, the virtual machine is placed into a “paused” state, preserving the memory in RAM, while the hosting environment (e.g. the underlying operating system) applies the necessary updates and patches.</span></span>
<span data-ttu-id="07221-107">Den virtuella datorn sedan återupptas inom 30 sekunder från paus.</span><span class="sxs-lookup"><span data-stu-id="07221-107">The virtual machine is then resumed within 30 seconds of being paused.</span></span>
<span data-ttu-id="07221-108">När den virtuella datorn har återupptagits synkroniseras klockan automatiskt.</span><span class="sxs-lookup"><span data-stu-id="07221-108">After resuming, the clock of the virtual machine is automatically synchronized.</span></span>

<span data-ttu-id="07221-109">Alla uppdateringar kan inte distribueras med den här mekanismen, men med tanke på den korta pausperioden minskar distributionen av uppdateringar på det här sättet avsevärt inverkan på virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="07221-109">Not all updates can be deployed by using this mechanism, but given the short pause period, deploying updates in this way greatly reduces impact to virtual machines.</span></span>

<span data-ttu-id="07221-110">Flera instanser uppdateringar (virtuella datorer i en tillgänglighetsuppsättning) är tillämpade en uppdateringsdomän i taget.</span><span class="sxs-lookup"><span data-stu-id="07221-110">Multi-instance updates (VMs in an availability set) are applied one update domain at a time.</span></span>

<span data-ttu-id="07221-111">Vissa program kan påverkas av de här uppdateringarna som är mer än andra.</span><span class="sxs-lookup"><span data-stu-id="07221-111">Some applications may be impacted by these updates more than others.</span></span> <span data-ttu-id="07221-112">Program som utför händelsebearbetning i realtid, direktuppspelning eller omkodning eller hög genomströmning nätverk scenarier, till exempel kan inte vara utformade klarar en paus på 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="07221-112">Applications that perform real-time event processing, media streaming or transcoding, or high throughput networking scenarios, for example, may not be designed to tolerate a 30 second pause.</span></span> <span data-ttu-id="07221-113">Program som körs på en virtuell dator kan läsa mer om kommande uppdateringar genom att anropa den [schemalagda händelser](../virtual-machines-scheduled-events.md) API för den [Azure Metadata Service](../virtual-machines-instancemetadataservice-overview.md).</span><span class="sxs-lookup"><span data-stu-id="07221-113">Applications running in a virtual machine can learn about upcoming updates by calling the [Scheduled Events](../virtual-machines-scheduled-events.md) API of the [Azure Metadata Service](../virtual-machines-instancemetadataservice-overview.md).</span></span>
