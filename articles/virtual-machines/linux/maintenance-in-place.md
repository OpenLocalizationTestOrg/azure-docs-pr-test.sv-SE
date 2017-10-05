---
title: "VM bevarar Underhåll för virtuella Windows-datorer i Azure | Microsoft Docs"
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
ms.openlocfilehash: 09fc9021e8dfb910d1a81178434ca2e27c0bacf7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="vm-preserving-maintenance-in-place-vm-migration"></a>VM bevarar Underhåll (migrering på plats VM)

Även om majoriteten av uppdateringarna har ingen inverkan på värdbaserade virtuella datorer, finns det fall där uppdateringar av komponenter och tjänster resultera i minimala störningar att köra virtuella datorer (utan en fullständig omstart av den virtuella datorn).

Dessa uppdateringar sker med teknik som gör att live migrering på plats, även kallat ”minne bevara uppdateringen”. När du uppdaterar värden, placera den virtuella datorn är i tillståndet ”Pausad” bevaring minnet i RAM, medan värdmiljön (t.ex. det underliggande operativsystemet) gäller nödvändiga uppdateringar och korrigeringar.
Den virtuella datorn sedan återupptas inom 30 sekunder från paus.
När den virtuella datorn har återupptagits synkroniseras klockan automatiskt.

Alla uppdateringar kan inte distribueras med den här mekanismen, men med tanke på den korta pausperioden minskar distributionen av uppdateringar på det här sättet avsevärt inverkan på virtuella datorer.

Flera instanser uppdateringar (virtuella datorer i en tillgänglighetsuppsättning) är tillämpade en uppdateringsdomän i taget.

Vissa program kan påverkas av de här uppdateringarna som är mer än andra. Program som utför händelsebearbetning i realtid, direktuppspelning eller omkodning eller hög genomströmning nätverk scenarier, till exempel kan inte vara utformade klarar en paus på 30 sekund. Program som körs på en virtuell dator kan läsa mer om kommande uppdateringar genom att anropa den [schemalagda händelser](../virtual-machines-scheduled-events.md) API för den [Azure Metadata Service](../virtual-machines-instancemetadataservice-overview.md).