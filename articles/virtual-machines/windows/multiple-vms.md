---
title: aaaCreate flera virtuella datorer | Microsoft Docs
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
ms.openlocfilehash: 37729fabd91049744f6bd94c55221f5c1c65d527
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-multiple-azure-virtual-machines"></a>Skapa flera virtuella Azure-datorer
Det finns många scenarier där du behöver toocreate ett stort antal liknande virtuella datorer (VM). Några exempel på högpresterande Datorsystem, storskaliga dataanalys, skalbara och ofta tillståndslös mellannivå eller backend-servrar (till exempel webbservrar) och distribuerade databaser.

Den här artikeln beskrivs hello alternativen toocreate flera virtuella datorer i Azure. Dessa alternativ utöver hello enkla fall där du manuellt skapa en serie av virtuella datorer. toocreate många virtuella datorer hello processer som du vanligtvis använder inte skala bra om du behöver toocreate mer än ett fåtal virtuella datorer.

Enkelriktade toocreate många liknande virtuella datorer är toouse hello Azure Resource Manager konstruktion av *resurs slingor*.

## <a name="resource-loops"></a>Resursen slingor
Resursen loopar är en syntaktiska shorthand i Azure Resource Manager-mallar. Resurs-loopar kan skapa en uppsättning liknande konfigurerad resurser i en slinga. Du kan använda resursen slingor toocreate flera lagringskonton, nätverkskort eller virtuella datorer. Mer information om resurs slingor finns för[skapa virtuella datorer i tillgänglighetsuppsättningar använda resursen slingor](https://azure.microsoft.com/documentation/templates/201-vm-copy-index-loops/).

## <a name="challenges-of-scale"></a>Utmaningar för skala
Även om resursen slingor gör det enklare toobuild ut en molninfrastruktur i skala och ger kortare mallar, förblir vissa utmaningar. Du måste till exempel toocorrelate network interface-styrenheter (NIC) med motsvarande Virtual Machines och storage-konton om du använder en resurs loop toocreate 100 virtuella-datorer. Eftersom hello antal virtuella datorer är sannolikt toobe skiljer sig från hello antal lagringskonton, har du toodeal med en annan resurs loop storlekar för. Det är problem med solvable men hello komplexitet ökar avsevärt med en skala.

En annan challenge inträffar när du behöver en infrastruktur som kan skalas Elastiskt. Till exempel kanske en Autoskala infrastruktur som ökar eller minskar hello antal virtuella datorer i svaret tooworkload automatiskt. Virtuella datorer ger inte en integrerad mekanism toovary i antal (skalbar och skala). Om du skalar i genom att ta bort virtuella datorer, är det svårt tooguarantee hög tillgänglighet genom att se till att virtuella datorer balanseras mellan domäner för uppdatering och feltolerans.

Slutligen går flera anrop toocreate resurser toohello underliggande infrastruktur när du använder en resurs-loop. När flera anrop skapa liknande resurser, Azure har en implicit möjlighet tooimprove på den här designen och optimera distribution tillförlitlighet och prestanda. Det är där *skalningsuppsättningar i virtuella* kommer in.

## <a name="virtual-machine-scale-sets"></a>Skalningsuppsättningar för virtuella datorer
Skaluppsättningar för den virtuella datorn är en Azure Compute resurs toodeploy och hantera en uppsättning identiska virtuella datorer. Med alla virtuella datorer som konfigurerats Hej samma, skalningsuppsättningar är enkelt tooscale i och skala ut. Ändrar du bara hello antal virtuella datorer i hello uppsättningen. Du kan också konfigurera VM scale anger tooautoscale baserat på hello behoven hos hello arbetsbelastning.

För program som behöver tooscale beräkningsresurser ut och i skala balanserade operations implicit fel- och update-domäner.

I stället för att sammanföra flera resurser, till exempel nätverkskort och virtuella datorer har en skalningsuppsättning i nätverket, lagring, virtuell dator och egenskaper för webbtjänsttillägg som du kan konfigurera centralt.

För en introduktion tooVM skala anger information finns i toohello [virtuella datorn anger produktsidan](https://azure.microsoft.com/services/virtual-machine-scale-sets/). Mer detaljerad information gå toohello [skalningsuppsättningarna för virtuella datorer dokumentationen](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/).

