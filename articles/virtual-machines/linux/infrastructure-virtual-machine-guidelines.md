---
title: Azure Linux virtuella datorer riktlinjer | Microsoft Docs
description: "Lär dig mer om viktiga design och implementeringslösning riktlinjer för distribution av virtuella Linux-datorer i Azure"
documentationcenter: 
services: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 740767d7-9a40-407b-93e7-c29dd976ffd7
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.openlocfilehash: 2d886795b301ab79272cae10a53831fd44c18f10
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-virtual-machines-guidelines-for-linux"></a>Virtuella Azure-datorer riktlinjer för Linux
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

Den här artikeln fokuserar på att förstå de nödvändiga planeringsstegen för att skapa och hantera virtuella maskiner (VMs) i Azure-miljön.

## <a name="implementation-guidelines-for-vms"></a>Implementeringsriktlinjer för virtuella datorer
Beslut:

* Hur många virtuella datorer kan du kräva för olika nivåer av program och komponenter i infrastrukturen?
* Vilka resurser som processor och minne varje virtuell dator behöver och vilka är lagringskraven?

Aktiviteter:

* Definiera arbetsbelastningar för ditt program och resurser som krävs för de virtuella datorerna.
* Justera resurskrav för varje virtuell dator med en lämplig VM storlek och lagring typ.
* Definiera resursgrupperna för olika nivåer och komponenter i din infrastruktur.
* Definiera namnkonventionen din VM.
* Skapa dina virtuella datorer med hjälp av Azure CLI, web portalen eller med Resource Manager-mallar.

## <a name="virtual-machines"></a>Virtuella datorer
En av de viktigaste resurserna i Azure-miljön är sannolikt virtuella datorer. Den här resursen är där du kör ditt program, databaser, autentiseringstjänster, osv.

Det är viktigt att förstå den [olika storlekar på VM](sizes.md) korrekt storleken miljön ur prestanda och kostnad. Om din virtuella dator inte har tillräckligt med CPU-kärnor eller minne, sjunker prestanda för programmet oavsett hur väl den designas och utvecklas. Granska de föreslagna arbetsbelastningarna för varje virtuell dator-serie som en startpunkt som du bestämmer dig för vilken storlek VM ska användas för varje komponent i din infrastruktur. Du kan [ändra storlek på en virtuell dator](change-vm-size.md) efter distributionen.

Lagring spelar en viktig roll i VM-prestanda. Du kan använda standardlagring som använder vanlig snurrande diskar, eller Premium-lagring för höga i/o-arbetsbelastningar och topprestanda använder SSD-diskar. Som med VM-storlek det kostnader att tänka på att välja lagringsmedium. Du kan läsa den [lagring infrastruktur riktlinjer artikel](infrastructure-storage-solutions-guidelines.md) att förstå hur du utformar lämplig lagring för optimala prestanda för dina virtuella datorer.

## <a name="resource-groups"></a>Resursgrupper
Komponenter till exempel virtuella datorer är logiskt grupperade för enklare hantering och underhåll med [Azure-resursgrupper](../../azure-resource-manager/resource-group-overview.md). Med resursgrupper kan du skapa, hantera och övervaka alla resurser som utgör ett visst program. Du kan också implementeras [rollbaserade åtkomstkontroller](../../active-directory/role-based-access-control-what-is.md) att bevilja åtkomst till andra i din grupp till de resurser som de kräver. Ta tid att planera din resursgrupper och rolltilldelningar. Det finns olika sätt att utforma och implementera resursgrupper, så bör du läsa den [resurs grupper riktlinjer artikel](infrastructure-resource-groups-guidelines.md) att förstå hur du bäst för att bygga ut dina virtuella datorer.

## <a name="templates"></a>Mallar
Du kan skapa mallar som definieras av deklarativ JSON-filer, skapa dina virtuella datorer. Mallar normalt skapa även lagring, nätverk, nätverkskort, IP-adressering, etc. tillsammans med virtuella datorerna. Du kan använda mallar för att skapa en konsekvent och reproducerbara miljöer för utveckling och testning i syfte att enkelt replikera produktionsmiljöer och vice versa. Du kan läsa mer om [skapar och använder mallar](../../azure-resource-manager/resource-group-overview.md#template-deployment) att förstå hur du kan använda dem för att skapa och distribuera din virtuella dator.

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]

