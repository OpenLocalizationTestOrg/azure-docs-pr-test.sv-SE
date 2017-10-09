---
title: "Förklarar beräkna enheter i Azure-databas för PostgreSQL | Microsoft Docs"
description: "Azure DB för PostgreSQL: den här artikeln förklarar hello begreppet Compute enheter och vad som händer när din arbetsbelastning når Hej max Compute-enheter."
services: postgresql
author: kamathsun
ms.author: sukamat
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 05/23/2017
ms.openlocfilehash: 42ec766ff6ce82ceee34d42e0f4a4ad86bc7b45d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="explaining-compute-units-in-azure-database-for-postgresql"></a>Förklarar beräkning enheter i Azure-databas för PostgreSQL
Den här artikeln förklarar hello begreppet Compute enheter och vad som händer när din arbetsbelastning når Hej max Compute-enheter.

## <a name="what-are-compute-units"></a>Vad är Compute enheter?
Compute-enheter som är ett mått på CPU bearbetning dataflödet som garanterat toobe tillgängliga tooa enkel Azure-databas för PostgreSQL-servern. En Compute-enhet är en blandning av processor-och minnesresurser. I allmänhet del 50 Compute enheter toohalf av en kärna. 100 enheter för compute del tooone kärnor. 2000 compute enheter del tootwenty kärnor garanterad bearbetning genomströmning tillgängliga tooyour Server.

Hej mängden minne per Compute-enhet är optimerad för hello Basic och Standard prisnivåer. Dubblerade hello Compute enheter genom att öka hello prestandanivå är lika toodoubling hello uppsättning resurser tillgängliga toothat enkel Azure-databas för PostgreSQL.

Ett Standard 800 Compute enheter innehåller till exempel 8 x mer CPU genomflöde och minne än en 100 Compute standardenheter konfiguration. Dock medan 100 Compute standardenheter tillhandahåller hello samma CPU genomflöde jämfört med tooBasic 100 Compute enheter, hello mängden minne som är förkonfigurerad i Standardprisnivån är dubbla hello mängden minne som har konfigurerats för grundläggande prisnivån. Därför ger Standardprisnivån bättre arbetsbelastningsprestanda och kortare transaktion svarstid än i grundläggande hello samma Compute enheter valt prisnivå.

## <a name="how-can-i-determine-hello-number-of-compute-units-needed-for-my-workload"></a>Hur vet hello antalet Compute enheter som behövs för min arbetsbelastning?
Om du behöver toomigrate en PostgreSQL-server som körs lokalt eller på en virtuell dator kan bestämma du hello antal Compute genom att uppskatta hur många kärnor bearbetning genomströmning din arbetsbelastning behöver. 

Om din befintliga lokala eller virtuella server använder för närvarande 4 kärnor (oräknade CPU hyperthread), börja med att konfigurera 400 Compute enheter för din Azure-databas för PostgreSQL-servern. Compute-enheter kan dynamiskt skalas upp eller ned beroende på dina behov för arbetsbelastning med nästan inga driftavbrott. 

Övervakningsdiagrammet hello mått i hello Azure-portalen eller skriva Azure CLI-kommandon - toomeasure beräkna enheter. Relevanta mått toomonitor är hello Compute enhet procent och beräkna enhet gränsen.

>[!IMPORTANT]
> Överväg att övervaka hello beräkning enheter användning samt om du hittar lagring IOPS inte är fullständigt utnyttjade toohello maximalt. Höja hello Compute-enheter kan ha högre i/o-genomflöde av inskränkning hello flaskhalsar på grund av toolimited CPU eller minne.

## <a name="what-happens-when-i-hit-my-maximum-compute-units"></a>Vad händer när jag träffar min max Compute-enheter?
Prestanda är kalibrera och regleras tooprovide resurser toorun din databas arbetsbelastning in toohello max gränser för hello valda prisnivån och prestandanivå. 

Om din arbetsbelastning når hello gränser i antingen hello Compute enheter eller etablerade IOPS-gränser, du kan fortsätta tooutilize hello resurser på hello högsta tillåtna nivå, men dina frågor är sannolikt toosee ökad latens. Dessa gränser kommer inte några fel, men i stället en nedgång i hello arbetsbelastning, om inte hello långsammare blir så allvarliga som frågar timeout. 

Om din arbetsbelastning når hello övre gräns för antalet anslutningar, aktiveras explicit fel. Mer information om resurser begränsar finns [begränsningar i Azure-databas för PostgreSQL](concepts-limits.md).

## <a name="next-steps"></a>Nästa steg
Mer information om prisnivåer finns [Azure-databas för PostgreSQL prisnivåer](./concepts-service-tiers.md).
