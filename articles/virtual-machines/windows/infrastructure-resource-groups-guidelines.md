---
title: "Resursgrupper för virtuella Windows-datorer i Azure | Microsoft Docs"
description: "Lär dig mer om viktiga riktlinjer för designen och implementeringen för distribution av resursgrupper i Azure infrastrukturtjänster."
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
ms.openlocfilehash: c0aca77af04f895d516f4943188d7890ae9586b7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-resource-group-guidelines-for-windows-vms"></a>Azure-resurs grupp riktlinjer för virtuella Windows-datorer

[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

Den här artikeln fokuserar på att förstå hur du skapar din miljö och gruppen Alla komponenter i resursgrupper logiskt.

## <a name="implementation-guidelines-for-resource-groups"></a>Implementeringsriktlinjer för resursgrupper
Beslut:

* Ska du bygga ut resursgrupper genom infrastrukturkomponenter kärnor, eller genom fullständig programdistribution?
* Behöver du begränsa åtkomsten till resursgrupper med hjälp av rollbaserad åtkomstkontroll?

Aktiviteter:

* Definiera vad kärnkomponenter infrastruktur och dedikerade resursgrupper som du behöver.
* Gå igenom hur du implementerar Resource Manager-mallar för konsekvent och reproducerbara distributioner.
* Definiera vilka användarroller åtkomst som du behöver för att styra åtkomsten till resursgrupper.
* Skapa en uppsättning med resursgrupper med hjälp av din namngivningskonvention. Du kan använda Azure PowerShell eller portalen.

## <a name="resource-groups"></a>Resursgrupper
I Azure kan gruppera du relaterade resurser, till exempel lagringskonton, virtuella nätverk och virtuella datorer (VM) för att distribuera, hantera och underhålla dem som en enda enhet. Den här metoden gör det enklare att distribuera program samtidigt som de relaterade resurserna tillsammans ur management, eller att bevilja andra åtkomst till den gruppen av resurser. Resursgruppnamn kan vara upp till 90 tecken långt. En mer omfattande förståelse av resursgrupper, läsa den [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).

En nyckelfunktion till resursgrupper är möjligheten att bygga ut din miljö med hjälp av mallar. En mall är helt enkelt en JSON-fil som deklarerar lagring, nätverk, och beräkna resurser. Du kan också definiera relaterade anpassade skript eller konfigurationer för att tillämpa. Med dessa mallar kan skapa du konsekvent och reproducerbara distributioner för dina program. Den här metoden gör det enkelt att bygga ut en miljö för utveckling och sedan använda samma mall för att skapa en Produktionsdistribution eller vice versa. För att bättre förstå med hjälp av mallar, läsa [genomgång mall](../../azure-resource-manager/resource-manager-template-walkthrough.md) som hjälper dig att varje steg i Skapa en mall.

Det finns två olika metoder som du kan använda när du utformar din miljö med resursgrupper:

* Resursgrupper för varje programdistribution som kombinerar lagringskonton, virtuella nätverk och undernät, virtuella datorer kan läsa in belastningsutjämning, osv.
* Centraliserad resursgrupper som innehåller dina core virtuella nätverk och undernät eller storage-konton. Programmen är sedan i sin egen resursgrupper som bara innehåller virtuella datorer, belastningsutjämnare, nätverksgränssnitt, osv.

Eftersom du skala ut, skapa centraliserade resursgrupper för ditt virtuella nätverk och undernät gör det enklare att skapa nätverksanslutningar anslutningar mellan lokala för hybrid anslutningsmöjligheter. Den alternativa metoden är för varje program har sina egna virtuella nätverk som kräver konfiguration och underhåll.  [Rollbaserad åtkomstkontroll](../../active-directory/role-based-access-control-what-is.md) ger en detaljerad sätt styra åtkomsten till resursgrupper. Du kan styra vilka användare som kan komma åt resurserna för program i produktion, eller för infrastruktur kärnresurser kan du begränsa infrastruktur tekniker att arbeta med dem. Program-ägare har endast åtkomst till programkomponenterna inom deras resursgrupp och inte grundläggande Azure-infrastrukturen i din miljö. Överväg att användare som behöver åtkomst till resurser och utforma din resursgrupper i enlighet med detta när du utformar din miljö. 

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]

