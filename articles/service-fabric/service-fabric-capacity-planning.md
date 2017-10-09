---
title: "aaaCapacity planering för Service Fabric-appar | Microsoft Docs"
description: "Beskriver hur tooidentify hello antalet compute-noder som krävs för ett Service Fabric-program"
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: markfuss
editor: 
ms.assetid: 9fa47be0-50a2-4a51-84a5-20992af94bea
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 44a69e9d8ec5efcc43122dc42e8f923ef37378f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="capacity-planning-for-service-fabric-applications"></a>Kapacitetsplanering för Service Fabric-program
Det här dokumentet lär du dig hur tooestimate hello mycket resurser (processorer, minne, disk och lagring) måste toorun Azure Service Fabric-program. Det är vanligt för din resurs krav toochange över tid. Du kräver vanligtvis några resurser som du utvecklar och testning i tjänsten och sedan kräver mer resurser som du gå till produktion och programmet växer i större utsträckning. När du skapar ditt program kan tänka igenom hello långsiktiga kraven och fatta beslut som gör att din tjänst tooscale toomeet hög kundernas behov.

 När du skapar ett Service Fabric-kluster kan bestämma du vilka typer av virtuella datorer (VM) utgör hello klustret. Varje virtuell dator har en begränsad mängd resurser i hello form av CPU (kärnor och hastighet), nätverkets bandbredd, RAM-minne och diskutrymme. När tjänsten växer med tiden kan uppgradera du tooVMs som erbjuder större resurser och/eller lägga till fler virtuella datorer tooyour klustret. toodo hello senare, du måste skapa din tjänst först så att den kan dra nytta av nya virtuella datorer som hämta läggs toohello klustret dynamiskt.

Vissa tjänster hantera lite toono data på hello själva de virtuella datorerna. Kapacitetsplanering för dessa tjänster bör fokuserar huvudsakligen på prestanda, vilket innebär att välja hello lämpliga därför CPU (kärnor och hastighet) hello virtuella datorer. Dessutom bör du bandbredd i nätverket, inklusive hur ofta nätverket överföringar sker och hur mycket data överförs. Om din tjänst måste tooperform såväl som tjänsten Minnesanvändningen ökar, kan du lägga till fler virtuella datorer toohello klustret och läsa in saldo hello nätverksbegäranden över alla hello virtuella datorer.

För tjänster som hanterar stora mängder data på hello virtuella datorer, bör kapacitetsplanering fokuserar främst på storleken. Därför bör du noga överväga hello lagringskapacitet enligt hello VM RAM-minne och diskutrymme. hello virtuellt minne management system i Windows gör diskutrymme som ser ut som RAM tooapplication kod. Dessutom ger hello Service Fabric runtime smart växling behålla endast varm data i minnet och flytta hello kalldata toodisk. Program kan därmed använda mer minne än vad som är fysiskt tillgängligt på hello VM. Med mer RAM-minne bara ökar prestanda, eftersom hello VM kan ha flera disklagring i RAM-minne. hello VM som du väljer ska ha en tillräckligt stor toostore hello diskdata som du vill använda på hello VM. På liknande sätt ska hello VM ha tillräckligt med RAM-minne tooprovide du med hello prestanda som du önskar. Din tjänst data växer över tid, kan du lägga till fler virtuella datorer toohello klustret och partition hello data över alla hello virtuella datorer.

## <a name="determine-how-many-nodes-you-need"></a>Fastställa hur många noder du behöver
Partitionering din tjänst kan du tooscale ut din tjänst data. Mer information om partitionering finns [partitionering Service Fabric](service-fabric-concepts-partitioning.md). Varje partition måste få plats i en enda virtuell dator, men flera (små) partitioner kan placeras på en enda virtuell dator. Därför ger har flera mindre partitioner dig större flexibilitet än att ha några större partitioner. hello kompromiss är att ha många partitioner ökar Service Fabric kostnader och du kan inte utföra överförda åtgärder mellan partitioner. Det finns också flera potentiella nätverkstrafik om koden för tjänsten behöver ofta tooaccess datadelar som bor i olika partitioner. När du utformar din tjänst bör du noggrant dessa- och nackdelar tooarrive på en effektiv partitionering strategi.

