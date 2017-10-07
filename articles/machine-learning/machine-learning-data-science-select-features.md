---
title: aaaFeature val i hello Team datavetenskap Process | Microsoft Docs
description: "Beskriver hello syftet val av funktioner och innehåller exempel på deras roll i hello förbättring av data för machine learning."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 878541f5-1df8-4368-889a-ced6852aba47
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: zhangya;bradsev
ms.openlocfilehash: 54af93c83e4cc6a3670b3ad62490e0f74082b4ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="feature-selection-in-hello-team-data-science-process-tdsp"></a>Val av funktioner i hello Team Data vetenskap processen (TDSP)
Den här artikeln beskriver hello tillämpningen av val av funktioner och innehåller exempel på sin roll i hello förbättring av data för machine learning. Dessa exempel hämtas från Azure Machine Learning Studio. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Hej tekniker och funktioner är en del av hello Team Data vetenskap processen (TDSP) beskrivs i [vad är hello Team av vetenskapliga data?](data-science-process-overview.md). Funktionen tekniker och markeringen är delar av hello **utveckla funktioner** steg i hello TDSP.

* **egenskapsval**: den här processen försöker toocreate ytterligare relevanta funktioner från hello befintliga raw-funktioner i hello data och tooincrease förutsägande power toohello Inlärningsalgoritmen.
* **funktionen val**: den här processen väljer hello viktiga delmängd av den ursprungliga datafunktioner i ett försök tooreduce hello dimensionalitet hello utbildning problemet.

Normalt **egenskapsval** är tillämpade första toogenerate ytterligare funktioner och hello **funktion markeringen** steget är utförs tooeliminate irrelevanta, redundant eller hög korrelerade funktioner.

## <a name="filtering-features-from-your-data---feature-selection"></a>Filtrera funktioner från dina Data - val av funktioner
Val av funktioner är en process som används ofta för hello konstruktion av utbildning datamängder för förutsägelsemodellering aktiviteter, till exempel klassificerings- eller regressionsmodell uppgifter. hello målet är tooselect en delmängd av hello funktioner från hello ursprungliga datauppsättningen som minskar dess storlek genom att använda en minimal uppsättning funktioner toorepresent hello maximal mängd varians i hello data. Den här delmängd av funktionerna är sedan hello endast funktioner toobe ingår tootrain hello modellen. Val av funktioner har två huvudsakliga syften.

* Först Funktionsurval ökar ofta klassificering noggrannhet genom att ta bort irrelevanta, redundanta eller hög korrelerade funktioner.
* Dessutom minskas hello antal funktioner som gör modellen utbildning processen mer effektiv. Detta är särskilt viktigt för inlärning är dyr tootrain som support vector datorer.

Även om Funktionsurval avser tooreduce hello antal funktioner i hello dataset används tootrain hello modellen, är det inte vanligtvis enligt tooby hello termen ”dimensionalitet minskning”. Funktionen val metoder extrahera en delmängd av ursprungliga funktioner i hello data utan att ändra dem.  Metoder för dimensionalitet minskning av bakåtkompilerade funktioner som kan omvandla hello ursprungliga funktioner och därmed ändra dem. Exempel på dimensionalitet minskning metoder är Principal komponenten analys, kanoniska korrelation analys och enkel värdet uppdelning.

Bland annat kallas en allmänt tillämpade kategori av funktionen markeringsmetoder i en övervakad kontext ”filter baserat Funktionsurval”. Genom att utvärdera hello korrelation mellan varje funktion och hello målattribut gäller metoderna statistiskt mått-tooassign en poäng tooeach funktion. hello funktioner rangordnas sedan hello poäng, vilket kan vara används toohelp angiven hello tröskel för att hålla eller ta bort en specifik funktion. Exempel på hello statistiskt mått som används i dessa metoder är Person korrelation ömsesidig information och hello Chi kvadraten test.

Det finns moduler som angetts för val av funktioner i Azure Machine Learning Studio. I följande bild hello visas dessa moduler innehåller [Filter-baserade Funktionsurval] [ filter-based-feature-selection] och [Fisher linjär Discriminant analys] [ fisher-linear-discriminant-analysis].

![Funktionen val exempel](./media/machine-learning-data-science-select-features/feature-Selection.png)

Tänk till exempel hello av hello [Filter-baserade Funktionsurval] [ filter-based-feature-selection] modul. I hello syftet att underlätta för dig fortsätta vi toouse hello datautvinning textexempel ovan. Anta att vi vill toobuild en regressionsmodell efter en uppsättning 256 funktioner har skapats via hello [hash-funktionen] [ feature-hashing] modulen och hello svar variabeln är hello ”Kol1” och representerar en bok Granska mellan 1 too5. Genom att ange ”funktionen bedömningen metoden” toobe ”kvadratvärdet korrelation” hello ”målkolumnen” toobe ”Kol1” och hello ”antal önskade funktioner” too50. Sedan hello modulen [Filter-baserade Funktionsurval] [ filter-based-feature-selection] genererar en datamängd som innehåller 50 funktioner tillsammans med hello målattribut ”Kol1”. hello följande bild visar hello flödet av experimentet och hello indataparametrar vi bara beskrivs.

![Funktionen val exempel](./media/machine-learning-data-science-select-features/feature-Selection1.png)

hello visar följande bild hello resulterande datauppsättningar. Varje funktion beräknas baserat på hello kvadratvärdet korrelation mellan sig själv och hello målattribut ”Kol1”. hello funktioner med högsta poäng behålls.

![Funktionen val exempel](./media/machine-learning-data-science-select-features/feature-Selection2.png)

hello motsvarande resultat av hello valda funktioner som visas i hello följande bild.

![Funktionen val exempel](./media/machine-learning-data-science-select-features/feature-Selection3.png)

Genom att använda detta [Filter-baserade Funktionsurval] [ filter-based-feature-selection] modulen, 50 av 256 funktioner är markerade eftersom de har hello mest korrelerade funktioner med hello målvariabel ”Kol1” baserat på hello bedömningen metoden ”kvadratvärdet korrelation”.

## <a name="conclusion"></a>Slutsats
Funktionen tekniker och Funktionsurval är två ofta utformad och de valda funktionerna öka hello effektiviteten för hello utbildning process som försöker tooextract hello nyckelinformation i hello data. De även förbättra hello kraften i indata dessa modeller tooclassify hello korrekt och toopredict resultat för intresse mer robustly. Funktionen tekniker och val kan också kombinera toomake hello learning mer beräkningsmässigt tractable. Den gör detta genom att öka och minska hello antal funktioner behövs toocalibrate eller tåg en modell. Matematiskt tala hello funktioner valda tootrain hello modellen är en minimal uppsättning oberoende variabler som förklarar hello mönster i hello data och förutsäga resultat har.

Observera att det inte alltid är tooperform teknik eller funktion val av funktioner. Om det behövs eller inte beror på hello data som vi har eller samla in hello algoritmen vi väljer och hello syftet med hello experiment.

<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[fisher-linear-discriminant-analysis]: https://msdn.microsoft.com/library/azure/dcaab0b2-59ca-4bec-bb66-79fd23540080/

