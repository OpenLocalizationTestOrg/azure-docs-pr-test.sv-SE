---
title: "aaaWhat är Team av vetenskapliga data?  | Microsoft Docs"
description: "hello Team av vetenskapliga data är en systematiskt metod för att skapa intelligent program som utnyttjar avancerade analyser."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: a098aa2e-fd79-4543-8e15-9aae9d8b3ee6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2017
ms.author: bradsev
ROBOTS: NOINDEX
redirect_url: data-science-process-overview
redirect_document_id: True
ms.openlocfilehash: 57187be9c884389c13c226eab74aff137f5514a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-hello-team-data-science-process-tdsp"></a>Vad är hello Team Data vetenskap processen (TDSP)?
Hej [Team Data vetenskap processen (TDSP)](data-science-process-overview.md) ger systematiskt toobuilding intelligent program som aktiverar team med data forskare toocollaborate effektivt över hello hela livscykeln för aktiviteter som krävs tooturn programmen i produkter. Hej TDSP beskrivs en rad olika steg som tillhandahåller **vägledning** på toodefine hello problem, konfigurera hello verktyg och miljö som behövs, analyserar hur relevanta data, skapa och utvärdera förutsägelsemodeller och sedan distribuera dessa modeller i företagsprogram. 

Här följer hello stegen i **Team datavetenskap Process**:  

![CAP-arbetsflöde](./media/machine-learning-data-science-the-cortana-analytics-process/CAP-workflow.png)

hello-processen är **iterativ**: hello förståelse för nya och befintliga eller förbättringar i hello modellen utvecklas och kräver omarbetning steg som tidigare har slutförts i hello sekvens. Befintliga organisationens utveckling och processer planering av projekt är **enkelt anpassats** toowork med hello TDSP definierats ordningsföljden. 

hello steg i processen för hello är i diagram och länkade i hello [TDSP Utbildningsväg](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) och beskrivs nedan.  

## <a name="preparation-steps"></a>Förberedelsesteg
## <a name="p1-plan-hello-analytics-project"></a>P1. Planera hello analytics projekt
Starta en analytics-projekt genom att definiera dina affärsmål och problem. De har angetts i **affärsbehov**. En central syftet med det här steget är tooidentify hello viktiga variabler (försäljning prognos eller hello sannolikheten för en ordning som bedrägliga, till exempel) som hello analys behov toopredict toosatisfy dessa krav. Ytterligare planering är vanligtvis väsentliga toodevelop förståelsen av hello **datakällor** behövs tooaddress hello målen för hello projekt från ett analytiska perspektiv. Det är inte ovanligt, till exempel toofind att befintliga system måste toocollect och logga ytterligare typer av data tooaddress hello problem och uppnå hello projektmål. Anvisningar finns [planera din miljö för hello Team datavetenskap Process](machine-learning-data-science-plan-your-environment.md) och [scenarier för avancerade analyser i Azure Machine Learning](machine-learning-data-science-plan-sample-scenarios.md).  

## <a name="p2-setup-analytics-environment"></a>P2. Konfigurera analysmiljö
En analytics miljö för hello Team datavetenskap processen omfattar flera komponenter: 

* **data arbetsytor** där mellanlagras hello data för analys och modellering 
* en **bearbetande infrastrukturen** för förbearbetning, utforska och modellerar hello data
* en **körningsinfrastrukturen** toooperationalize hello analytiska modeller och kör hello intelligent klientprogram som använder hello modeller.  

hello analytics infrastruktur som behöver toobe installationen ingår ofta i en miljö som är separat från core operativa system. Men den vanligtvis använder data från flera system inom hello företag samt från källor externa toohello företag. hello analytics infrastruktur kan vara endast molnbaserad eller en lokal installation eller en kombination av hello två. Alternativ, se [ställa in datavetenskap miljöer för användning i hello Team datavetenskap Process](machine-learning-data-science-environment-setup.md).

## <a name="analytics-steps"></a>Analytics steg:
## <a name="1-ingest-data-into-hello-analytical-environment"></a>1. Mata in Data i hello analytiska miljö
hello första steget är toobring hello relevanta data från olika källor, antingen från inom eller utanför hello enterprise, i en analytiska miljöer där hello data kan bearbetas. Hej **format** av hello data vid källan kan skilja sig från hello-format som krävs av hello mål. Så kan vissa data transformation också ha toobe som utförs av hello införandet verktygsuppsättning. Alternativ, se [läser in data i miljöer med lagring för analys](machine-learning-data-science-ingest-data.md)

Många intelligent program finns i tillägget toohello inledande införandet av data, krävs toorefresh hello data regelbundet som en del av en pågående learning process. Detta kan göras genom att ställa in en **data pipeline** eller ett arbetsflöde. Detta utgör en del av hello iterativ tillhör hello process som innehåller återskapa och omvärdera hello analytiska modeller som används av hello intelligent program distribuera hello-lösning. Visa, till exempel [flytta data från en lokal SQL server tooSQL Azure med Azure Data Factory](machine-learning-data-science-move-sql-azure-adf.md).

