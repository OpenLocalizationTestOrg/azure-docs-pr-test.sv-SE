---
title: "Resursgrupper för Linux virtuella datorer i Azure | Microsoft Docs"
description: "Lär dig mer om viktiga riktlinjer för designen och implementeringen för distribution av resursgrupper i Azure infrastrukturtjänster."
documentationcenter: 
services: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 93ab9d93-965a-46b9-9c87-a10d652a6422
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 452acde571164a3ab4ce2dcccf99d2aed90361fe
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-resource-group-guidelines-for-linux-vms"></a>Azure-resurs grupp riktlinjer för virtuella Linux-datorer 

[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

Den här artikeln fokuserar på att förstå hur du skapar din miljö och gruppen Alla komponenter i resursgrupper logiskt.

## <a name="implementation-guidelines-for-resource-groups"></a>Implementeringsriktlinjer för resursgrupper
Beslut:

* Ska du bygga ut resursgrupper genom infrastrukturkomponenter kärnor, eller genom fullständig programdistribution?
* Behöver du begränsa åtkomsten till resursgrupper med hjälp av rollbaserad åtkomstkontroll?

Aktiviteter:

* Definiera vad kärnkomponenter infrastruktur och dedikerade resursgrupper som du behöver.
* Gå igenom hur du implementerar Resource Manager-mallar för konsekvent och reproducerbara distributioner.
* Definiera vilka användarroller åtkomst som du behöver för att styra åtkomsten till resursgrupper.
* Skapa en uppsättning med resursgrupper med hjälp av din namngivningskonvention. Du kan använda Azure CLI eller portal.

## <a name="resource-groups"></a>Resursgrupper
I Azure kan gruppera du relaterade resurser, till exempel lagringskonton, virtuella nätverk och virtuella datorer (VM) för att distribuera, hantera och underhålla dem som en enda enhet. Den här metoden gör det enklare att distribuera program samtidigt som de relaterade resurserna tillsammans ur management, eller att bevilja andra åtkomst till den gruppen av resurser. Resursgruppnamn kan vara upp till 90 tecken långt. För en mer omfattande förståelse för resursgrupper kan du läsa den [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).

En nyckelfunktion till resursgrupper är möjligheten att bygga din miljö som använder en JSON-fil som deklarerar lagring, nätverk, och beräkna resurser. Du kan också definiera relaterade anpassade skript eller konfigurationer för att tillämpa. Genom att använda dessa JSON-mallar kan skapa du konsekvent och reproducerbara distributioner för dina program. Den här metoden kan du skapa en miljö för utveckling och sedan använda samma mall för att skapa en Produktionsdistribution eller vice versa. För bättre förståelse om att använda mallar, läsa [genomgång mall](../../azure-resource-manager/resource-manager-template-walkthrough.md) som hjälper dig att varje steg i Skapa en JSON-mall.

Det finns två olika metoder som du kan använda när du utformar din miljö med resursgrupper:

* Resursgrupper för varje programdistribution som kombinerar lagringskonton, virtuella nätverk och undernät, virtuella datorer kan läsa in belastningsutjämning, osv.
* Centraliserad resursgrupper som innehåller dina core virtuella nätverk och undernät eller storage-konton. Programmen är sedan i sin egen resursgrupper som bara innehåller virtuella datorer, belastningsutjämnare, nätverksgränssnitt, osv.

Eftersom du skala ut, skapa centraliserade resursgrupper för ditt virtuella nätverk och undernät gör det enklare att skapa nätverksanslutningar anslutningar mellan lokala för hybrid anslutningsmöjligheter. Den alternativa metoden är för varje program har sina egna virtuella nätverk som kräver konfiguration och underhåll. [Rollbaserad åtkomstkontroll](../../active-directory/role-based-access-control-what-is.md) ger en detaljerad sätt styra åtkomsten till resursgrupper. Du kan styra vilka användare som kan komma åt resurserna för program i produktion, eller för infrastruktur kärnresurser kan du begränsa infrastruktur tekniker att arbeta med dem. Program-ägare har endast åtkomst till programkomponenterna inom deras resursgrupp och inte grundläggande Azure-infrastrukturen i din miljö. Överväg att användare som behöver åtkomst till resurser och utforma din resursgrupper i enlighet med detta när du utformar din miljö. 

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]

