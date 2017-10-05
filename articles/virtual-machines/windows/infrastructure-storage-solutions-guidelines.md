---
title: "Lagringslösningar för virtuella Windows-datorer i Azure | Microsoft Docs"
description: "Läs mer om viktiga design och implementeringslösning riktlinjer för att distribuera lösningar för lagring i Azure infrastrukturtjänster."
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4ceb7d32-7a0d-4004-b701-c2bbcaff6b0b
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ba0d66c5af771ebcb3ca8e6742e15a5506d8fd61
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-storage-infrastructure-guidelines-for-windows-vms"></a>Azure storage infrastruktur riktlinjer för virtuella Windows-datorer

[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

Den här artikeln fokuserar på att förstå lagringsbehov och designöverväganden för att uppnå optimal virtuell dator (VM)-prestanda.

## <a name="implementation-guidelines-for-storage"></a>Implementeringsriktlinjer för lagring
Beslut:

* Ska du använda Azure hanterade diskar eller ohanterad diskar?
* Behöver du använda Standard eller Premium-lagring för din arbetsbelastning?
* Behöver du disk striping skapa diskar som är större än 4TB?
* Behöver du disk striping att uppnå optimalt i/o-prestanda för din arbetsbelastning?
* Vilken uppsättning av storage-konton måste du vara värd för din IT-arbetsbelastning eller infrastruktur?

Aktiviteter:

* Granska i/o-kraven för de program som du distribuerar och planera ett lämpligt antal och typ av storage-konton.
* Skapa uppsättning av lagringskonton med din namngivningskonvention. Du kan använda Azure PowerShell eller portalen.

## <a name="storage"></a>Lagring
Azure Storage är en viktig del av att distribuera och hantera virtuella maskiner (VMs) och program. Azure Storage tillhandahåller tjänster för att lagra fildata, Ostrukturerade data och meddelanden och det är också en del av infrastrukturen som stöder virtuella datorer.

[Azure-hanterade diskar](../../storage/storage-managed-disks-overview.md) hanterar lagring för dig i bakgrunden. Med ohanterad diskar kan du skapa storage-konton för att lagra diskar (VHD-filer) för din virtuella Azure-datorer. När du ökar, måste du kontrollera att du har skapat ytterligare lagringskonton så att du inte överskrider gränsen på IOPS för lagring med alla diskar. För hanterade diskar hantering lagring, är du inte längre begränsad lagringskontogränser (till exempel 20 000 IOPS / -kontot). Du måste också längre kopiera egna, anpassade avbildningar (VHD-filer) till flera lagringskonton. Du kan hantera dem på en central plats – ett lagringskonto per Azure-region – och använda dem för att skapa hundratals virtuella datorer i en prenumeration. Vi rekommenderar att du använder hanterade diskar för nya distributioner.

Det finns två typer av storage-konton för att stödja virtuella datorer:

* Standardlagring konton ger dig tillgång till blob storage (används för att lagra Virtuella Azure-diskar), table storage, queue storage- och fillagring.
* [Premium-lagring](../../storage/storage-premium-storage.md) konton ger stöd för i/o-intensiv arbetsbelastning, till exempel SQL-servrar i ett kluster med AlwaysOn diskar med hög prestanda, låg latens. Premium-lagring stöder för närvarande endast Virtuella Azure-diskar.

Azure skapar virtuella datorer med en operativsystemdisk, en tillfällig disk och noll eller flera valfria datadiskar. Operativsystemdisken och datadiskar är Azure sidblobar, medan den tillfälliga disken lagras lokalt på den nod där datorn finns. Vara försiktig när du utvecklar program för att endast använda den här tillfälliga disken för icke-beständig data som den virtuella datorn kan migreras mellan värdar under en underhållshändelse. Data som lagrats på den tillfälliga disken skulle gå förlorade.

Hållbarhet och hög tillgänglighet som den underliggande Azure Storage-miljön så att dina data fortfarande är skyddade mot oplanerat underhåll eller maskinvara fel. När du utformar din Azure Storage-miljö kan du replikera VM-lagring:

* lokalt inom en viss Azure-datacenter
* i Azure-datacenter inom en viss region
* i Azure-Datacenter över olika regioner

Du kan läsa [mer om replikeringsalternativen för hög tillgänglighet](../../storage/storage-introduction.md#replication-for-durability-and-high-availability).

Operativsystem och datadiskar har en maximal storlek på 4TB. Du kan använda lagringsutrymmen i Windows Server 2012 eller senare för att passera begränsningen av tillsammans poolning datadiskar för att presentera logiska volymer som är större än 4TB till den virtuella datorn.

Det finns vissa skalbarhetsgränser när du utformar dina distributioner i Azure Storage - mer information, se [prenumeration och tjänsten gränser, kvoter och begränsningar för Microsoft Azure](../../azure-subscription-service-limits.md#storage-limits). Se även [skalbarhets- och prestandamål för Azure storage](../../storage/storage-scalability-targets.md).

För lagring av programmet, kan du lagra Ostrukturerade objektdata dokument, bilder, säkerhetskopiering, konfigurationsdata, loggar, t.ex. med blob storage. Programmet kan skriva direkt till Azure blob storage i stället för ditt program skrivs till en virtuell disk som är ansluten till den virtuella datorn. BLOB-lagring ger också möjlighet att [hot och kall lagringsnivåer](../../storage/storage-blob-storage-tiers.md) beroende på dina behov för tillgänglighet och kostnaden begränsningar.

## <a name="striped-disks"></a>Stripe-diskar
Förutom att du kan skapa diskar som är större än 4TB i många fall förbättrar med striping för datadiskar prestandan genom att låta flera BLOB till lagringsplatsen för en enskild volym. Med striping fortsätter i/o som krävs för att skriva och läsa data från en enskild logisk disk parallellt.

Azure inför gränser för antalet diskar och mängden bandbredd som är tillgängliga, beroende på VM-storlek. Mer information finns i [storlekar för virtuella datorer](sizes.md).

Tänk på följande om du använder disk striping för Azure datadiskar:

* Koppla de högsta tillåtna datadiskar som för VM-storlek.
* Använda lagringsutrymmen.
* Undvik att använda Azure-datadisk cachelagringsalternativ (princip för cachelagring = ingen).

Mer information finns i [lagringsutrymmen - utformning för prestanda](http://social.technet.microsoft.com/wiki/contents/articles/15200.storage-spaces-designing-for-performance.aspx).

## <a name="multiple-storage-accounts"></a>Flera lagringskonton
Det här avsnittet gäller inte för [Azure hanterade diskar](../../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)eftersom du inte skapar en separat storage-konton. 

När du utformar din Azure Storage-miljö för ohanterade diskar kan du använda flera lagringskonton antalet virtuella datorer som du distribuerar ökar. Den här metoden kan distribuera ut läsning/skrivning i den underliggande Azure Storage-infrastrukturen för att underhålla optimal prestanda för virtuella datorer och program. När du utformar de program som du distribuerar du i/o-kraven som varje virtuell dator har och Saldo ut dessa virtuella datorer i Azure Storage-konton. Försök undvika gruppera alla hög I/O krävande virtuella datorer i till en eller två storage-konton.

Mer information om i/o-funktioner i de olika alternativen för Azure Storage och vissa rekommenderar maxkapacitet finns [skalbarhets- och prestandamål för Azure storage](../../storage/storage-scalability-targets.md).

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]

