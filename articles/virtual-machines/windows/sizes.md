---
title: aaaWindows VM-storlekar i Azure | Microsoft Docs
description: "Visar hello olika storlekar för virtuella Windows-datorer i Azure."
services: virtual-machines-windows
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: aabf0d30-04eb-4d34-b44a-69f8bfb84f22
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/28/2017
ms.author: jonbeck
ms.openlocfilehash: 80bd8241b134031c41b56224e841c7557c4845cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sizes-for-windows-virtual-machines-in-azure"></a>Storlekar för virtuella Windows-datorer i Azure

Den här artikeln beskriver hello tillgängliga storlekar och alternativ för hello Azure virtuella datorer som du kan använda toorun dina Windows-appar och arbetsbelastningar. Det ger också distribution överväganden toobe medveten om när du planerar toouse dessa resurser.  Den här artikeln är också tillgängligt för [virtuella Linux-datorer](../linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


| Typ                     | Storlekar           |    Beskrivning       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Generellt syfte](sizes-general.md)          | Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0 7 | Balanserat förhållande mellan processor och minne. Idealisk för testning och utveckling, små toomedium databaser och låg toomedium trafik webbservrar. |
| [Beräkningsoptimerad](sizes-compute.md)        | FS, F             | Högt förhållande mellan processor och minne. Bra för webbservrar med medelhög trafik, nätverkstillämpningar, batchprocesser och programservrar.        |
| [Minnesoptimerad](../virtual-machines-windows-sizes-memory.md)         | Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D   | Förhållandet mellan hög minne till CPU. Perfekt för relationsdatabas servrar, medelhög toolarge cacheminnen och analyser i minnet.                 |
| [Lagringsoptimerad](../virtual-machines-windows-sizes-storage.md)        | Ls                | Högt diskgenomflöde och I/O. Perfekt för stordata, SQL- och NoSQL-databaser.                                                         |
| [GPU](sizes-gpu.md)            | NV NC            | Särskilda virtuella datorer som mål för tunga grafisk återgivning och redigering av video. Tillgängligt med en eller flera GPU-kort.       |
| [Databehandling med höga prestanda](sizes-hpc.md) | H, A8-11          | Våra virtuella datorer med de snabbaste och mest kraftfulla processorerna med nätverksgränssnitt för stora dataflöden (RDMA). 

<br> 

- Mer information om priser för hello olika storlekar finns [prissättning för Virtual Machines](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows). 
- toosee allmänna begränsningar på Azure Virtual Machines finns [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../../azure-subscription-service-limits.md).
- Lagringskostnader beräknas separat använda sidor i hello storage-konto. Mer information [priser för Azure Storage](https://azure.microsoft.com/pricing/details/storage/).
- Läs mer om hur [Azure compute-enheter (ACU)](acu.md) kan hjälpa dig att jämföra beräkning prestanda över Azure SKU: er.



## <a name="rest-api"></a>REST-API

Information om hur du använder hello REST API tooquery storlekar på VM finns hello följande:

- [Visa en lista med tillgängliga virtuella datorstorlekar för storleksändring](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-for-resizing)
- [Visa en lista med tillgängliga virtuella datorstorlekar för en prenumeration](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region)
- [Visa en lista med tillgängliga virtuella datorstorlekar i en tillgänglighetsuppsättning](
https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-availability-set)

## <a name="acu"></a>ACU

Läs mer om hur [Azure compute-enheter (ACU)](acu.md) kan hjälpa dig att jämföra beräkning prestanda över Azure SKU: er.

## <a name="next-steps"></a>Nästa steg

Läs mer om hello olika storlekar på VM som är tillgängliga:
- [Generellt syfte](sizes-general.md)
- [Beräkningsoptimerad](sizes-compute.md)
- [Minnesoptimerad](../virtual-machines-windows-sizes-memory.md)
- [Lagringsoptimerad](../virtual-machines-windows-sizes-storage.md)
- [GPU-optimerad](sizes-gpu.md)
- [Databehandling med höga prestanda](sizes-hpc.md)



