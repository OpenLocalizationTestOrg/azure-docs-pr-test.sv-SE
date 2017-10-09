---
title: "aaaResource grupper för virtuella Windows-datorer i Azure | Microsoft Docs"
description: "Läs mer om hello viktiga design och implementeringslösning riktlinjer för att distribuera resursgrupper i Azure infrastrukturtjänster."
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 0fbcabcd-e96d-4d71-a526-431984887451
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 56b5670ec98bf3e80b7a622d5d760a57a7d20809
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-resource-group-guidelines-for-windows-vms"></a>Azure-resurs grupp riktlinjer för virtuella Windows-datorer

[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

Den här artikeln fokuserar på att förstå hur toologically bygga ut din miljö och gruppen Alla hello komponenter i resursgrupper.

## <a name="implementation-guidelines-for-resource-groups"></a>Implementeringsriktlinjer för resursgrupper
Beslut:

* Ska du toobuild ut resursgrupper genom hello kärnkomponenterna i infrastrukturen eller genom fullständig programdistribution?
* Behöver du toorestrict åtkomst tooResource grupper med hjälp av rollbaserad åtkomstkontroll?

Aktiviteter:

* Definiera vad kärnkomponenter infrastruktur och dedikerade resursgrupper som du behöver.
* Granska hur tooimplement Resource Manager-mallar för konsekvent och reproducerbara distributioner.
* Definiera vilka användarroller åtkomst som du behöver för att styra åtkomst till tooResource grupper.
* Skapa hello uppsättning resursgrupper med hjälp av din namngivningskonvention. Du kan använda Azure PowerShell eller hello-portalen.

## <a name="resource-groups"></a>Resursgrupper
I Azure kan du logiskt gruppera relaterade resurser, till exempel lagringskonton, virtuella nätverk och virtuella maskiner (VMs) toodeploy, hantera och underhålla dem som en enda enhet. Den här metoden gör det enklare toodeploy program när hålla alla hello relaterade resurser tillsammans från management perspektiv eller toogrant andra toothat gruppen för åtkomst av resurser. Resursgruppnamn kan vara upp till 90 tecken långt. För en mer omfattande förståelse för resursgrupper läsa hello [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).

En nyckelfunktion tooResource grupper är möjligheten toobuild i din miljö med hjälp av mallar. En mall är helt enkelt en JSON-fil som deklarerar hello lagring, nätverk, och beräkna resurser. Du kan också definiera relaterade anpassade skript eller konfigurationer tooapply. Med dessa mallar kan skapa du konsekvent och reproducerbara distributioner för dina program. Den här metoden gör det enkelt toobuild ut en miljö under utveckling och sedan använda den samma mall toocreate en Produktionsdistribution eller vice versa. För att bättre förstå med hjälp av mallar, läsa [hello genomgång mall](../../azure-resource-manager/resource-manager-template-walkthrough.md) som hjälper dig att varje steg i hello skapa en mall.

Det finns två olika metoder som du kan använda när du utformar din miljö med resursgrupper:

* Resursgrupper för varje programdistribution som kombinerar hello lagringskonton, virtuella nätverk och undernät, virtuella datorer kan läsa in belastningsutjämning, osv.
* Centraliserad resursgrupper som innehåller dina core virtuella nätverk och undernät eller storage-konton. Programmen är sedan i sin egen resursgrupper som bara innehåller virtuella datorer, belastningsutjämnare, nätverksgränssnitt, osv.

När du skalar upp, skapa centraliserade resursgrupper för ditt virtuella nätverk och undernät gör det enklare toobuild Nätverksanslutningar mellan lokala för hybrid anslutningsmöjligheter. hello alternativ metod är för varje program toohave sina egna virtuella nätverk som kräver konfiguration och underhåll.  [Rollbaserad åtkomstkontroll](../../active-directory/role-based-access-control-what-is.md) ge en detaljerad sätt toocontrol åtkomst tooResource grupper. Du kan styra hello-användare som kan komma åt resurserna för program i produktion, eller för hello kärnresurser infrastruktur kan du begränsa endast infrastruktur engineers toowork med dem.. Program-ägare kan bara ha åtkomst toohello programkomponenter inom deras resursgrupp och inte hello core Azure-infrastrukturen i din miljö. Överväg att hello-användare som behöver komma åt resurser i toohello och utforma din resursgrupper i enlighet med detta när du utformar din miljö. 

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]