## <a name="2-explore-and-pre-process-data"></a>2. Utforska och förbehandla data
hello nästa steg är tooobtain en bättre förståelse av hello data genom att undersöka dess **sammanfattande statistik** , relationer, och genom att använda dessa **visualiseringen**. Det är också där utfärdar av **data quality** och integritet, till exempel värden som saknas, datatyper och inkonsekventa data relationer hanteras. Före bearbetning transformeringar är används tooclean in hello rådata innan ytterligare analytics och modellering kan äga rum. En beskrivning finns [uppgifter tooprepare data för förbättrad machine learning](machine-learning-data-science-prepare-data.md).

## <a name="3-develop-features"></a>3. Utveckla funktioner
Data forskare, tillsammans med domänen experter måste identifiera hello funktioner att avbilda hello viktigaste egenskaper för hello datauppsättning och som bäst kan vara används toopredict hello viktiga variabler vid planering. Dessa funktioner kan härledas från befintliga data eller kan kräva ytterligare data toobe samlas in. Den här processen kallas **egenskapsval** och är en av hello viktiga steg i att skapa en effektiv förutsägelseanalyser system. Det här steget kräver en kreativa kombination av domän kunskaper och hello insikter från hello utforskning steget. Anvisningar finns [Egenskapsval i hello Team datavetenskap Process](machine-learning-data-science-create-features.md).

## <a name="4-create-predictive-models"></a>4. Skapa förutsägelsemodeller
Datavetare skapa analytiska modeller toopredict hello nyckelparametrar identifieras av hello affärskrav som definierats i hello planera steg med hjälp av data som har rensats och featurized. Machine learning system stöder flera **modeling algoritmer** som är tillämpliga tooa stort antal fall. Anvisningar finns [hur toochoose algoritmer för Team Azure Machine Learning](machine-learning-algorithm-choice.md).

Datavetare måste välja hello lämpligaste modellen för förutsägelse uppgiften och det är inte ovanligt att resultaten från flera modeller måste toobe kombinerade tooobtain hello bästa resultat. hello delas indata för modellering vanligtvis slumpmässigt i tre delar:

* en datauppsättning för träning 
* en datauppsättning för verifiering 
* en datauppsättning för testning 

hello modeller är byggda med hello **datauppsättning för träning**. hello optimal kombination av modeller (med parametrar justerade) väljs genom att köra hello modeller och mäta hello förutsägelse fel för hello **validering datauppsättning**. Slutligen hello **testa datauppsättning** är används tooevaluate hello prestanda av hello valt modell på oberoende data som inte har använt tootrain eller verifiera hello modellen.  Instruktioner finns i [hur tooevaluate modell prestanda i Azure Machine Learning](machine-learning-evaluate-model-performance.md).

## <a name="5-deploy-and-consume-models"></a>5. Distribuera och använda modeller
När vi har en uppsättning modeller som utför och de kan vara **operationalized** för andra program tooconsume. Beroende på hello affärsbehov förutsägelser görs i antingen **realtid** eller på en **batch** basis. toobe operationalized, hello modeller har toobe visas med en **öppna API-gränssnitt** som är enkelt används från olika program sådana online-webbplats, kalkylblad, instrumentpaneler och affärs- och backend-program för. Se [distribuera en Azure Machine Learning-webbtjänst](machine-learning-publish-a-machine-learning-web-service.md).

## <a name="summary-and-next-steps"></a>Sammanfattning och nästa steg.
Hej [Team av vetenskapliga data](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) modellerats som en sekvens med hävdade steg som **vägledning** hello aktiviteter nödvändiga toouse advanced analytics toobuild en intelligent program. Varje steg innehåller också information om hur toouse olika Microsoft technologies toocomplete hello uppgifter beskrivs. 

Medan TDSP inte fastställa specifika typer av **dokumentationen** artefakter, är det en bästa praxis toodocument hello-resultat av hello datagranskning, modellering och utvärdering och toosave hello relevant kod så som hello analys kan hävdade vid behov. Detta kan också återanvändning av hello analytics fungerar när du arbetar med andra program som involverar liknande data och uppgifter för förutsägelse.

Fullständig genomgång för slutpunkt-till-slutpunkt som visar alla hello steg i hello process för **specifika scenarier** tillhandahålls också. Visa, till exempel:

* [hello Team av vetenskapliga data i praktiken: använder SQL Server](machine-learning-data-science-process-sql-walkthrough.md)
* [hello Team av vetenskapliga data i praktiken: med HDInsight Hadoop-kluster](machine-learning-data-science-process-hive-walkthrough.md).
* [Datavetenskap med Spark på Azure HD.mdnsight](machine-learning-data-science-spark-overview.md)
* [Skalbar datavetenskap i Azure Data Lake: en genomgång för slutpunkt till slutpunkt](machine-learning-data-science-process-data-lake-walkthrough.md)

