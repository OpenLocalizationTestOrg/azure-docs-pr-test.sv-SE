---
title: "aaaStorage lösningar för virtuella Windows-datorer i Azure | Microsoft Docs"
description: "Läs mer om hello viktiga design och implementeringslösning riktlinjer för att distribuera lösningar för lagring i Azure infrastrukturtjänster."
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
ms.openlocfilehash: 57c27a7305964a56e6b2d1e73dee8e7da19fa8f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-infrastructure-guidelines-for-windows-vms"></a>Azure storage infrastruktur riktlinjer för virtuella Windows-datorer

[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

Den här artikeln fokuserar på att förstå lagringsbehov och designöverväganden för att uppnå optimal virtuell dator (VM)-prestanda.

## <a name="implementation-guidelines-for-storage"></a>Implementeringsriktlinjer för lagring
Beslut:

* Ska du toouse Azure hanterade diskar eller ohanterad diskar?
* Behöver du toouse Standard eller Premium-lagring för din arbetsbelastning?
* Behöver du disk striping toocreate diskar större än 4TB?
* Behöver du disk striping tooachieve i/o optimalt för din arbetsbelastning?
* Vilken uppsättning av lagringskonton behöver du toohost din IT-arbetsbelastning eller infrastrukturen?

Aktiviteter:

* Granska i/o-kraven för hello program som du distribuerar och planera hello lämpligt antal och typer av lagringskonton.
* Skapa hello uppsättning lagringskonton med din namngivningskonvention. Du kan använda Azure PowerShell eller hello-portalen.

## <a name="storage"></a>Lagring
Azure Storage är en viktig del av att distribuera och hantera virtuella maskiner (VMs) och program. Azure Storage tillhandahåller tjänster för att lagra fildata, Ostrukturerade data och meddelanden och det är också en del av hello-infrastrukturen som stöder virtuella datorer.

[Azure-hanterade diskar](../../storage/storage-managed-disks-overview.md) hanterar lagring du hello bakgrunden. Med ohanterad diskar skapar du lagringskonton toohold hello diskar (VHD-filer) för din virtuella Azure-datorer. När du ökar, måste du kontrollera att du har skapat ytterligare lagringskonton så att du inte överskrider hello IOPS gränsen för lagring med alla diskar. För hanterade diskar hantering lagring, är du inte längre begränsad hello lagringskontogränser (till exempel 20 000 IOPS / -kontot). Du har också längre toocopy anpassade avbildningar (VHD-filer) toomultiple storage-konton. Du kan hantera dem på en central plats – ett lagringskonto per Azure-region – och använder dem toocreate hundratals för virtuella datorer i en prenumeration. Vi rekommenderar att du använder hanterade diskar för nya distributioner.

Det finns två typer av storage-konton för att stödja virtuella datorer:

* Standardlagring konton ger dig tillgång tooblob lagring (används för att lagra Virtuella Azure-diskar), table storage, queue storage och fillagring.
* [Premium-lagring](../../storage/storage-premium-storage.md) konton ger stöd för i/o-intensiv arbetsbelastning, till exempel SQL-servrar i ett kluster med AlwaysOn diskar med hög prestanda, låg latens. Premium-lagring stöder för närvarande endast Virtuella Azure-diskar.

Azure skapar virtuella datorer med en operativsystemdisk, en tillfällig disk och noll eller flera valfria datadiskar. Hej operativsystemdisken och datadiskar är Azure sidblobar, medan hello diskutrymme lagras lokalt på hello-nod där hello datorn finns. Vara försiktig när du skapar program tooonly använder den här tillfälliga disken för icke-beständig data som hello VM kan migreras mellan värdar under en underhållshändelse. Data som lagrats på hello diskutrymme skulle gå förlorade.

Hållbarhet och hög tillgänglighet som hello underliggande Azure Storage miljö tooensure att dina data fortfarande är skyddade mot oplanerat underhåll eller maskinvara fel. När du utformar din Azure Storage-miljö kan du välja tooreplicate VM-lagring:

* lokalt inom en viss Azure-datacenter
* i Azure-datacenter inom en viss region
* i Azure-Datacenter över olika regioner

Du kan läsa [mer om hello replikeringsalternativ för hög tillgänglighet](../../storage/storage-introduction.md#replication-for-durability-and-high-availability).

Operativsystem och datadiskar har en maximal storlek på 4TB. Du kan använda lagringsutrymmen i Windows Server 2012 eller senare toosurpass begränsningen av tillsammans poolning data diskar toopresent logiska volymer som är större än 4TB tooyour VM.

Det finns vissa skalbarhetsgränser när du utformar dina distributioner i Azure Storage - mer information, se [prenumeration och tjänsten gränser, kvoter och begränsningar för Microsoft Azure](../../azure-subscription-service-limits.md#storage-limits). Se även [skalbarhets- och prestandamål för Azure storage](../../storage/storage-scalability-targets.md).

För lagring av programmet, kan du lagra Ostrukturerade objektdata dokument, bilder, säkerhetskopiering, konfigurationsdata, loggar, t.ex. med blob storage. I stället för ditt program skrivning tooa virtuell disk som är ansluten toohello VM kan hello program skriva direkt tooAzure blob-lagring. BLOB storage tillhandahåller också hello alternativ för [hot och kall lagringsnivåer](../../storage/storage-blob-storage-tiers.md) beroende på dina behov för tillgänglighet och kostnaden begränsningar.

## <a name="striped-disks"></a>Stripe-diskar
Förutom att tillåta toocreate diskar större än 4TB i många fall, förbättrar med hjälp av striping för datadiskar prestandan genom att låta flera blobbar tooback hello lagring för en enskild volym. Med striping, hello i/o krävs toowrite och läsa data från en enskild logisk disk fortsätter parallellt.

Azure inför gränser hello antalet diskar och mängden bandbredd som är tillgängliga, beroende på hello VM-storlek. Mer information finns i [storlekar för virtuella datorer](sizes.md).

Om du använder disk striping för Azure datadiskar, bör du hello följande riktlinjer:

* Koppla hello högsta tillåtna datadiskar för hello VM-storlek.
* Använda lagringsutrymmen.
* Undvik att använda Azure-datadisk cachelagringsalternativ (princip för cachelagring = ingen).

Mer information finns i [lagringsutrymmen - utformning för prestanda](http://social.technet.microsoft.com/wiki/contents/articles/15200.storage-spaces-designing-for-performance.aspx).

## <a name="multiple-storage-accounts"></a>Flera lagringskonton
Det här avsnittet gäller inte för[Azure hanterade diskar](../../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)eftersom du inte skapar en separat storage-konton. 

När du utformar din Azure Storage-miljö för ohanterade diskar kan du använda flera lagringskonton som hello antal virtuella datorer som du distribuerar ökar. Den här metoden hjälper till att distribuera ut hello i/o över hello underliggande Azure Storage infrastruktur toomaintain optimal prestanda för virtuella datorer och program. När du utformar hello-program som du distribuerar du hello i/o-kraven har varje virtuell dator och Saldo ut dessa virtuella datorer i Azure Storage-konton. Försök tooavoid gruppera alla hello höga i/o krävande virtuella datorer i toojust en eller två storage-konton.

Mer information om hello i/o-funktioner för hello olika Azure Storage-alternativ och vissa rekommenderar maxkapacitet, finns [skalbarhets- och prestandamål för Azure storage](../../storage/storage-scalability-targets.md).

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]

