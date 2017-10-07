---
title: "aaaUse hello provdatauppsättningar i Machine Learning Studio | Microsoft Docs"
description: "Beskrivningar av hello datauppsättningar som används i Exempelmodeller som ingår i Machine Learning Studio. Du kan använda dessa provdatauppsättningar för dina experiment."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 03a0b844-e8a7-4896-996f-d3c7a0db7a50
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: c7786478db82d40aaf27c37b3947ded5f042dd70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-sample-datasets-in-azure-machine-learning-studio"></a>Använd hello provdatauppsättningar i Azure Machine Learning Studio
[top]: #machine-learning-sample-datasets

När du skapar en ny arbetsyta i Azure Machine Learning med ett antal provdatauppsättningar och experiment som standard. Många av dessa provdatauppsättningar som används av hello Exempelmodeller i hello [Azure Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/). Andra ingår som exempel på olika typer av data som vanligtvis används i machine learning.

Vissa av dessa data är tillgängliga i Azure Blob storage. Hello följande tabell ger en direktlänk för dessa datamängder. Du kan använda dessa data i dina experiment med hjälp av hello [importera Data] [ import-data] modul.

hello resten av dessa provdatauppsättningar är tillgängliga i arbetsytan under **sparade datauppsättningar** hello modulen paletten toohello till vänster i hello experimentet när du öppnar eller skapa ett nytt experiment i Machine Learning Studio.
Du kan använda någon av dessa data i dina egna experiment genom att dra den tooyour experimentet.


[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<table>

<tr>
  <th align=left>DataSet-namnet</th>
  <th align=left>DataSet-beskrivning</th>
</tr>

<tr>
  <td valign=top>Vuxna inventering intäkter binär klassificering dataset</td>
  <td valign=top>
En delmängd av hello 1994 inventering databasen, med fungerande vuxna över hello ålder på 16 med ett justerade intäkter index med > 100.<p> </p><b>Användning:</b> klassificera personer som använder demografi toopredict om en person får än 50 kB per år.<p> </p><b>För forskning:</b> Kohavi R., Becker B., (1996). UCI Machine Learning databasen <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California, skola Information och datorvetenskap </td>
</tr>

<tr ID=airport-codes-dataset>
  <td valign=top>Flygplats koder Dataset</td>
  <td valign=top>
USA flygplats koder.<p> </p>Den här datauppsättningen innehåller en rad för varje USA flygplats, tillhandahåller hello flygplats-ID och namn tillsammans med hello plats orten och staten.
  </td>
</tr>

<tr>
  <td valign=top>Bil price data (Raw)</td>
  <td valign=top>
Information om bilar av märke och modell, inklusive hello pris, funktioner, till exempel hello antalet cylindrar och MPG samt en insurance riskpoäng.<p> </p>Hej riskpoäng associeras ursprungligen med automatisk pris och sedan justerad för faktiska risk i en process som kallas tooactuaries som symboling. + 3 värdet anger att hello auto är riskfyllda och ett värde av-3 som den är förmodligen.<p> </p><b>Användning:</b> Predict hello riskpoäng av funktioner som använder regression eller multivariate klassificering. <p> </p><b>För forskning:</b> Schlimmer, J.C. (1987). UCI Machine Learning databasen <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California, skola Information och datorvetenskap </td>
</tr>

<tr ID=bike-rental-uci-dataset>
  <td valign=top>Cykel uthyrning UCI dataset</td>
  <td valign=top>
UCI cykel uthyrning dataset som bygger på verkliga data från kapital Bikeshare företag som upprätthåller en cykel uthyrning nätverk i Washington DC.<p> </p>hello datauppsättningen innehåller en rad för varje timme av varje dag under 2011 och 2012, med totalt 17,379 rader. hello är varje timme cykel hyra från 1 too977.

  </td>
</tr>

<tr ID=bill-gates-rgb-image>
  <td valign=top>Faktura Gates RGB-bild</td>
  <td valign=top>
Offentligt tillgängliga avbildningsfil konverteras tooCSV data.<p> </p>hello kod för att konvertera hello avbildning finns i hello <strong>färg kvantifieringsfel via K-Means kluster</strong> modellen detaljsida.
  </td>
</tr>

<tr>
  <td valign=top>Blod bidrag data</td>
  <td valign=top>
En delmängd av data från hello blod bidragsgivare databas hello vinstsyfte Service Center Hsin Chu ort, Taiwan.<p> </p>Bidragsgivare data innehåller hello månader sedan senaste bidrag), och frekvens eller hello Totalt antal bidrag, tid sedan senaste bidrag och mängden blod donerat.<p> </p><b>Användning:</b> hello målet är toopredict via klassificering om hello bidragsgivare donerat blod i mars 2007, där 1 anger en bidragsgivare under hello målperiod, och 0 om en icke-bidragsgivare. <p> </p><b>För forskning:</b> Yeh I.C., (2008). UCI Machine Learning databasen <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California, skola Information och datorvetenskap <p> </p>Yeh, jag-Cheng, Yang, bestämmer Jang och Ting, Tao Ming ”Knowledge identifiering i läget Nedsatt funktionalitet modellen med Bernoulli sekvens” Expert system med program, 2008, <a href="http://dx.doi.org/10.1016/j.eswa.2008.07.018">http://dx.doi.org/10.1016/j.eswa.2008.07.018</a>
  </td>
