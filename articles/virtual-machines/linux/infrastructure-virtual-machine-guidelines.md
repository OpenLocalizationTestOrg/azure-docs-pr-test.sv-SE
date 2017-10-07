---
title: aaaAzure Linux virtuella datorer riktlinjer | Microsoft Docs
description: "Lär dig mer om hello viktiga design och implementeringslösning riktlinjer för distribution av virtuella Linux-datorer i Azure"
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
ms.openlocfilehash: 0f779f791005441b6b16e106a3dc5a095bb33034
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-virtual-machines-guidelines-for-linux"></a>Virtuella Azure-datorer riktlinjer för Linux
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

Den här artikeln fokuserar på förstå hello krävs planeringssteg för att skapa och hantera virtuella maskiner (VMs) i Azure-miljön.

## <a name="implementation-guidelines-for-vms"></a>Implementeringsriktlinjer för virtuella datorer
Beslut:

* Hur många virtuella datorer kan du kräva för olika nivåer av program och komponenter i infrastrukturen?
* Vilka resurser som processor och minne varje virtuell dator behöver och vilka är lagringskraven för hello?

Aktiviteter:

* Definiera hello arbetsbelastningar för ditt program och hello resurser hello VM: ar kräver.
* Justera hello resurskrav för varje virtuell dator med hello lämpliga VM storlek och lagring.
* Definiera resursgrupperna för hello olika nivåer och komponenter i din infrastruktur.
* Definiera namnkonventionen din VM.
* Skapa dina virtuella datorer med hjälp av hello Azure CLI, web portalen eller med Resource Manager-mallar.

## <a name="virtual-machines"></a>Virtuella datorer
En hello huvudsakliga resurser i Azure-miljön är sannolikt virtuella datorer. Den här resursen är där du kör ditt program, databaser, autentiseringstjänster, osv.

Det är viktigt toounderstand hello [olika storlekar på VM](sizes.md) toocorrectly storlek miljön ur prestanda och kostnad. Om din virtuella dator inte har tillräckligt med CPU-kärnor eller minne, sjunker prestanda för programmet oavsett hur väl den designas och utvecklas. Granska hello föreslagna arbetsbelastningar för varje virtuell dator-serie som en startpunkt som du bestämmer dig för vilken storlek VM toouse för varje komponent i din infrastruktur. Du kan [ändra hello storleken på en virtuell dator](change-vm-size.md) efter distributionen.

Lagring spelar en viktig roll i VM-prestanda. Du kan använda standardlagring som använder vanlig snurrande diskar, eller Premium-lagring för höga i/o-arbetsbelastningar och topprestanda använder SSD-diskar. Som med hello VM-storlek, det är kostnad överväganden tooselecting hello lagringsmedium. Du kan läsa hello [lagring infrastruktur riktlinjer artikel](infrastructure-storage-solutions-guidelines.md) toounderstand hur toodesign lämpliga lagring för optimala prestanda för dina virtuella datorer.

## <a name="resource-groups"></a>Resursgrupper
Komponenter till exempel virtuella datorer är logiskt grupperade för enklare hantering och underhåll med [Azure-resursgrupper](../../azure-resource-manager/resource-group-overview.md). Med resursgrupper kan du skapa, hantera och övervaka alla hello-resurser som utgör ett visst program. Du kan också implementeras [rollbaserade åtkomstkontroller](../../active-directory/role-based-access-control-what-is.md) toogrant åtkomst tooothers med ditt team tooonly hello-resurser som de behöver. Ta tid tooplan ut resursgrupperna och rolltilldelningar. Det finns olika sätt tooactually utforma och implementera resursgrupper, så att tooread hello [resurs grupper riktlinjer artikel](infrastructure-resource-groups-guidelines.md) toounderstand bästa toobuild ut dina virtuella datorer.

## <a name="templates"></a>Mallar
Du kan skapa mallar som definieras av deklarativ JSON-filer, toocreate dina virtuella datorer. Mallar vanligtvis bygga också hello som krävs för lagring, nätverk, nätverksgränssnitt, IP-adressering, etc. tillsammans med hello själva de virtuella datorerna. Du kan använda mallar toocreate konsekvent och reproducerbara miljöer för utveckling och testning syften tooeasily replikera produktionsmiljöer och vice versa. Du kan läsa mer om [skapar och använder mallar](../../azure-resource-manager/resource-group-overview.md#template-deployment) toounderstand hur du kan använda dem för att skapa och distribuera din virtuella dator.

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]

