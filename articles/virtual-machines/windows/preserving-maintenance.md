---
title: "aaa VM bevara Underhåll för virtuella Windows-datorer i Azure | Microsoft Docs"
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
ms.openlocfilehash: b798f0afd9d8dc60ca8a78f7cc77435a0ddc76fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="vm-preserving-maintenance-with-in-place-vm-migration"></a>VM bevarar Underhåll med lokalt VM-migrering

Hello majoriteten av uppdateringarna har ingen effekt toohosted virtuella datorer, finns det fall där uppdateringar toocomponents eller tjänster resultera i minimala störningar toorunning virtuella datorer (utan en fullständig omstart av hello virtuell dator).

Dessa uppdateringar sker med teknik som gör att live migrering på plats, även kallat ”minne bevara uppdateringen”. När du uppdaterar hello värden, hello virtuella datorn placeras i tillståndet ”Pausad” bevaring hello minne i RAM, medan hello värdmiljön (t.ex. det underliggande operativsystemet) gäller hello nödvändiga uppdateringar och korrigeringar.
hello virtuella sedan återupptas inom 30 sekunder från paus.
Efter återställning, synkroniseras automatiskt hello klocka hello virtuell dator.

Inte alla uppdateringar som kan distribueras med hjälp av den här mekanismen, men den angivna perioden kort paus distribuera uppdateringar i den här vägen avsevärt minskar inverkan toovirtual datorer.

Flera instanser uppdateringar (virtuella datorer i en tillgänglighetsuppsättning) är tillämpade en uppdateringsdomän i taget.

Vissa program kan påverkas av de här uppdateringarna som är mer än andra. Program som utför händelsebearbetning i realtid, direktuppspelning eller omkodning eller hög genomströmning nätverk scenarier, till exempel inte utformad tootolerate en paus på 30 sekund. Program som körs på en virtuell dator kan läsa mer om kommande uppdateringar genom att anropa hello [schemalagda händelser](../virtual-machines-scheduled-events.md) API hello [Azure Metadata Service](../virtual-machines-instancemetadataservice-overview.md).
