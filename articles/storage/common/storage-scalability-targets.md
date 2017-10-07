---
title: "aaaAzure skalbarhets- och Storage prestandamål | Microsoft Docs"
description: "Läs mer om hello skalbarhets- och prestandamål för Azure Storage, inklusive kapacitet, förfrågningar och inkommande och utgående bandbredd för både standard och premium storage-konton. Förstå prestandamål för partitioner inom varje hello Azure Storage-tjänster."
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
ms.openlocfilehash: 7afe4366a02887b4e3d9781c26c8adda81adce95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-scalability-and-performance-targets"></a>Skalbarhets- och prestandamål i Azure Storage
## <a name="overview"></a>Översikt
Det här avsnittet beskriver hello ämnen för skalbarhet och prestanda för Microsoft Azure Storage. En sammanfattning av andra Azure gränser finns [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../../azure-subscription-service-limits.md).

> [!NOTE]
> Alla lagringskonton körs på hello nya flat nätverkstopologi och stöd för hello skalbarhet och prestanda mål som beskrivs nedan, oavsett när de har skapats. Läs mer på platt nätverk hello Azure Storage-arkitektur och skalbarhet [Microsoft Azure Storage: A hög Available Cloud Storage Service with Strong Consistency](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).
> 
> [!IMPORTANT]
> hello skalbarhets- och prestandamål som listas här är avancerade mål, men den kan nås. I samtliga fall måste hello begäran hastighet och bandbredden som uppnås genom ditt lagringskonto beror på hello storleken på objekt som lagras, hello åtkomstmönster används och hello typ av arbetsbelastning som utförs av ditt program. Vara säker på att tootest service-toodetermine om dess prestanda uppfyller dina krav. Om möjligt undviker plötsliga toppar i hello mängden trafik och kontrollera att trafik som är korrekt distribuerad mellan partitioner.
> 
> När programmet når hello gränsen för en partition kan hantera för din arbetsbelastning, Azure Storage börjar tooreturn-Felkod 503 (servern är upptagen) eller felkod 500 (åtgärden Timeout) svar. När detta inträffar bör hello program använda en princip för exponentiell backoff för återförsök. hello exponentiell backoff tillåter hello belastningen på hello partition toodecrease och tooease ut toppar i trafiken toothat partition.
> 
> 

Om hello måste överskrider hello skalbarhetsmål för ett enda lagringskonto för programmet, du bygga program-toouse flera lagringskonton och partitionera dataobjekt mellan dessa lagringskonton. Se [priser för Azure Storage](https://azure.microsoft.com/pricing/details/storage/) för information om Volympriser.

## <a name="scalability-targets-for-blobs-queues-tables-and-files"></a>Skalbarhetsmål för blobbar, köer, tabeller och filer
[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

<!-- conceptual info about disk limits -- applies toounmanaged and managed -->
## <a name="scalability-targets-for-virtual-machine-disks"></a>Skalbarhetsmål för virtuella diskar
[!INCLUDE [azure-storage-limits-vm-disks](../../../includes/azure-storage-limits-vm-disks.md)]

Se [Windows VM-storlekar](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) eller [Linux VM-storlekar](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) för ytterligare information.

## <a name="managed-virtual-machine-disks"></a>Hanterade virtuella diskar

[!INCLUDE [azure-storage-limits-vm-disks-managed](../../../includes/azure-storage-limits-vm-disks-managed.md)]

## <a name="unmanaged-virtual-machine-disks"></a>Ohanterad virtuella diskar
[!INCLUDE [azure-storage-limits-vm-disks-standard](../../../includes/azure-storage-limits-vm-disks-standard.md)]

[!INCLUDE [azure-storage-limits-vm-disks-premium](../../../includes/azure-storage-limits-vm-disks-premium.md)]

## <a name="scalability-targets-for-azure-resource-manager"></a>Skalbarhetsmål för Azure resource manager
[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

## <a name="partitions-in-azure-storage"></a>Partitioner i Azure-lagring
Alla objekt som innehåller data som lagras i Azure Storage (BLOB, meddelanden, enheter och filer) tillhör tooa partition och identifieras av en partitionsnyckel. hello partition avgör hur Azure Storage belastningsutjämning blobbar, meddelanden, enheter och filer över servrar toomeet hello trafikbehov av dessa objekt. hello Partitionsnyckeln är unikt och är används toolocate blob, meddelandet och entiteten.

hello tabellen ovan i [skalbarhetsmål för Standard Lagringskonton](#standard-storage-accounts) visar hello prestandamål för en enskild partition för varje tjänst.

Partitioner påverkar belastningsutjämning och skalbarhet för varje hello lagringstjänster i hello följande sätt:

* **Blobbar**: hello Partitionsnyckeln för en blob är kontonamnet behållarnamn + blobbnamnet. Detta innebär att varje blob kan ha en egen partition om belastningen på hello blob begär den. Blobbar kan distribueras över flera servrar i ordning tooscale ut toothem åtkomst, men en enda blob kan bara hanteras av en enskild server. Blobbar kan samlas i blob-behållare, finns men det inga partitionering effekter från den här grupperingen.
* **Filer**: hello partitionsnyckel för en fil är namnet + filen resursnamn-konto. Det innebär att alla filer i en filresurs finns också i en enda partition.
* **Meddelanden**: hello partitionsnyckel för ett meddelande är hello kontonamn + könamn, så att alla meddelanden i en kö är grupperade i en enda partition och hanteras av en enskild server. Olika köer kan bearbetas av olika servrar toobalance hello ladda för men många köer som kan ha ett lagringskonto.
* **Entiteter**: hello partitionsnyckel för en entitet är kontonamnet + tabellnamn + partitionsnyckel, där hello Partitionsnyckeln är hello värdet för hello krävs användardefinierade **PartitionKey** egenskapen för hello entiteten. Alla entiteter med samma partitionsnyckelvärde grupperas i hello samma partition av och hanteras av hello samma hello partitions-server. Det här är en viktig punkt toounderstand vid utformning av ditt program. Ditt program bör balansera hello skalbarhet fördelarna med att sprida entiteter över flera partitioner med hello data access fördelarna med att gruppera enheter i en enda partition.  

En stor fördel toogrouping en uppsättning enheter i en tabell i en enda partition är att det är möjligt tooperform atomiska batchåtgärder över entiteter i hello samma partition, eftersom det finns en partition på en enskild server. Om du inte vill tooperform batch-åtgärder på en grupp av enheter, bör du gruppera dem med hello samma partitionsnyckel. 

Hej på andra sidan, enheter som är i hello samma tabell men har olika partitionsnycklar kan belastningsutjämnas mellan olika servrar, vilket gör det möjligt toohave bättre skalbarhet.

Detaljerade rekommendationer för hur man designar partitionering strategi för tabeller hittar [här](https://msdn.microsoft.com/library/azure/hh508997.aspx).

## <a name="see-also"></a>Se även
* [Storage-priser](https://azure.microsoft.com/pricing/details/storage/)
* [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../../azure-subscription-service-limits.md)
* [Premium Storage: Lagring med höga prestanda för Azure Virtual Machines-arbetsbelastningar](../storage-premium-storage.md)
* [Azure Storage-replikering](../storage-redundancy.md)
* [Microsoft Azure Storage-prestanda och skalbarhet Checklista](../storage-performance-checklist.md)
* [Microsoft Azure Storage: En högtillgänglig lagring molntjänst med stark konsekvens](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

