---
title: Skapa flera virtuella datorer | Microsoft Docs
description: "Alternativ för att skapa flera virtuella datorer i Windows"
services: virtual-machines-windows
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: dfc1d1bb-a47d-4d7c-9fd2-f12050baacab
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2016
ms.author: guybo
ms.openlocfilehash: 5e96805f8880a30a5fc8779d8f07addb6d068c09
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-multiple-azure-virtual-machines"></a>Skapa flera virtuella Azure-datorer
Det finns många scenarier där du behöver skapa ett stort antal liknande virtuella datorer (VM). Några exempel på högpresterande Datorsystem, storskaliga dataanalys, skalbara och ofta tillståndslös mellannivå eller backend-servrar (till exempel webbservrar) och distribuerade databaser.

Den här artikeln beskrivs tillgängliga alternativ för att skapa flera virtuella datorer i Azure. Dessa alternativ utöver de enkla fall där du manuellt skapa en serie av virtuella datorer. Om du vill skapa många virtuella datorer, skala inte de processer som du vanligtvis använder bra om du behöver skapa fler än ett fåtal virtuella datorer.

Ett sätt att skapa många liknande virtuella datorer är att använda Azure Resource Manager-konstruktion av *resurs slingor*.

## <a name="resource-loops"></a>Resursen slingor
Resursen loopar är en syntaktiska shorthand i Azure Resource Manager-mallar. Resurs-loopar kan skapa en uppsättning liknande konfigurerad resurser i en slinga. Du kan använda resursen slingor för att skapa flera lagringskonton, nätverkskort eller virtuella datorer. Mer information om resurs slingor avser [skapa virtuella datorer i tillgänglighetsuppsättningar använda resursen slingor](https://azure.microsoft.com/documentation/templates/201-vm-copy-index-loops/).

## <a name="challenges-of-scale"></a>Utmaningar för skala
Även om resursen slingor gör det lättare att bygga ut en molninfrastruktur i skala och ger kortare mallar, förblir vissa utmaningar. Om du använder en resurs-slinga för att skapa 100 virtuella datorer, måste du korrelera network interface-styrenheter (NIC) med motsvarande Virtual Machines och storage-konton. Eftersom antalet virtuella datorer är sannolikt att skilja sig från antalet storage-konton, har du hantera olika resurs loop storlekar för. Det är problem med solvable men komplexitet ökar avsevärt med en skala.

En annan challenge inträffar när du behöver en infrastruktur som kan skalas Elastiskt. Till exempel kanske en Autoskala infrastruktur som automatiskt ökar eller minskar antalet virtuella datorer som svar på arbetsbelastningen. Virtuella datorer ger inte en integrerad mekanism för att variera antalet (skalbar och skala). Om du skalar i genom att ta bort virtuella datorer, är det svårt att garantera hög tillgänglighet genom att se till att virtuella datorer balanseras mellan domäner för uppdatering och feltolerans.

Slutligen när du använder en resurs loop går flera anrop för att skapa resurser du till den underliggande infrastrukturresursen. När flera anrop skapa liknande resurser, har Azure implicit möjlighet att förbättra den här designen och optimera distribution tillförlitlighet och prestanda. Det är där *skalningsuppsättningar i virtuella* kommer in.

## <a name="virtual-machine-scale-sets"></a>Skalningsuppsättningar för virtuella datorer
Skaluppsättningar för den virtuella datorn är en Azure Compute-resurs för att distribuera och hantera en uppsättning identiska virtuella datorer. Konfigurera samma, VM skala som är lätt att skala och skala ut med alla virtuella datorer. Du bara ändra antalet virtuella datorer i uppsättningen. Du kan också konfigurera skalningsuppsättningar Autoskala baserat på kraven för arbetsbelastningen.

För program som behöver för att skala ut och in beräkningsresurser balanserade skalningsåtgärder implicit fel- och update-domäner.

I stället för att sammanföra flera resurser, till exempel nätverkskort och virtuella datorer har en skalningsuppsättning i nätverket, lagring, virtuell dator och egenskaper för webbtjänsttillägg som du kan konfigurera centralt.

En introduktion till skalningsuppsättningar avser den [virtuella datorn anger produktsidan](https://azure.microsoft.com/services/virtual-machine-scale-sets/). Mer information går du till den [skalningsuppsättningarna för virtuella datorer dokumentationen](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/).