</tr>

<tr ID=book-reviews-from-amazon>
  <td valign=top>Book granskningar från Amazon</td>
  <td valign=top>
Granskning av böcker i Amazon, tas från hello amazon.com webbplats av University Pennsylvania forskare (<a href="http://www.cs.jhu.edu/~mdredze/datasets/sentiment/">sentiment</a>). Se hello research dokumentet ”biografier, Bollywood, BOM rutor och blandningsföretag: domän anpassning för Sentiment klassificering” av John Blitzer och markera Dredze Fernando Pereira; Koppling av beräkningar Linguistics (ACL) 2007.<p> </p>hello ursprungliga datauppsättningen har 975K granskningar med rangordning 1, 2, 3, 4 och 5. hello omdömen har skrivits i engelska och är från hello tidsperiod 1997 – 2007. Den här datauppsättningen har ned sampla too10K granskningar.
  </td>
</tr>

<tr>
  <td valign=top>Bröstet bröstcancerdiagnoser data</td>
  <td valign=top>
En av tre bröstcancerdiagnoser-relaterade datamängder som tillhandahålls av hello Oncology Institute som förekommer ofta i machine learning dokumentation. Kombinerar diagnostikinformation med funktioner från laboratorieanalys av ungefär 300 vävnadsprover.<p> </p><b>Användning:</b> klassificera hello typ av bröstcancerdiagnoser, baserat på 9 attribut, vilket är linjär och vissa är kategoriska. <p> </p><b>För forskning:</b> Wohlberg, W.H., gata, W.N. och Mangasarian, O.L. (1995). UCI Machine Learning databasen <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California, skola Information och datorvetenskap </td>
</tr>

<tr ID=breast-cancer-features>
  <td valign=top>Bröstet Bröstcancerdiagnoser funktioner <td valign=top>
hello datamängden innehåller information om 102K misstänkta områden (kandidater) med röntgen bilder, beskrivs av 117 funktioner. hello funktioner är tillverkarspecifika och deras innebörd visas inte hello dataset skapare (Siemens hälsovård). 
  </td>
</tr>

<tr ID=breast-cancer-info>
  <td valign=top>Bröstet Bröstcancerdiagnoser Info</td>
  <td valign=top>
hello datauppsättningen innehåller ytterligare information för varje misstänkt region för röntgenbilden. Varje exempel innehåller information (t.ex. etikett, patient ID, koordinaterna för korrigering relativa toohello hela bilden) om hello motsvarande radnummer i hello bröstet Bröstcancerdiagnoser funktioner dataset. Varje tålamod har ett antal exempel. Några exempel är positivt för patienter som har en bröstcancerdiagnoser, och vissa är negativt. Patienter som inte har en bröstcancerdiagnoser är alla exempel negativt. hello datamängden har 102K exempel. prioriterar hello dataset, 0,6% av hello datapunkter är positiva, hello rest är negativt. hello datamängden har gjorts tillgängliga av Siemens hälsovård.
  </td>