Anta att programmet har en enda tillståndskänslig tjänst som har en store-storlek som du förväntar dig toogrow tooDB_Size GB i ett år. Du är beredd tooadd fler program (och partitioner) som uppstår tillväxt utöver det aktuella året.  hello replikering faktor (RF) som avgör hello antalet repliker för din tjänst påverkar hello totala DB_Size. hello är totala DB_Size över alla repliker hello replikering faktor multiplicerat med DB_Size.  Node_Size representerar hello disk space/RAM-minne per nod önskade toouse för din tjänst. För bästa prestanda bör hello DB_Size ryms i minnet i hello klustret, och en Node_Size är runt hello RAM för hello VM bör väljas. Genom att allokera en Node_Size som är större än hello RAM kapacitet måste hello sidindelning som tillhandahålls av hello Service Fabric runtime. Prestandan kanske därför inte är optimalt om din hela data betraktas som toobe varm (sedan sedan hello data är växlingsbart systemminne in/ut). För många tjänster där en bråkdel av hello data är hot, är det dock mer kostnadseffektivt.

hello antalet noder som krävs för maximala prestanda kan beräknas enligt följande:

```
Number of Nodes = (DB_Size * RF)/Node_Size

```


## <a name="account-for-growth"></a>Konto för tillväxt
Du kanske vill toocompute hello antalet noder baserat på hello DB_Size som du förväntar dig din tjänst toogrow till, förutom toohello DB_Size som du började med. Utöka sedan hello antalet noder i takt med din tjänst växer så att du inte överbelasta hello antalet noder. Men hello antalet partitioner ska baseras på hello antalet noder som behövs när du kör din tjänst på högsta tillväxt.

Det är bra toohave vissa extra datorer som är tillgängliga när som helst så att du kan hantera alla oväntat toppar eller inte (till exempel om några virtuella datorer gå).  Hello extra kapacitet ska fastställas med hjälp av din förväntade toppar, utgångspunkt är tooreserve några extra VM: ar (5-10 procent extra).

hello ovan förutsätter att en enda tillståndskänslig tjänst. Om du har mer än en tillståndskänslig service har tooadd hello DB_Size som är associerade med hello andra tjänster i hello formel. Alternativt kan du beräkna hello antalet noder separat för varje tillståndskänslig service.  Tjänsten kan ha repliker eller partitioner som inte är Balanserat. Tänk på att partitioner kan också ha fler data än andra. Mer information om partitionering finns [partitionering artikel om bästa praxis](service-fabric-concepts-partitioning.md). Hello föregående formeln är dock partition och repliken storleksoberoende eftersom Service Fabric säkerställer att hello repliker sprids bland hello noderna på ett optimerat sätt.

## <a name="use-a-spreadsheet-for-cost-calculation"></a>Använda ett kalkylblad för beräkning
Nu vi lägger reellt tal i hello formel. En [exempel kalkylblad](https://servicefabricsdkstorage.blob.core.windows.net/publicrelease/SF%20VM%20Cost%20calculator-NEW.xlsx) visar hur tooplan hello kapacitet för ett program som innehåller tre typer av dataobjekt. Vi ungefär storleken och hur många objekt som vi räknar med toohave för varje objekt. Vi kan också välja hur många repliker som vi vill för varje objekttyp. hello beräknas hello totala mängden minne toobe lagras i hello kluster.

Vi ange sedan ett VM-storlek och månadskostnaden. Baserat på hello VM-storlek, anger hello kalkylblad du hello minsta antalet partitioner måste du använda toosplit som passar dina data toophysically på hello noder. Du kanske vill ha ett större antal partitioner tooaccommodate ditt programs specifika beräknings- och trafikbehov. hello kalkylblad visar hello antalet partitioner som hanterar hello användarobjekt profil har ökat från en toosix.

Baserat på den här informationen hello kalkylbladet innehåller nu att du kan få alla hello data med hello önskad partitioner och repliker fysiskt på ett kluster med noder 26. Men skulle det här klustret tätt packas, så du kan vissa ytterligare noder tooaccommodate nodfel och uppgraderingar. hello kalkylblad visar också att med mer än 57 noder ger inga ytterligare värde eftersom du ha tomma noder. Igen, kanske du vill toogo ovan 57 noder ändå tooaccommodate nodfel och uppgraderingar. Du kan justera hello kalkylblad toomatch ditt programs specifika behov.   

![Kalkylblad för beräkningen][Image1]

## <a name="next-steps"></a>Nästa steg
Checka ut [partitionering Service Fabric services] [ 10] toolearn mer om partitionering din tjänst.

<!--Image references-->
[Image1]: ./media/SF-Cost.png

<!--Link references--In actual articles, you only need a single period before hello slash-->
[10]: service-fabric-concepts-partitioning.md
