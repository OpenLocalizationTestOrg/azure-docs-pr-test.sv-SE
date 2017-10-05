---
title: "Azure Storage skalbarhets- och prestandamål | Microsoft Docs"
description: "Läs mer om skalbarhets- och prestandamål för Azure Storage, inklusive kapacitet, förfrågningar och inkommande och utgående bandbredd för både standard och premium storage-konton. Förstå prestandamål för partitioner inom varje Azure Storage-tjänster."
services: storage
documentationcenter: na
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: be721bd3-159f-40a1-88c1-96418537fe75
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 07/12/2017
ms.author: robinsh
ms.openlocfilehash: ed90e5d63e4c93f9c5054b02d2b4457b44caf6eb
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-storage-scalability-and-performance-targets"></a>Skalbarhets- och prestandamål i Azure Storage
## <a name="overview"></a>Översikt
Det här avsnittet beskrivs avsnitt skalbarhet och prestanda för Microsoft Azure Storage. En sammanfattning av andra Azure gränser finns [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../azure-subscription-service-limits.md).

> [!NOTE]
> Alla lagringskonton körs på den nya flat nätverkstopologin och stöd för skalbarhet och prestanda mål som beskrivs nedan, oavsett när de har skapats. Läs mer på Azure Storage platt nätverk-arkitektur och skalbarhet [Microsoft Azure Storage: A hög Available Cloud Storage Service with Strong Consistency](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).
> 
> [!IMPORTANT]
> Skalbarhets- och prestandamål som listas här är avancerade mål, men den kan nås. I samtliga fall, förfrågningar och bandbredden som uppnås genom din lagring konto beror på storleken på objekt som lagras, åtkomstmönster som används, och vilken typ av arbetsbelastning programmet utför. Se till att testa tjänsten för att avgöra om dess prestanda uppfyller dina krav. Om möjligt undviker plötsliga toppar i mängden trafik och kontrollera att trafik som är korrekt distribuerad mellan partitioner.
> 
> När programmet når gränsen för en partition kan hantera för din arbetsbelastning, börjar Azure Storage att returnera Felkod 503 (servern är upptagen) eller felkoden 500 (åtgärden Timeout) svar. När detta inträffar ska programmet använda en princip för exponentiell backoff för återförsök. Exponentiell backoff möjliggör att belastningen på partitionen minska och övergång toppar i trafiken till den aktuella partitionen.
> 
> 

Om behov av programmet överskrider skalbarhetsmål för ett enda lagringskonto, du skapar ditt program att använda flera lagringskonton och partitionera dataobjekt mellan dessa lagringskonton. Se [priser för Azure Storage](https://azure.microsoft.com/pricing/details/storage/) för information om Volympriser.

## <a name="scalability-targets-for-blobs-queues-tables-and-files"></a>Skalbarhetsmål för blobbar, köer, tabeller och filer
[!INCLUDE [azure-storage-limits](../../includes/azure-storage-limits.md)]

<!-- conceptual info about disk limits -- applies to unmanaged and managed -->
## <a name="scalability-targets-for-virtual-machine-disks"></a>Skalbarhetsmål för virtuella diskar
[!INCLUDE [azure-storage-limits-vm-disks](../../includes/azure-storage-limits-vm-disks.md)]

Se [Windows VM-storlekar](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) eller [Linux VM-storlekar](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) för ytterligare information.

## <a name="managed-virtual-machine-disks"></a>Hanterade virtuella diskar

[!INCLUDE [azure-storage-limits-vm-disks-managed](../../includes/azure-storage-limits-vm-disks-managed.md)]

## <a name="unmanaged-virtual-machine-disks"></a>Ohanterad virtuella diskar
[!INCLUDE [azure-storage-limits-vm-disks-standard](../../includes/azure-storage-limits-vm-disks-standard.md)]

[!INCLUDE [azure-storage-limits-vm-disks-premium](../../includes/azure-storage-limits-vm-disks-premium.md)]

## <a name="scalability-targets-for-azure-resource-manager"></a>Skalbarhetsmål för Azure resource manager
[!INCLUDE [azure-storage-limits-azure-resource-manager](../../includes/azure-storage-limits-azure-resource-manager.md)]

## <a name="partitions-in-azure-storage"></a>Partitioner i Azure-lagring
Alla objekt som innehåller data som lagras i Azure Storage (BLOB, meddelanden, enheter och filer) tillhör en partition och identifieras av en partitionsnyckel. Partitionen avgör hur Azure Storage belastningsutjämning blobbar, meddelanden, enheter och filer över servrar så att den passar trafik till dessa objekt. Partitionsnyckeln är unikt och används för att leta upp en blob, meddelande eller enhet.

Tabellen ovan i [skalbarhetsmål för Standard Lagringskonton](#standard-storage-accounts) visar prestandamål för en enskild partition för varje tjänst.

Partitioner påverkar belastningsutjämning och skalbarhet för var och en av lagringstjänsterna på följande sätt:

* **Blobbar**: Partitionsnyckeln för en blob är kontonamnet behållarnamn + blobbnamnet. Detta innebär att varje blob kan ha en egen partition om belastningen på blob begär den. Blobbar kan distribueras över flera servrar för att skala ut åtkomst till dem, men en enda blob kan bara hanteras av en enskild server. Blobbar kan samlas i blob-behållare, finns men det inga partitionering effekter från den här grupperingen.
* **Filer**: Partitionsnyckeln för en fil är namnet + filen resursnamn-konto. Det innebär att alla filer i en filresurs finns också i en enda partition.
* **Meddelanden**: Partitionsnyckeln för ett meddelande är kontonamnet + könamn, så att alla meddelanden i en kö är grupperade i en enda partition och hanteras av en enskild server. Olika köer kan bearbetas av olika servrar för belastningsutjämning för men många köer som kan ha ett lagringskonto.
* **Entiteter**: Partitionsnyckeln för en entitet är kontot namn + tabellnamn + partitions-nyckel, där Partitionsnyckeln är värdet för det obligatoriska användardefinierade **PartitionKey** -egenskapen för entiteten. Alla entiteter med samma partitionsnyckelvärde grupperas i samma partition och hanteras av samma partition server. Det här är en viktig aspekt att förstå vid utformning av ditt program. Ditt program bör balansera skalbarhet fördelarna med att sprida entiteter över flera partitioner med data access fördelarna med att gruppera enheter i en enda partition.  

En stor fördel att gruppera en uppsättning enheter i en tabell i en enda partition är att det är möjligt att utföra batchåtgärder atomiska via entiteter i samma partition, eftersom det finns en partition på en enskild server. Om du vill utföra batchåtgärder på en grupp av enheter, bör du gruppera dem med samma partitionsnyckel. 

Å andra sidan enheter som är i samma tabell, men de har olika partitionsnycklar kan vara belastningsutjämnade via olika servrar, vilket gör det möjligt att ha bättre skalbarhet.

Detaljerade rekommendationer för hur man designar partitionering strategi för tabeller hittar [här](https://msdn.microsoft.com/library/azure/hh508997.aspx).

## <a name="see-also"></a>Se även
* [Storage-priser](https://azure.microsoft.com/pricing/details/storage/)
* [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../azure-subscription-service-limits.md)
* [Premium Storage: Lagring med höga prestanda för Azure Virtual Machines-arbetsbelastningar](storage-premium-storage.md)
* [Azure Storage-replikering](storage-redundancy.md)
* [Microsoft Azure Storage-prestanda och skalbarhet Checklista](storage-performance-checklist.md)
* [Microsoft Azure Storage: En högtillgänglig lagring molntjänst med stark konsekvens](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

