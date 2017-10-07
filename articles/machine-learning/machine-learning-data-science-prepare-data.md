---
title: "aaaClean och förbereda data för Azure Machine Learning | Microsoft Docs"
description: "Före processer och rensa data tooprepare den för machine learning."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: bdf659ec-4881-4324-8b9c-747cbfa0c3cd
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: 3e3c3e4b0cfb9187f5820d7165e6ee1ea013ba02
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tasks-tooprepare-data-for-enhanced-machine-learning"></a>Uppgifter tooprepare data för förbättrad machine learning
Förbearbetning och rensa data är viktiga uppgifter som vanligtvis utföras innan dataset kan användas effektivt för machine learning. Rådata är ofta mycket brus och otillförlitliga och saknar värden. Med hjälp av dessa data för modellering kan ge missvisande resultat. Dessa uppgifter är en del av hello Team Data vetenskap processen (TDSP) och följ vanligtvis en inledande undersökning av en dataset används toodiscover och planera hello före bearbetning som krävs. Mer detaljerad information om hello TDSP processen finns hello steg som beskrivs i hello [Team datavetenskap Process](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

Förbearbetning och rensa uppgifter, kan som hello data från kartläggning av naturresurser uppgiften utföras i en mängd olika miljöer, till exempel SQL- eller Hive- eller Azure Machine Learning Studio och med olika verktyg och språk, till exempel R eller Python, beroende på om dina data är lagras och hur den har formaterats. Eftersom TDSP iterativ karaktär, kan dessa uppgifter ske på flera steg i hello arbetsflöde för hello processen.

Den här artikeln introducerar olika databearbetning begrepp och uppgifter som kan utföras före eller efter att mata in data i Azure Machine Learning.

Ett exempel på datagranskning och före som bearbetas i Azure Machine Learning studio finns hello [förbearbetning av data i Azure Machine Learning Studio](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/) video.

## <a name="why-pre-process-and-clean-data"></a>Varför Förbearbeta och rensa data?
Verkligt data har samlats in från olika källor och processer och den kan innehålla oegentligheter eller skadade data genom att kompromettera hello kvaliteten på hello dataset. hello vanliga data quality problem som uppstår är:

* **Ofullständig**: Data saknar attribut eller som innehåller värden som saknas.
* **Störningar**: Data innehåller felaktiga poster eller avvikare.
* **Inkonsekvent**: Data innehåller poster i konflikt eller avvikelser.

Data kvalitet är en förutsättning för kvalitet förutsägelsemodeller. tooavoid ”skräp i skräp ut” och förbättra kvalitet och därför modellen prestanda, det är absolut nödvändigt tooconduct en data hälsa skärmen toospot data problem tidigt och fatta beslut om hello motsvarande databehandling och rensa steg.

## <a name="what-are-some-typical-data-health-screens-that-are-employed"></a>Vilka är några vanliga hälsa skärmar som används?
Vi kan kontrollera hello allmänna kvalitet data genom att kontrollera:

* Hej antalet **poster**.
* Hej antalet **attribut** (eller **funktioner**).
* hello attributet **datatyper** (nominell, ordningstal eller kontinuerlig).
* Hej antalet **saknade värden**.
* **-Format** hello data.
  * Om hello data finns i TVS eller CSV, kontrollerar du att hello kolumnen avgränsare och rad avgränsare alltid korrekt separata kolumner och rader.
  * Kontrollera om hello data är korrekt formaterad baserat på deras respektive standarder om hello data är i HTML- eller XML-format.
  * Parsning kan också vara nödvändigt i ordning tooextract strukturerad information från delvis strukturerade eller Ostrukturerade data.
* **Inkonsekvent dataposter**. Kontrollera hello värdeintervall är tillåtna. t.ex. Om hello data innehåller student GPA, kontrollera om hello GPA är i hello avses intervallet, säg 0 ~ 4.

När du upptäcker problem med data, **bearbetningssteg** finns behov som ofta omfattar rensa värden som saknas, databasnormalisering, discretization, tooremove för bearbetning av text och/eller ersätta inbäddade tecken som kan påverka datajustering av, blandat datatyperna gemensamma fält och andra.

**Azure Machine Learning förbrukar korrekt tabelldata**.  Om hello data redan är i tabellform kan före databehandling utföras direkt med Azure Machine Learning i hello Machine Learning Studio.  Om data inte är i tabellform kan Säg XML, parsning krävas i tooconvert hello data tootabular beställningsformulär.  

## <a name="what-are-some-of-hello-major-tasks-in-data-pre-processing"></a>Vilka är några av hello viktiga uppgifter i föregående databearbetning?
* **Datarensning**: Fyll i eller saknade värden, identifiera och ta bort störningar data och avvikare.
* **DTS**: normalisera data tooreduce dimensioner och brus.
* **Data minskning**: exempel dataposter eller attribut för enklare datahantering av.
* **Data discretization**: konvertera kontinuerliga attribut toocategorical attribut för att underlätta för användning med vissa machine learning-metoder.
* **Rensa text**: ta bort inbäddade tecken som kan orsaka data feljusteringarna för till exempel inbäddade flikar i en datafil med tabbavgränsade, inbäddade nya rader som kan bryta poster, osv.

hello avsnitten nedan finns information om några av de här stegen för databearbetning.

## <a name="how-toodeal-with-missing-values"></a>Hur toodeal med saknade värden?
toodeal med värden som saknas, är det bäst toofirst identifiera hello orsak för hello saknas värden toobetter referensen hello problem. Vanliga saknas värdet hantering metoderna är:

* **Ta bort**: ta bort poster med värden som saknas
* **Dummy ersättning**: ersätta saknade värden med ett värde för dummy: t. ex. *okänd* för kategoriska eller 0 för numeriska värden.
* **Innebära ersättning**: om hello data som saknas är numeriska ersätter hello saknade värden med hello medelvärde.
* **Frekventa ersättning**: om hello saknas data är kategoriska kan ersätta hello saknade värden med hello vanligaste objekt
* **Regression ersättning**: Använd det regression metoden tooreplace saknas värden med regressed värden.  

## <a name="how-toonormalize-data"></a>Hur toonormalize data?
Databasnormalisering nytt skalas numeriska värden tooa intervall som anges. Populära databasnormalisering metoderna är:

* **Min Max normalisering**: linjärt transformera hello tooa dataområdet, säg mellan 0 och 1, där hello minimivärdet är skalade too0 och högsta värdet too1.
* **Z-score normalisering**: skala data baserat på medelvärdet och standardavvikelsen: dela hello skillnaden mellan hello data och hello medelvärdet av hello standardavvikelsen.
* **Decimal skalning**: skala hello data genom att flytta hello decimaltecknet för hello attributvärde.  

## <a name="how-toodiscretize-data"></a>Hur toodiscretize data?
Data kan vara diskretisera genom att konvertera kontinuerlig värden toonominal attribut eller intervall. Det finns några olika sätt att göra detta:

* **Lika breda Diskretisering**: dela hello intervallet av alla möjliga värden för ett attribut i N grupper av hello samma storlek och tilldela hello värden i en bin med hello bin nummer.
* **Lika höjd Diskretisering**: dela hello intervall varje som innehåller hello samma antal instanser av alla värden för ett attribut i N grupper, och sedan tilldela hello värden i en bin med hello bin nummer.  

## <a name="how-tooreduce-data"></a>Hur tooreduce data?
Det finns olika metoder tooreduce Datastorlek för enklare hantering. Beroende på data storlek och hello domän kan hello följande metoder användas:

* **Registrera provtagning**: exempel hello dataposter och välj endast hello representativ underuppsättning hello data.
* **Attributet provtagning**: Välj endast en delmängd av de viktigaste hello-attribut från hello data.  
* **Aggregeringen**: dela hello data i grupper och lagra hello siffror för varje grupp. Till exempel samman hello dagliga intäkter i en restaurang kedja över hello senaste 20 år ligger toomonthly intäkter tooreduce hello storleken på hello data.  

## <a name="how-tooclean-text-data"></a>Hur tooclean textdata?
**Textfält i tabelldata** kan innehålla tecken som påverkar kolumner justering och/eller post gränser. För t.ex. inbäddade flikar i en fil med tabbavgränsade orsak kolumnen feljusteringarna och inbäddade bryta tecken för ny rad poster rader. Felaktig textkodning hantering vid skrivning/läsning text leder tooinformation förlust, oavsiktligt introduktion oläsliga tecken, t.ex. null-värden och kan även påverka text parsning. Noggrann parsning och redigera kan krävas i ordning tooclean textfält för rätt justering och/eller tooextract strukturerade data från text Ostrukturerade och halvstrukturerade data.

**Datagranskning** ger en tidig vy i hello data. Ett antal problem kan inte är öppnade avslöjar under det här steget och motsvarande metoder kan vara tillämpade tooaddress dessa problem.  Det är viktigt tooask frågor, till exempel vad är hello hello problemet och hur hello problemet kanske har införts. Det hjälper dig att besluta om hello databearbetning steg som behöver toobe tas tooresolve även dem. hello slags insikter en avser tooderive från hello data kan också vara används tooprioritize hello databearbetning arbete.

## <a name="references"></a>Referenser
> *Datautvinning: Koncept och tekniker*, tredje utgåvan Morgan Kaufmann 2011 Jiawei Han och Micheline Kamber Jian Pei
> 
> 

