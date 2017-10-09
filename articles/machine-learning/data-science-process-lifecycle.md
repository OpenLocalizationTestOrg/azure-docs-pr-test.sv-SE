---
title: aaaAzure Team datavetenskap processen livscykel | Microsoft Docs
description: "Steg behövs tooexecute datavetenskap projekt."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: b1f677bb-eef5-4acb-9b3b-8a5819fb0e78
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: bradsev;
ms.openlocfilehash: 58114a1c2d3289d1c4b2781219d0bf9647dbccd1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="team-data-science-process-lifecycle"></a>Team datavetenskap Process livscykel

hello Team Data vetenskap processen (TDSP) innehåller en rekommenderad livscykel som du kan använda toostructure datavetenskap projekt. hello livscykel beskrivs hello steg från start toofinish som projekt följer vanligtvis när de utförs. Om du använder en annan datavetenskap livscykel, exempelvis [SKARPA DM](https://wikipedia.org/wiki/Cross_Industry_Standard_Process_for_Data_Mining), [KDD](https://wikipedia.org/wiki/Data_mining#Process) eller din organisations egna anpassade process, kan du fortfarande använda hello uppgiftsbaserade TDSP med dessa development livscykler. 

Den här livscykeln har utformats för datavetenskap projekt som är avsedda tooship som en del av intelligent program. Dessa program distribuera machine learning eller artificiell intelligence modeller för förutsägelseanalys. Undersökande datavetenskap projekt och ad hoc-analytics projekt kan också dra nytta av den här processen. Men i sådana fall vissa stegen kanske inte behövs.    

Här är en bild av hello **Team datavetenskap Process livscykel**. 

![TDSP livscykel](./media/data-science-process-overview/tdsp-lifecycle.png) 

Hej TDSP livscykel består av fem viktiga steg som utförs upprepade gånger. Exempel på dessa är:

* **Så här fungerar för företag**
* **Datainsamling och förstå**
* **Modeling**
* **Distribution**
* **Kundens godkännande**

Vi tillhandahåller hello följande information för varje steg:

* **Mål**: hello specifika mål.
* **Hur toodo den**: hello specifika uppgifter beskrivs och riktlinjer som tillhandahålls slutfört dem.
* **Artefakter**: hello leveranser och hello stöd för att skapa dem.


## <a name="1-business-understanding"></a>1. Förståelse för verksamheten

### <a name="goals"></a>Mål
* hello **nyckeln variabler** har angetts som är tooserve som hello **modell mål** och vars relaterade mått som används avgör hello slutförd för hello projektet.
* hello relevanta **datakällor** identifieras att hello företag har åtkomst tooor behov tooobtain.

### <a name="how-toodo-it"></a>Hur toodo den
Det finns två huvudsakliga uppgifter som beskrivs i det här steget: 

* **Definiera mål**: arbeta med kunden och andra berörda parter toounderstand och identifiera hello affärsproblem. Formulera frågor som definierar hello affärsmål och att datavetenskap tekniker kan använda som mål.
* **Identifiera datakällor**: hitta hello relevanta data som hjälper dig att besvara frågor från hello som definierar hello målen för hello-projekt.

#### <a name="11-define-objectives"></a>1.1 definiera mål

1. En central syftet med det här steget är tooidentify hello nyckeln **business variabler** att hello analys måste toopredict. Dessa variabler är refererad tooas hello **modell mål** och hello mätvärden som är kopplade till dem används toodetermine hello lyckats hello-projekt. Två exempel på sådana mål är försäljning prognos eller hello sannolikheten för en order som falska.

2. Definiera hello **projektet mål** genom att fråga och förfina ”skarpa” frågor som är relevanta och specifika och entydigt. Datavetenskap är hello process med hjälp av namn och siffror tooanswer frågor. Mer information om att ställa frågor skarpa finns [hur toodo datavetenskap](https://blogs.technet.microsoft.com/machinelearning/2016/03/28/how-to-do-data-science/) blogg. Datavetenskap / machine learning är brukar användas tooanswer fem typer av frågor:
 
   * Hur mycket eller hur många? (regression)
   * Vilken kategori? (klassificering)
   * Vilken grupp? (klustring)
   * Är detta underligt? (avvikelseidentifiering)
   * Vilket alternativ som ska utföras? (rekommendationer)

    Avgöra vilka frågor du frågar och hur svarande det ger affärsmålen.

3. Definiera hello **projektet team** genom att ange hello roller och ansvarsområden för dess medlemmar. Utveckla en plan för övergripande milstolpe som du itererar som identifieras mer information.  

4. **Definiera framgångsmått**. Till exempel: uppnå kunden omsättning prognosens noggrannhet kan förbättras av X % hello slutet av det här 3-månaders-projektet så att vi kan erbjuda kampanjer tooreduce omsättning. hello mått måste vara **SMART**: 
   * **S**pecifika 
   * **M**easurable
   * **En**chievable 
   * **R**elevant 
   * **T**ime-bunden 

#### <a name="12-identify-data-sources"></a>1.2 identifiera datakällor
Identifiera datakällor som innehåller kända exempel svar tooyour skarpa frågor. Leta efter hello följande data:

* Data som är **de** toohello fråga. Har vi åtgärder hello mål och funktioner som är relaterade toohello mål?
* Data som är en **korrekt mått** av vår modell mål och hello funktionerna i intresse.

Det är inte ovanligt, till exempel toofind att befintliga system måste toocollect och logga ytterligare typer av data tooaddress hello problem och uppnå hello projektmål. I det här fallet om du vill använda toolook för externa datakällor eller uppdatera dina system toocollect nya data.

### <a name="artifacts"></a>Artefakter
Här följer hello produkterna i det här steget:

* [**Befrakta dokumentet**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Project/Charter.md): en standardmall tillhandahålls i hello TDSP projektet struktur definition. Detta är en levande dokument som uppdateras under hello projekt som nya identifieringar görs och som företagets behov förändras. hello nyckeln är tooiterate vid det här dokumentet, lägga till fler detaljer slutföra hello identifieringsprocessen. Behåll hello kundens och andra intressenters inblandad i ändrar hello och tydligt kommunicera hello orsaker till hello ändringar toothem.  
* [**Datakällor**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/Data%20Defintion.md#raw-data-sources): Detta är hello **Raw datakällor** avsnitt i hello **datadefinitioner** rapporten som hittas i hello TDSP projekt **Data rapporten**  mapp. Det anger hello ursprungliga och målplatser för hello rådata. I senare steg kan du fylla i ytterligare information som skript toomove hello data tooyour analytiska miljö.  
* [**Data ordlistor**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/tree/master/Docs/DataDictionaries): det här dokumentet innehåller beskrivningar av hello data som tillhandahålls av hello-klienten. Dessa beskrivningar innehåller information om hello schemat (datatyper, information om valideringsregler, om sådana finns) och hello entitet relationen diagram om det är tillgängligt.


## <a name="2-data-acquisition-and-understanding"></a>2. Förvärv och förståelse av data

### <a name="goals"></a>Mål
* En ren, högkvalitativ dataset förstås vars relationer toohello target-variabler som finns i hello lämpliga analytics miljö, redo toomodel.
* En lösningsarkitektur hello data pipeline toorefresh och poäng data regelbundet har utvecklats för.

### <a name="how-toodo-it"></a>Hur toodo den
Det finns tre huvudsakliga uppgifter som beskrivs i det här steget:

* **Mata in data för hello** till hello analytiska målmiljön.
* **Utforska data hello** toodetermine om hello DQS är tillräckligt tooanswer hello fråga. 
* **Konfigurera en data-pipeline** tooscore nya eller regelbundet uppdatera data.

#### <a name="21-ingest-hello-data"></a>2.1 mata in hello data
Ställ in hello processen toomove hello data från källplatser toohello platser där analytics-åtgärder som utbildning och förutsägelser toobe utförs. Teknisk information och alternativ för hur toodo detta med olika Azure data services, se [läser in data i miljöer med lagring för analys av](machine-learning-data-science-ingest-data.md). 

#### <a name="22-explore-hello-data"></a>2.2 utforska hello data
Innan du träna modeller måste toodevelop en god förståelse av hello data. Verkliga datauppsättningar är ofta mycket brus eller saknar värden eller ha en värd om andra skillnader. Sammanfatta data och visualisering kan använda tooaudit hello kvaliteten på dina data och ange hello information behövs tooprocess hello data innan den är klar för modellering. Den här processen är ofta iterativ.

TDSP ger en automatiserad verktyget [IDEAR](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/DataReport-Utils) toohelp visualisera hello data och förbereda data sammanfattningsrapporter. Vi rekommenderar att börja med IDEAR första tooexplore hello data toohelp utveckla ursprungliga data förstå interaktivt med ingen kodning och sedan skriva anpassad kod för datagranskning och visualisering. Information om rensning hello finns [uppgifter tooprepare data för förbättrad machine learning](machine-learning-data-science-prepare-data.md).  

När du är nöjd med hello kvaliteten hello rengöras data hello nästa steg är toobetter förstå hello mönster som ingår i hello data som hjälper dig välja och utveckla en lämplig förutsägelsemodell för dina mål. Leta efter bevis för hur väl anslutna hello data är toohello mål och om det finns tillräckliga data toomove framåt med hello modellering du går vidare. Den här processen är igen, ofta iterativ. Du kan behöva toofind nya datakällor med exaktare eller flera relevanta data tooaugment hello datamängd från början identifierats i hello föregående steg.  

#### <a name="23-set-up-a-data-pipeline"></a>2.3 Konfigurera en data-pipeline
Dessutom toohello inledande införandet och rensa hello data måste du vanligtvis tooset konfigurerar en process tooscore nya data eller uppdatera hello data regelbundet som en del av en pågående learning process. Detta kan göras genom att skapa en pipeline för data eller ett arbetsflöde. Här är en [exempel](machine-learning-data-science-move-sql-azure-adf.md) på hur tooset upp en pipeline med [Azure Data Factory](https://azure.microsoft.com/services/data-factory/). 

En lösningsarkitektur för hello data pipeline utvecklas i det här steget. hello pipeline också utvecklas parallellt med hello följa stegen i hello datavetenskap projektet. hello-pipeline kan vara batch-baserade eller direktuppspelning/real-gång eller en hybrid beroende på ditt företag måste och hello begränsningarna i din befintliga system som den här lösningen integreras. 

### <a name="artifacts"></a>Artefakter
hello följande är hello produkterna i det här steget.

* [**Data Quality rapporten**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/DataSummaryReport.md): den här rapporten innehåller data sammanfattningar, relationer mellan varje attribut och mål, variabel rangordning etc. hello [IDEAR](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/DataReport-Utils) verktyg som tillhandahålls som en del av TDSP kan snabbt skapa den här rapporten för alla tabular datauppsättningen som en CSV-fil eller en relationella tabell. 
* **Lösningsarkitektur**: Detta kan vara ett diagram eller en beskrivning av dina data pipeline används toorun bedömningen eller förutsägelser för nya data när du har skapat en modell. Den innehåller också hello pipeline tooretrain modellen baserat på nya data. hello dokumentet sparas i hello [projekt](https://github.com/Azure/Azure-TDSP-ProjectTemplate/tree/master/Docs/Project) directory när du använder hello TDSP directory struktur mall.
* **Kontrollpunkt beslut**: innan du börjar fullständig funktionen tekniker och modellskapandet omvärdera hello projektet toodetermine om hello förväntade värdet är tillräckligt toocontinue arbetar den. Du kan till exempel vara klar tooproceed, måste toocollect mer data, eller Avbryt hello projekt som hello data inte finns tooanswer hello fråga.


## <a name="3-modeling"></a>3. Modellering

### <a name="goals"></a>Mål
* Optimal datafunktioner för hello maskininlärningsmodell.
* En informativ ML-modell som beräknar hello mål bäst.
* En ML-modell som passar för produktion.

### <a name="how-toodo-it"></a>Hur toodo den
Det finns tre huvudsakliga uppgifter som beskrivs i det här steget:

* **Egenskapsval**: skapa datafunktioner från hello rådata toofacilitate modellen utbildning.
* **Modellen utbildning**: hitta hello modell svar hello frågan bäst genom att jämföra deras framgångsmått.
* Om din modell är **lämpar sig för produktion**.

#### <a name="31-feature-engineering"></a>3.1 egenskapsval
Funktionen tekniker omfattar inkludering, sammanställning och omvandling av rådata variabler toocreate hello funktioner som används i hello analys. Om du vill inblick i vad som aktiverar en modell, måste toounderstand hur funktionerna är andra relaterade tooeach och hello maskininlärningsalgoritmer är toouse dessa funktioner. Det här steget kräver en kreativa kombination av domän kunskaper och insikter från hello utforskning steget. Det här är en balansgång för att söka efter och informativa variabler, inklusive samtidigt för många orelaterade variabler. Informativa variabler förbättra våra resultat. orelaterade variabler introducera onödiga störningar i hello modellen. Du måste också toogenerate funktionerna för alla nya data som hämtades när den bedömningen. Hello generering av dessa funktioner kan därför endast beror på data som är tillgängliga när hello bedömningen. Teknisk information om funktionen tekniker när du använder olika tekniker för Azure data finns [Egenskapsval i hello datavetenskap Process](machine-learning-data-science-create-features.md). 

#### <a name="32-model-training"></a>3.2 träning
Beroende på typ av frågan som du försöker besvara finns det många modellering algoritmer. Anvisningar om hur du väljer hello algoritmer finns [hur toochoose algoritmer för Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md). Även om den här artikeln är avsedd för Azure Machine Learning hello vägledning för det här är användbart för alla projekt för maskininlärning. 

hello process för modellen innehåller hello följande steg: 

* **Dela hello indata** slumpmässigt för modellering i en datauppsättning för träning och en test-datamängd.
* **Bygga modeller hello** med hello datauppsättning för träning.
* **Utvärdera** (träning och testning dataset) en serie konkurrerande maskininlärningsalgoritmer tillsammans med hello olika associerade prestandajustering parametrar (kallas även parametern Svep) som är riktade mot svara hello fråga intressanta med hello aktuella data.
* **Fastställa hello ”bästa” lösningen** tooanswer hello frågan genom att jämföra hello lyckade mått mellan alternativa metoder.

> [!NOTE]
> **Undvika läckage av**: läckage av Data kan orsakas av hello inkludering av data från utanför hello utbildning datamängd som tillåter en modell eller maskininlärningsalgoritmen toomake orealistiskt bra förutsägelser. Läckage är en vanlig orsak till varför datavetare få vad när de får förutsägande resultat som verkar vara för bra toobe SANT. Dessa beroenden kan vara svårt toodetect. tooavoid läckage kräver ofta iterera mellan bygga en datauppsättning för analys, skapa en modell och utvärdera hello noggrannhet. 
> 
> 

Vi tillhandahåller en [automatisk modellering och rapportering verktyget](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/Modeling) med TDSP som kan toorun via flera algoritmer och parametern sveps tooproduce en baslinje-modell. Den ger också en baslinje för modellering rapporten sammanfattar prestanda för varje modell och parameterkombinationen inklusive variabeln vikten. Den här processen är också iterativ som den kan enheten ytterligare funktionen tekniker. 

### <a name="artifacts"></a>Artefakter
hello artefakter som skapas i det här steget inkluderar:

* [**Egenskapsuppsättningar**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/Data%20Defintion.md#feature-sets): hello utvecklats för hello modellering beskrivs hello **funktionsuppsättningarna** avsnitt i hello **datadefinitionen** rapporten. Den innehåller pekare toohello kod toogenerate hello funktioner och en beskrivning på hur hello-funktionen har genererats.
* [**Modellen rapporten**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Model/Model%201/Model%20Report.md): mall-baserad rapport som innehåller information om varje experiment skapas för varje modell som testas, en standard.
* **Kontrollpunkt beslut**: utvärdera om hello modellen presterar bra tillräckligt med toodeploy den tooa produktionssystem. Vissa viktiga frågor tooask är:
  * Hello modellen fråga hello med tillräckligt angetts hello testdata? 
  * Borde testa alla alternativa metoder: samla in ytterligare data, gör mer funktionen tekniker eller experimentera med andra algoritmer?


## <a name="4-deployment"></a>4. Distribution

### <a name="goal"></a>Målet
* Modeller med en rörledning för data är distribuerade tooa produktion eller produktions-liknande miljö för godkännande av slutanvändaren. 

### <a name="how-toodo-it"></a>Hur toodo den
hello huvudsakliga uppgift beskrivs i det här steget:

* **Operationalisera hello modellen**: distribuera hello modellen och pipeline tooa eller produktions-liknande miljö för användning av programmet.

#### <a name="41-operationalize-a-model"></a>4.1 operationalisera en modell
När du har en uppsättning modeller som utför också kan du operationalized dem för andra program tooconsume. Beroende på hello affärsbehov görs förutsägelser i realtid eller på grundval av batch. toobe operationalized, hello modeller har toobe visas med ett öppet API-gränssnitt som är enkelt används från olika program, till exempel online webbplatser, kalkylblad, instrumentpaneler och affärs- och backend-program för. Exempel på modellen operationalization med en Azure Machine Learning-webbtjänst finns [distribuera en Azure Machine Learning-webbtjänst](machine-learning-publish-a-machine-learning-web-service.md). Det är också en bästa praxis toobuild telemetri och övervakningen i hello produktion modellen och hello data pipeline distribueras toohelp med efterföljande systemstatus rapportering och felsökning.  

### <a name="artifacts"></a>Artefakter
* Status för instrumentpanelen i systemet hälsa och nyckeln mått.
* Sista modellering rapporten med detaljer distribution.
* Slutlig lösning arkitektur dokument.

## <a name="5-customer-acceptance"></a>5. Kundgodkännande

### <a name="goal"></a>Målet
* **Slutför hello projektet leveranser**: Kontrollera att hello pipeline, hello modell och deras distribution i en produktionsmiljö är uppfyller kundens mål.

### <a name="how-toodo-it"></a>Hur toodo den
Det finns två huvudsakliga uppgifter som beskrivs i det här steget:

* **Systemvalidering**: Bekräfta hello distribueras modell och pipeline uppfyller kundens behov.
* **Projektet hand av**: toohello entitet som är toorun hello system i produktion.

hello kunden bör verifiera att hello systemet uppfyller sina affärsbehov och hello svar hello frågor med acceptabel Precision toodeploy hello system tooproduction för användning av sina klientprogram. All hello dokumentation är slutförd och granskas. En hand av av hello projektet toohello entitet som ansvarar för åtgärder har slutförts. Den här entiteten kan exempelvis vara en IT- eller kunden datavetenskap team eller en agent hello kunden som ansvarar för att köra hello system i produktion. 

### <a name="artifacts"></a>Artefakter
hello huvudsakliga artefakt som skapas i det här sista steget är hello **avsluta rapport av projekt för kunden**. Detta är hello tekniska rapport som innehåller alla detaljer i hello projekt som användbara toolearn om och fungerar hello system. En [avsluta rapporten](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Project/Exit%20Report.md) mall tillhandahålls av TDSP som kan användas som de är eller anpassas för specifika klienten behöver. 

## <a name="summary"></a>Sammanfattning
Hej [Team datavetenskap Process livscykel](http://aka.ms/datascienceprocess) modelleras efter en rad olika hävdade steg som ger riktlinjer för hello uppgifter behov toouse förutsägelsemodeller. Dessa modeller kan distribueras i en miljö toobe utnyttjas toobuild intelligent program i produktion. hello syftet med den här processen livscykeln är toocontinue toomove datavetenskap projektet framåt mot en tydlig engagement slutpunkt. När det gäller att datavetenskap är en uppgift i forskning och identifiering, som kan tooclearly kommunicera dessa uppgifter tooyour team och dina kunder med en väl definierad uppsättning artefakter som anställda standardiserade mallar kan hjälpa dig att undvika missförstånd och öka hello risken för slutförd komplexa datavetenskap projektet.

## <a name="next-steps"></a>Nästa steg
Fullständig genomgång för slutpunkt-till-slutpunkt som visar alla hello steg i hello process för **specifika scenarier** tillhandahålls också. De anges och är kopplad till miniatyr beskrivningar i hello [Team datavetenskap Process genomgång](data-science-process-walkthroughs.md) avsnittet.

