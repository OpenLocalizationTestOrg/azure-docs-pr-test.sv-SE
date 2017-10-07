---
title: "aaaVM bevarar Underhåll för virtuella Windows-datorer i Azure | Microsoft Docs"
description: "Direktmigrering VM för minne bevarar uppdateringar."
services: virtual-machines-windows
documentationcenter: 
author: 
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
ms.author: 
ms.openlocfilehash: 544a2dcca52bb3ac51d341bceaf4ba3e7c71fd82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="vm-preserving-maintenance-in-place-vm-migration"></a><span data-ttu-id="bf8ca-103">VM bevarar Underhåll (migrering på plats VM)</span><span class="sxs-lookup"><span data-stu-id="bf8ca-103">VM preserving maintenance (In-place VM migration)</span></span>

<span data-ttu-id="bf8ca-104">Hello majoriteten av uppdateringarna har ingen effekt toohosted virtuella datorer, finns det fall där uppdateringar toocomponents eller tjänster resultera i minimala störningar toorunning virtuella datorer (utan en fullständig omstart av hello virtuell dator).</span><span class="sxs-lookup"><span data-stu-id="bf8ca-104">While hello majority of updates have no impact toohosted VMs, there are cases where updates toocomponents or services result in minimal interference toorunning VMs (without a full reboot of hello virtual machine).</span></span>

<span data-ttu-id="bf8ca-105">Dessa uppdateringar sker med teknik som gör att live migrering på plats, även kallat ”minne bevara uppdateringen”.</span><span class="sxs-lookup"><span data-stu-id="bf8ca-105">These updates are accomplished with technology that enables in-place live migration, also called "memory-preserving update".</span></span> <span data-ttu-id="bf8ca-106">När du uppdaterar hello värden, hello virtuella datorn placeras i tillståndet ”Pausad” bevaring hello minne i RAM, medan hello värdmiljön (t.ex. det underliggande operativsystemet) gäller hello nödvändiga uppdateringar och korrigeringar.</span><span class="sxs-lookup"><span data-stu-id="bf8ca-106">When updating hello host, hello virtual machine is placed into a “paused” state, preserving hello memory in RAM, while hello hosting environment (e.g. the underlying operating system) applies hello necessary updates and patches.</span></span>
<span data-ttu-id="bf8ca-107">hello virtuella sedan återupptas inom 30 sekunder från paus.</span><span class="sxs-lookup"><span data-stu-id="bf8ca-107">hello virtual machine is then resumed within 30 seconds of being paused.</span></span>
<span data-ttu-id="bf8ca-108">Efter återställning, synkroniseras automatiskt hello klocka hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="bf8ca-108">After resuming, hello clock of hello virtual machine is automatically synchronized.</span></span>

<span data-ttu-id="bf8ca-109">Inte alla uppdateringar som kan distribueras med hjälp av den här mekanismen, men den angivna perioden kort paus distribuera uppdateringar i den här vägen avsevärt minskar inverkan toovirtual datorer.</span><span class="sxs-lookup"><span data-stu-id="bf8ca-109">Not all updates can be deployed by using this mechanism, but given the short pause period, deploying updates in this way greatly reduces impact toovirtual machines.</span></span>

<span data-ttu-id="bf8ca-110">Flera instanser uppdateringar (virtuella datorer i en tillgänglighetsuppsättning) är tillämpade en uppdateringsdomän i taget.</span><span class="sxs-lookup"><span data-stu-id="bf8ca-110">Multi-instance updates (VMs in an availability set) are applied one update domain at a time.</span></span>

<span data-ttu-id="bf8ca-111">Vissa program kan påverkas av de här uppdateringarna som är mer än andra.</span><span class="sxs-lookup"><span data-stu-id="bf8ca-111">Some applications may be impacted by these updates more than others.</span></span> <span data-ttu-id="bf8ca-112">Program som utför händelsebearbetning i realtid, direktuppspelning eller omkodning eller hög genomströmning nätverk scenarier, till exempel inte utformad tootolerate en paus på 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="bf8ca-112">Applications that perform real-time event processing, media streaming or transcoding, or high throughput networking scenarios, for example, may not be designed tootolerate a 30 second pause.</span></span> <span data-ttu-id="bf8ca-113">Program som körs på en virtuell dator kan läsa mer om kommande uppdateringar genom att anropa hello [schemalagda händelser](../virtual-machines-scheduled-events.md) API hello [Azure Metadata Service](../virtual-machines-instancemetadataservice-overview.md).</span><span class="sxs-lookup"><span data-stu-id="bf8ca-113">Applications running in a virtual machine can learn about upcoming updates by calling hello [Scheduled Events](../virtual-machines-scheduled-events.md) API of hello [Azure Metadata Service](../virtual-machines-instancemetadataservice-overview.md).</span></span>
