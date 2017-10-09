---
title: "aaaWhat toodo hello ett avbrott i Azure-tjänsten påverkar virtuella Azure-nätverk för händelsen | Microsoft Docs"
description: "Lär dig vilka toodo i hello händelse av ett avbrott i Azure-tjänsten påverkar virtuella Azure-nätverk."
services: virtual-network
documentationcenter: 
author: NarayanAnnamalai
manager: jefco
editor: 
ms.assetid: ad260ab9-d873-43b3-8896-f9a1db9858a5
ms.service: virtual-network
ms.workload: virtual-network
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2016
ms.author: narayan;aglick
ms.openlocfilehash: db022d2a042d255cf8ec6afb68cd8436aeecfe08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="virtual-network--business-continuity"></a>Virtuellt nätverk – kontinuitet för företag
## <a name="overview"></a>Översikt
Ett virtuellt nätverk (VNet) är en logisk representation av nätverket i hello molnet. Det gör att du toodefine egna privata IP-adress utrymme och segment hello nätverk i undernät. Vnet som fungerar som ett förtroende gräns toohost dina beräkningsresurser som Azure virtuella datorer och molntjänster (web/worker-roller). Ett virtuellt nätverk kan direkt privata IP-kommunikation mellan hello resurser som finns i den. Ett virtuellt nätverk kan också vara länkade tooan lokalt nätverk via ett av hello hybrid alternativ, till exempel en VPN-Gateway eller ExpressRoute.

Ett virtuellt nätverk skapas inom hello omfånget för en region. Du kan skapa Vnet med samma adressutrymme i två olika regioner (d.v.s. oss Öst och oss Väst men det går inte att ansluta dem tooone en annan direkt). 

## <a name="business-continuity"></a>Verksamhetskontinuitet
Det kan finnas flera olika sätt att programmet kan avbrytas. En viss region kan vara helt beskärs förfallodatum tooa naturkatastrof eller en partiell katastrof på grund av tooa fel på flera enheter och tjänster. i alla dessa situationer skiljer hello inverkan på hello VNet-tjänsten.

**F: Vad gör i hello händelse av ett avbrott tooan hela region? dvs. Om en region är helt klara på grund av tooa naturkatastrof? Vad händer toohello virtuella nätverk finns i hello region?**

S: hello virtuella nätverk och hello resurser i hello påverkas region förblir oåtkomlig under hello tiden för hello avbrott i tjänsten.

![Enkel virtuella nätverksdiagram](./media/virtual-network-disaster-recovery-guidance/vnet.png)

**F: Vad kan jag toodo återskapa hello samma virtuella nätverk i en annan region?**

S: virtuella nätverk (VNet) är ganska enkel resurs. Du kan anropa Azure API: erna toocreate ett VNet med hello samma adressutrymmet i en annan region. toore-skapa hello samma miljö som fanns i hello påverkas region har du toomake API-anrop toore-distribuera dina molntjänster (web/worker-roller) och virtuella datorer som du hade. Du måste även ha toospin upp en VPN-Gateway och ansluta tooyour lokalt nätverk om du har lokala anslutningen (till exempel i en hybriddistribution).

hello anvisningar för att skapa ett virtuellt nätverk finns [här](virtual-networks-create-vnet-arm-pportal.md). 

**F: kan en replik av ett VNet i en viss region skapas på nytt i en annan region i förväg?**

S: Ja, kan du skapa två Vnet med hjälp av hello samma privata IP-adressutrymme och resurser i två olika regioner före tid. Om en kund värd för tjänster i hello VNet mot internet, kan de har ställt in toogeo Traffic Manager-väg trafik toohello region som är aktiv. Men en kund kan inte ansluta två Vnet med hello samma adress utrymme tootheir lokalt nätverk eftersom det skulle orsaka problem med routning. Hello tiden för en katastrofåterställning och förlust av ett VNet i en region, en kund kan ansluta hello andra VNet i hello tillgängliga region med matchande adressutrymme tootheir lokalt nätverk.

hello anvisningar för att skapa ett virtuellt nätverk finns [här](virtual-networks-create-vnet-arm-pportal.md).