</tr>

<tr ID=crm-appetency-labels-shared>
  <td valign=top>CRM Appetency etiketter delade</td>
  <td valign=top>
Etiketterna från hello KDD Cup 2009 customer relationship förutsägelse challenge (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_appetency.labels">orange_small_train_appetency.labels</a>).
  </td>
</tr>

<tr ID=crm-churn-labels-shared>
  <td valign=top>CRM omsättning etiketter delade</td>
  <td valign=top>
Etiketterna från hello KDD Cup 2009 customer relationship förutsägelse challenge (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_churn.labels">orange_small_train_churn.labels</a>).
  </td>
</tr>

<tr ID=crm-dataset-shared>
  <td valign=top>CRM datamängden delas</td>
  <td valign=top>
Den här informationen kommer från hello KDD Cup 2009 customer relationship förutsägelse challenge (<a href="http://www.sigkdd.org/kdd-cup-2009-customer-relationship-prediction - orange_small_train.data.zip">orange_small_train.data.zip</a>).<p> </p>hello datauppsättningen innehåller 50K kunder från hello franska Telecom företagets Orange. Varje kund har 230 anonymiserade funktioner, 190 som är numeriska och 40 är kategoriska. hello funktioner är mycket sparse.
  </td>
</tr>

<tr ID=crm-upselling-labels-shared>
  <td valign=top>CRM Upselling etiketter delade</td>
  <td valign=top>
Etiketterna från hello KDD Cup 2009 customer relationship förutsägelse challenge (<a href="http://www.sigkdd.org/site/2009/files/orange_large_train_upselling.labels">orange_large_train_upselling.labels</a>).
  </td>
</tr>

<tr>
  <td valign=top>Energi effektivitet Regression data</td>
  <td valign=top>
En samling av simulerade energi profiler, baserat på 12 skapande av olika former. hello byggnader särskiljs med hjälp av 8 funktioner, till exempel rutor området hello rutor området distribution och orientering.<p> </p><b>Användning:</b> använda regression eller klassificering toopredict hello energieffektivitet klassificeringen baserat som en av två riktigt värdefulla svar. Flera klassen klassificering, är i avrunda hello svar variabeln toohello närmsta heltal. <p> </p><b>För forskning:</b> Xifara A. & Tsanas A. (2012). UCI Machine Learning databasen <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California, skola Information och datorvetenskap </td>
</tr>

<tr ID=flight-delays-data>
  <td valign=top>Svarta fördröjningar Data</td>
  <td valign=top>
Passagerare som rör sig i tid prestandadata från hello TranStats insamling av hello USA Avdelning transport (<a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">i tid</a>).<p> </p>hello dataset omfattar hello tidsperiod April oktober 2013. Innan du laddar upp tooAzure Machine Learning Studio bearbetades hello datamängden på följande sätt:<ul><li>hello datamängden har filtrerade toocover endast hello 70 mest använda flygplatser i hello kontinentala USA</li><li>Avbrutna flygplan har märkt med skjutas upp med mer än 15 minuter</li><li>Distribueras flygplan har filtrerats ut</li><li>hello följande kolumner har valts: år, månad, dag i månaden, DayOfWeek, operatör, OriginAirportID, DestAirportID, CRSDepTime, DepDelay, DepDel15, CRSArrTime, ArrDelay, ArrDel15, avbruten</li></ul>
</td>
</tr>

<tr>
  <td valign=top>Rör sig i tid prestanda (Raw)</td>
  <td valign=top>
Registrerar flygplan svarta mottagna och avvikelser i USA från oktober 2011.<p> </p><b>Användning:</b> förutsäga svarta fördröjningar. <p> </p><b>För forskning:</b> från USA Avd. transporten <a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time</a>.
  </td>
</tr>

<tr>
  <td valign=top>Data om skogsbränder</td>
  <td valign=top>
Innehåller väder data, till exempel temperatur- och fuktighetskonsekvens index och vindhastigheten nordöstra Portugal, tillsammans med uppgifter om skogsbränder i ett område.<p> </p><b>Användning:</b> är en aktivitet med svårt regression där hello syfte är toopredict hello bränts område i skogsbränder. <p> </p><b>För forskning:</b> Cortez P., & Morais A. (2008). UCI Machine Learning databasen <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California, skola Information och datorvetenskap <p> </p>[Cortez och Morais 2007] S. Cortez och A. Morais. Data Utvinningsmodellen metod-tooPredict skogsbränder med meteorologiska Data. I J. Neves, m-F. Santos och J. Machado Eds., nya trender i styrs av datorn, förfaranden hello 13 EPIA 2007 - portugisiska konferens på styrs av datorn, December, 523-Guimarães Portugal sidor 512, 2007. APPIA ISBN 13 978-989-95618-0-9. Finns på: <a href="http://www.dsi.uminho.pt/~pcortez/fires.pdf">http://www.dsi.uminho.pt/~pcortez/fires.pdf</a>.
  </td>
</tr>

<tr ID=german-credit-card-uci-dataset>
  <td valign=top>Tyska kreditkort UCI dataset</td>
  <td valign=top>
Hej UCI Statlog (tyska kreditkort) datauppsättningen (<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">Statlog + tyska + kredit + Data</a>), med hjälp av hello german.data fil.<p> </p>hello dataset klassificerar personer beskrivs av en uppsättning attribut som låg eller hög kreditrisker. Varje exempel representerar en person. Det finns 20 funktioner, både numeriska och kategoriska, och en binär etikett (hello kredit risk värde). Hög kredit risk poster har etikett = 2, låg kredit risk poster har etikett = 1. Hej misclassifying ett exempel på låg risk som hög är 1, hello kostnaden för misclassifying en hög risk exempel så låga är 5.
  </td>
</tr>

<tr ID=imdb-movie-titles>
  <td valign=top>IMDB text</td>
  <td valign=top>
hello datauppsättningen innehåller information om filmer som har klassificerat i Twitter tweets: IMDB film ID, film namn, genre och produktionsår. Det finns 17 kB filmer i hello dataset. hello dataset introducerades i hello dokumentet ”S. Dooms, T. Tyskland Pessemier och L. Martens. MovieTweetings: en film klassificeringen Dataset som samlas in från Twitter. Workshop på gemensamt skapade och mänsklig beräkning för rekommenderare CrowdRec vid RecSys 2013 ”.
  </td>
</tr>

<tr>
  <td valign=top>Iris två klassdata</td>
  <td valign=top>
Detta är kanske hello bästa kända databasen toobe hittades i hello mönster recognition dokumentation. hello dataset är relativt liten, som innehåller 50 exempel varje på bladet mått från tre iris sorter.<p> </p><b>Användning:</b> förutsäga hello iris typen från hello mått.  <p> </p><b>För forskning:</b> Fisher R.A. (1988). UCI Machine Learning databasen <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California, skola Information och datorvetenskap </td>
</tr>

<tr ID=movie-tweets>
  <td valign=top>Film Tweets</td>
  <td valign=top>
hello dataset är en utökad version av hello film Tweetings dataset. hello datamängden har 170K klassificering för filmer, extraheras från väl strukturerade tweets på Twitter. Varje instans representerar en tweet och är en tuppel: användar-ID, IMDB film ID, klassificering, timestamp, antalet för den här tweet och antalet retweets för den här tweet. hello datamängden har gjorts tillgängliga av A. SA, S. Dooms, B. Loni och D. Tikk för rekommenderare system Challenge 2014.
  </td>
</tr>

<tr>
  <td valign=top>MPG-data för olika bilar</td>
  <td valign=top>
Den här datauppsättningen utgör en lite annorlunda version av hello dataset som tillhandahålls av hello StatLib bibliotek med Carnegie Mellon University. hello datamängden har använts i hello 1983 American statistiska Association verksamhetshandbok för tillverkning.<p> </p>hello data visar bränsleförbrukningen för olika bilar i miles per gallon, tillsammans med information sådana hello antalet cylindrar, motorn Förskjutning, hästkrafter, total vikt och acceleration.<p> </p><b>Användning:</b> förutsäga bränsleekonomi baserat på 3 diskreta flervärdesattribut och 5 kontinuerliga attribut. <p> </p><b>För forskning:</b> StatLib Carnegie Mellon University (1993). UCI Machine Learning databasen <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California, skola Information och datorvetenskap </td>
</tr>

<tr>
  <td valign=top>Pima människor Diabetes binär klassificering dataset</td>
  <td valign=top>
En delmängd av data från hello National Institute Diabetes och mag och lever sjukdomar databas. hello dataset var filtrerade toofocus på hondjur patienter av Pima indiska arv. hello data innehåller medicinska data, till exempel glukos och inulinsirap nivåer samt lifestyle faktorer.<p> </p><b>Användning:</b> förutsäga om hello ämne har diabetes (binär klassificering). <p> </p><b>För forskning:</b> Sigillito, V. (1990). UCI Machine Learning databasen <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml ”</a>. Irvine, CA: University of California, skola Information och datorvetenskap </td>
</tr>

<tr>
  <td valign=top>Restaurang kundinformation</td>
  <td valign=top>
En uppsättning metadata om kunder, inklusive demografisk information och inställningar.<p> </p><b>Användning:</b> använder den här datauppsättningen, i kombination med hello andra två restaurang datauppsättningar, tootrain och testa ett rekommenderare system. <p> </p><b>För forskning:</b> Bache K. och Lichman M. (2013). UCI Machine Learning databasen <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California, skola Information och datorvetenskap.
  </td>
</tr>

<tr>
  <td valign=top>Restaurang funktionsdata</td>
  <td valign=top>
En uppsättning metadata om hotell och deras funktioner, till exempel mat typ, äta middag format och plats.<p> </p><b>Användning:</b> använder den här datauppsättningen, i kombination med hello andra två restaurang datauppsättningar, tootrain och testa ett rekommenderare system. <p> </p><b>För forskning:</b> Bache K. och Lichman M. (2013). UCI Machine Learning databasen <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California, skola Information och datorvetenskap.
  </td>
</tr>

<tr>
  <td valign=top>Restaurang klassificeringar</td>
  <td valign=top>
Innehåller klassificeringar som angetts av användare toorestaurants på en skala från 0 too2.<p> </p><b>Användning:</b> använder den här datauppsättningen, i kombination med hello andra två restaurang datauppsättningar, tootrain och testa ett rekommenderare system. <p> </p><b>För forskning:</b> Bache K. och Lichman M. (2013). UCI Machine Learning databasen <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California, skola Information och datorvetenskap.
  </td>
</tr>

<tr>
  <td valign=top>Stål Annealing flera klassen dataset</td>
  <td valign=top>
Den här datauppsättningen innehåller en serie av poster från stål glödgning försök med hello fysiska attribut (bredd, tjocklek, typ (slutet, blad, etc.) av hello resulterande stål typer.<p> </p><b>Användning:</b> förutsäga något av två numeriska klassattribut; hårdhet eller styrka. Du kan också analysera visar sambandet mellan attribut.<p> </p>Stålsorter följer en set-standard definieras av SAE och andra organisationer. Du letar efter en specifik 'klass' (hello klassvariabel) och vill toounderstand hello värden som behövs. <p> </p><b>För forskning:</b> pund, D. & Buntine, W. (NA). UCI Machine Learning databasen <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California, skola Information och datorvetenskap <p> </p>En användbar guide toosteel betyg finns här: <a href="http://www.outokumpu.com/SiteCollectionDocuments/Outokumpu-steel-grades-properties-global-standards.pdf">http://www.outokumpu.com/SiteCollectionDocuments/Outokumpu-steel-grades-properties-global-standards.pdf</a>
  </td>
</tr>

<tr>
  <td valign=top>Teleskopet data</td>
  <td valign=top>
Posterna för hög energi gamma partikel belastning tillsammans med bakgrundsljud både simuleras med hjälp av en Monte Carlo process.<p> </p>hello syftet med hello simuleringen var tooimprove hello korrektheten i grunden-baserade luften Cherenkov gamma teleskop, med hjälp av statistiska metoder toodifferentiate mellan hello önskade signal (Cherenkov strålning duschar) och bakgrundsljud (hadronic duschar som initieras av cosmic strålar i hello övre luften IT-avdelning).<p> </p>hello data har bearbetats toocreate ett avlångt kluster med lång hello-axeln är inriktad på hello kamera center. hello egenskaperna för den här ellips (kallas ofta Hillas parametrar) är bland hello avbildningen parametrar som kan användas för diskriminering.<p> </p><b>Användning:</b> förutsäga om representerar bild av en signal eller bakgrunden brus.<p> </p><b>Kommentarer:</b> enkel klassificering noggrannhet är inte relevant för dessa data sedan klassificera en händelse i bakgrunden som signalen är lägre än klassificera en signalhändelse som bakgrund. Jämförelse av olika klassificerare ska hello ROC diagrammet användas. Hej sannolikheten för att ta emot en händelse i bakgrunden som signal måste vara under följande tröskelvärden hello: 0,01 0,02, 0,05, 0,1 eller 0,2.<p> </p>Observera också att underskattar hello antalet händelser som bakgrund (för hadronic duschar h), medan i verkliga mätningar hello h eller brus klassen representerar hello merparten av händelser. <p> </p><b>För forskning:</b> Bock, R.K. (1995). UCI Machine Learning databasen <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California, skola Information </td>
</tr>

<tr ID=weather-dataset>
  <td valign=top>Väder Dataset</td>
  <td valign=top>
Varje timme mark-baserade väder observationer från NOAA (<a href="http://cdo.ncdc.noaa.gov/qclcd_ascii/, merged data from 201304 too201310">samman data från 201304 too201310</a>).<p> </p>hello väder data omfattar observationer från flygplats väder stationer, som omfattar hello tidsperiod April oktober 2013. Innan du laddar upp tooAzure Machine Learning Studio bearbetades hello datamängden på följande sätt:<ul><li>Väder station-ID: N har mappade toocorresponding flygplats ID: N</li><li>Väder stationer som inte är associerade med hello 70 mest använda flygplatser har filtrerats ut</li><li>hello datumkolumnen är uppdelad i separata år, månad och dag kolumner</li><li>hello följande kolumner har valts: AirportID, år, månad, dag, tid, tidszon, SkyCondition, synlighet, WeatherType, DryBulbFarenheit, DryBulbCelsius, WetBulbFarenheit, WetBulbCelsius, DewPointFarenheit, DewPointCelsius, RelativeHumidity, Vindhastigheten, WindDirection, ValueForWindCharacter, StationPressure, PressureTendency, PressureChange, SeaLevelPressure, RecordType, HourlyPrecip, Altimeter</li></ul>
  </td>
</tr>

<tr ID=wikipedia-sp-500-dataset>
  <td valign=top>Wikipedia SP 500 Dataset</td>
  <td valign=top>
Data hämtas från Wikipedia (<a href="http://www.wikipedia.org/">http://www.wikipedia.org/</a>) baserat på artiklar för varje företag S & P 500 lagras som XML-data.<p> </p>Innan du laddar upp tooAzure Machine Learning Studio bearbetades hello datamängden på följande sätt:<ul><li>Extrahera textinnehåll för varje företag</li><li>Ta bort wiki formatering</li><li>Ta bort icke-alfanumeriska tecken</li><li>Konvertera alla text toolowercase</li><li>Kända företagets kategorier har lagts till</li></ul><p> </p>Observera att vissa företag en artikel inte kunde hittas, så hello antalet poster är mindre än 500.
  </td>
</tr>





<tr ID=direct-marketing>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/direct_marketing.csv">direct_marketing.csv</a></td>
  <td valign=top>
hello datauppsättningen innehåller kunddata och uppgifter om deras svar tooa direkt e kampanj. Varje rad representerar en kund. hello datauppsättningen innehåller 9 funktioner om användaren demografi och tidigare beteende och 3 etikettkolumner (Besök konvertering och ägna).  Besök är en binary-kolumn som anger att en kund som besökt när hello marknadsföringskampanj konvertering anger en kund köpt något och ägna är hello belopp som har använt.  hello datamängden har gjorts tillgängliga av Kevin Hillstrom för MineThatData e-Analytics och Data Mining utmaning.
  </td>
</tr>

<tr ID=lyrl2004-tokens-test>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_test.csv">lyrl2004_tokens_test.csv</a></td>
  <td valign=top>
Funktioner i test exemplen i hello RCV1 V2 Reuters nyheter dataset. hello datamängden har 781K nyhetsartiklar tillsammans med deras ID. (första kolumnen i hello dataset). Varje artikel tokeniserad stopworded, och berodde. hello datamängden har gjorts tillgängliga av David. D. Lewis.
  </td>
</tr>

<tr ID=lyrl2004-tokens-train>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_train.csv">lyrl2004_tokens_train.csv</a></td>
  <td valign=top>
Funktioner för utbildning exemplen i hello RCV1 V2 Reuters nyheter dataset. hello datamängden har 23K nyhetsartiklar tillsammans med deras ID. (första kolumnen i hello dataset). Varje artikel tokeniserad stopworded, och berodde. hello datamängden har gjorts tillgängliga av David. D. Lewis.
  </td>
</tr>

<tr ID=intrusion-detection>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a><br></td>
  <td valign=top>
DataSet från hello KDD Cup 1999 Knowledge identifiering och Data Mining verktyg konkurrens (<a href="http://kdd.ics.uci.edu/databases/kddcup99/kddcup99.html">kddcup99.html</a>).<p> </p>hello datamängden har hämtats och lagrats i Azure Blob storage (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a>) och innehåller både träning och testning datauppsättningar. hello utbildning datamängden har cirka 126K rader och 43 kolumner, inklusive hello etiketter. Tre kolumner är en del av hello Etikettinformation och 40 kolumner, som består av strängen kategoriska och numeriska funktioner är tillgängliga för hello modell. hello testdata har cirka 22,5 K testa exempel med hello samma 43 kolumner som hello övningsdata.

  </td>
</tr>

<tr ID=rcv1-v2-topics-qrels>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/rcv1-v2.topics.qrels.csv">rcv1 v2.topics.qrels.csv</a></td>
  <td valign=top>
Avsnittet om porttilldelningar för nyhetsartiklar i hello RCV1 V2 Reuters nyheter dataset. En nyhetsartikel i kan tilldelas tooseveral avsnitt. hello-format för varje rad ”&lt;avsnittsnamn&gt; &lt;dokument-id&gt; 1”. hello datauppsättningen innehåller 2.6M avsnittet tilldelningar. hello datamängden har gjorts tillgängliga av David. D. Lewis.
  </td>
</tr>

<tr ID=student-performance>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a></td>
  <td valign=top>
Den här informationen kommer från hello KDD Cup 2010 Student prestanda utvärdering challenge (<a href="http://www.kdd.org/kdd-cup-2010-student-performance-evaluation">student prestandautvärdering</a>). hello data som används är hello Algebra_2008_2009 träningsmängden (Stamper, J., Niculescu-Mizil A. Ritter, S. Gordon, G.J. och Koedinger K.R. (2010). Algebra jag 2008-2009. Challenge dataset från KDD Cup 2010 utbildningssyfte Data Mining utmaning. Hitta den på <a href="http://pslcdatashop.web.cmu.edu/KDDCup/downloads.jsp">downloads.jsp</a> eller <a href="http://www.kdd.org/sites/default/files/kddcup/site/2010/files/algebra_2008_2009.zip">algebra_2008_2009.zip</a>.<p> </p>hello datamängden har hämtats och lagrats i Azure Blob storage (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a>) och innehåller loggfilerna från en student tutoring system. hello angetts bland annat problem-ID och dess kort beskrivning student ID, tidsstämpel och hur många försöker hello student innan lösa hello problem i hello Högerklicka sätt. hello ursprungliga datauppsättningen har 8.9M poster. den här datauppsättningen har ned sampla toohello första 100K rader. hello datamängden har 23 tabbavgränsade kolumner med olika typer: numeriska, kategoriska, och tidsstämpel.

  </td>
</tr>




</table>


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
