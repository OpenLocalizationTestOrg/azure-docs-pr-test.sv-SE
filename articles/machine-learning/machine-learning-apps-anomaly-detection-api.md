---
title: aaaAzure Machine Learning Avvikelseidentifiering identifiering API | Microsoft Docs
description: "Avvikelseidentifiering identifiering API är ett exempel som skapats med Microsoft Azure Machine Learning som identifierar avvikelser i tid series-data med numeriska värden som jämnt fördelade i tid."
services: machine-learning
documentationcenter: 
author: alokkirpal
manager: jhubbard
editor: cgronlun
ms.assetid: 52fafe1f-e93d-47df-a8ac-9a9a53b60824
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/05/2017
ms.author: alok;rotimpe
ms.openlocfilehash: ce153689b8ddb36b67a2ad3607d846ea83ebcf61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="machine-learning-anomaly-detection-api"></a>Maskininlärning Avvikelseidentifiering API
## <a name="overview"></a>Översikt
[Avvikelseidentifiering identifiering API](https://gallery.cortanaintelligence.com/MachineLearningAPI/Anomaly-Detection-2) är ett exempel som skapats med Azure Machine Learning som identifierar avvikelser i tid series-data med numeriska värden som jämnt fördelade i tid.

Detta API kan identifiera hello följande typer av avvikande mönster i tid series-data:

* **Positiva och negativa trender**: till exempel när övervakning minnesanvändning i computing trenden kan vara av intresse eftersom den kan vara en indikation på en minnesläcka
* **Ändringar i hello dynamiska värdeintervallet**: till exempel när du övervakar hello undantag som utlöses av en tjänst i molnet, ändringar i hello dynamiska värdeintervallet kan tyda på instabilitet i hello hälsotillstånd hello-tjänsten och
* **Ger spikar i diagrammet och sjunker**: till exempel när du övervakar hello antal misslyckade inloggningar i en tjänst eller ett antal utcheckningar i en e-handelsplats toppar eller korta kan tyda på onormalt beteende.

Dessa machine learning detektorerna spåra ändringarna i värden över tid och rapporten pågående ändringar i deras värden som avvikelseidentifiering resultat. De kräver inte ad hoc tröskelvärdet justera och deras resultat kan vara används toocontrol falska positiva satsen. Hej avvikelseidentifiering API är användbart i flera scenarier som övervakning genom att spåra KPI: er med tiden användning läser övervakning via mätvärden, till exempel antal sökningar, antal klick prestandaövervakning via räknare som minne, CPU, filer etc. över tid.

Hej Avvikelseidentifiering erbjudande medföljer användbara verktyg tooget du startade.

* Hej [webbprogrammet](http://anomalydetection-aml.azurewebsites.net/) hjälper dig att utvärdera och visualisera hello resultaten av avvikelseidentifiering API: er på dina data.

> [!NOTE]
> Försök **IT Avvikelseidentifiering Insights-lösningen** drivs av [detta API](https://gallery.cortanaintelligence.com/MachineLearningAPI/Anomaly-Detection-2)
> 
> tooget end tooend lösningen distribueras tooyour Azure-prenumeration <a href="https://gallery.cortanaintelligence.com/Solution/Anomaly-Detection-Pre-Configured-Solution-1" target="_blank"> **börja här >**</a>
> 
>

## <a name="api-deployment"></a>API-distribution
I ordning toouse hello API, måste du distribuera den tooyour Azure-prenumeration där den kommer att finnas som en Azure Machine Learning-webbtjänst.  Du kan göra detta från hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/MachineLearningAPI/Anomaly-Detection-2).  Det här distribuerar två AzureML-webbtjänster (och deras relaterade resurser) tooyour Azure-prenumeration – ett för avvikelseidentifiering med säsongsvärdet identifiering och ett utan säsongsvärdet identifiering.  När hello distributionen är klar, kommer du att kunna toomanage dina API: er från hello [AzureML-webbtjänster](https://services.azureml.net/webservices/) sidan.  Från den här sidan kan kommer du att kunna toofind dina slutpunkter, API-nycklar samt exempelkod för att anropa API för hello.  Mer detaljerade instruktioner finns [här](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-manage-new-webservice).

## <a name="scaling-hello-api"></a>Skalning hello-API
Som standard har distributionen ett ledigt utveckling och testning faktureringsavtal som innehåller 1 000 transaktioner per månad och 2 beräkning timmar per månad.  Du kan uppgradera tooanother plan enligt dina behov.  Information om hello priser av olika planer finns [här](https://azure.microsoft.com/en-us/pricing/details/machine-learning/) under ”priser för produktion webb-API”.

## <a name="managing-aml-plans"></a>Hantera AML-planer 
Du kan hantera faktureringsavtalet [här](https://services.azureml.net/plans/).  hello namn kommer att baseras på hello resursgruppens namn du väljer när du distribuerar hello API, plus en sträng som är unik tooyour prenumeration.  Anvisningar om hur tooupgrade planen finns [här](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-manage-new-webservice) under hello ”hantera fakturering planer” avsnittet.

## <a name="api-definition"></a>API-Definition
hello webbtjänsten innehåller ett REST-baserad API över HTTPS som kan användas på olika sätt, inklusive en webbplats eller ett mobilt program, R, Python, Excel, osv.  Du skickar din serie data toothis tidstjänst via ett REST API-anrop och den körs en kombination av hello tre avvikelseidentifiering typer som beskrivs nedan.

## <a name="calling-hello-api"></a>Anropar hello-API
I ordning toocall hello API behöver du tooknow hello endpoint plats och API-nyckel.  Båda dessa tillsammans med exempelkod för att anropa hello API är tillgängliga från hello [AzureML-webbtjänster](https://services.azureml.net/webservices/) sidan.  Navigera toohello önskad API och klicka sedan på hello ”förbruka” fliken toofind dem.  Observera att du kan anropa hello API som Swagger API (dvs. med hello URL-parametern `format=swagger`) eller som en icke - Swagger API (dvs utan hello `format` URL-parametern).  hello exempelkod använder hello Swagger-format.  Nedan visas ett exempel förfrågan och svar i icke-Swagger-format.  Exemplen är toohello säsongsvärdet slutpunkt.  hello icke säsongsvärdet slutpunkt är liknande.

### <a name="sample-request-body"></a>Exempel Begärandetexten
hello-begäran innehåller två objekt: `Inputs` och `GlobalParameters`.  Hej exempelbegäran nedan, vissa parametrar skickas uttryckligen medan andra inte (Rulla ned till en fullständig lista över parametrar för varje slutpunkt).  Parametrar som inte skickas explicit i hello begäran använder hello standardvärden som anges nedan.

    {
                "Inputs": {
                        "input1": {
                                "ColumnNames": ["Time", "Data"],
                                "Values": [
                                        ["5/30/2010 18:07:00", "1"],
                                        ["5/30/2010 18:08:00", "1.4"],
                                        ["5/30/2010 18:09:00", "1.1"]
                                ]
                        }
                },
        "GlobalParameters": {
            "tspikedetector.sensitivity": "3",
            "zspikedetector.sensitivity": "3",
            "bileveldetector.sensitivity": "3.25",
            "detectors.spikesdips": "Both"
        }
    }

### <a name="sample-response"></a>Exempelsvar
Observera att, i ordning toosee hello `ColumnNames` fältet, måste du inkludera `details=true` som en URL-parametern i begäran.  Finns hello tabellerna nedan för hello betydelse bakom var och en av dessa fält.

    {
        "Results": {
            "output1": {
                "type": "table",
                "value": {
                    "Values": [
                        ["5/30/2010 6:07:00 PM", "1", "1", "0", "0", "-0.687952590518378", "0", "-0.687952590518378", "0", "-0.687952590518378", "0"],
                        ["5/30/2010 6:08:00 PM", "1.4", "1.4", "0", "0", "-1.07030497733224", "0", "-0.884548154298423", "0", "-1.07030497733224", "0"],
                        ["5/30/2010 6:09:00 PM", "1.1", "1.1", "0", "0", "-1.30229513613974", "0", "-1.173800281031", "0", "-1.30229513613974", "0"]
                    ],
                    "ColumnNames": ["Time", "OriginalData", "ProcessedData", "TSpike", "ZSpike", "BiLevelChangeScore", "BiLevelChangeAlert", "PosTrendScore", "PosTrendAlert", "NegTrendScore", "NegTrendAlert"],
                    "ColumnTypes": ["DateTime", "Double", "Double", "Double", "Double", "Double", "Int32", "Double", "Int32", "Double", "Int32"]
                }
            }
        }
    }


## <a name="score-api"></a>Poäng API
hello poäng API används för att köra avvikelseidentifiering på icke-säsongsbaserade tid series-data. hello API kör ett antal avvikelseidentifiering detektorerna på hello data och returnerar sina avvikelseidentifiering poäng. hello bilden nedan visar ett exempel på avvikelser som hello poäng API kan identifiera. Den här tidsserier har 2 distinkta ändras och 3 toppar. hello röda punkter visar hello tid vid vilken hello nivå ändring identifieras och hello svart punkter visar hello upptäckte toppar.
![Poäng API][1]

### <a name="detectors"></a>Detektorerna
Hej avvikelseidentifiering API stöder detektorerna i 3 kategorier. Information om specifika indataparametrar och utdata för varje detektor kan hittas i hello i den följande tabellen.

| Detektor kategori | Detektor | Beskrivning | Indataparametrar | utdata |
| --- | --- | --- | --- | --- |
| Topp detektorerna |TSpike detektor |Identifiera toppar och korta baserat på långt hello värden är från första och tredje Kvartiler |*tspikedetector.sensitivity:* tar heltalsvärde i hello intervallet 1-10 standard: 3; Högre värden ska fånga mer extrema värden, vilket gör det mindre känsliga |TSpike: binära värden – '1' om topp/dip upptäcks, '0' annars |
| Topp detektorerna | ZSpike detektor |Identifiera toppar och korta baserat på hur långt hello datapoints är medelvärdet för |*zspikedetector.sensitivity:* ta heltalsvärde i hello intervallet 1-10 standard: 3; Högre värden ska fånga mer extrema värden, vilket gör det mindre känsliga |ZSpike: binära värden – '1' om topp/dip upptäcks, '0' annars | |
| Långsam Trend detektor |Långsam Trend detektor |Identifiera långsamma positivt trend enligt hello set känslighet |*trenddetector.sensitivity:* tröskelvärdet för detektor poäng (standard: 3,25, 3,25 – 5 är en rimlig intervallet tooselect från; hello högre hello mindre känslig) |tscore: flytande tal som representerar avvikelseidentifiering poäng på trend |
| Förändring av detektorerna | Dubbelriktad nivå ändra detektor |Identifiera både uppåt och nedåt förändring enligt hello känslighet |*bileveldetector.sensitivity:* tröskelvärdet för detektor poäng (standard: 3,25, 3,25 – 5 är en rimlig intervallet tooselect från; hello högre hello mindre känslig) |rpscore: flytande tal som representerar avvikelseidentifiering poäng på förändring av uppåt och nedåt | |

### <a name="parameters"></a>Parametrar
Mer detaljerad information om dessa indataparametrar anges i hello nedan:

| Indataparametrar | Beskrivning | Standardinställningen | Typ | Giltigt intervall | Föreslagna intervallet |
| --- | --- | --- | --- | --- | --- |
| detectors.historyWindow |Historik (i antal datapunkter) som används för avvikelseidentifiering poäng beräkning |500 |heltal |10-2000 |Tidsserie beroende |
| detectors.spikesdips | Om toodetect bara ger spikar i diagrammet, endast korta eller båda |Båda |Räkna upp |Båda toppar, korta |Båda |
| bileveldetector.sensitivity |Känslighet för dubbelriktad nivå ändra detektor. |3.25 |dubbla |Ingen |3,25 5 (färre värden innebär mer känslig) |
| trenddetector.sensitivity |Känslighet för positivt trend detektor. |3.25 |dubbla |Ingen |3,25 5 (färre värden innebär mer känslig) |
| tspikedetector.sensitivity |Känslighet för TSpike detektor |3 |heltal |1-10 |3-5 (färre värden innebär mer känslig) |
| zspikedetector.sensitivity |Känslighet för ZSpike detektor |3 |heltal |1-10 |3-5 (färre värden innebär mer känslig) |
| postprocess.tailRows |Antal hello senaste data pekar toobe sparas i hello utdataresultat |0 |heltal |0 (Håll alla datapunkter), eller ange antal punkter tookeep i resultaten |Saknas |

### <a name="output"></a>Resultat
hello API körs alla detektorerna på gång series-data och returnerar avvikelseidentifiering resultat och binära topp indikatorer för varje punkt i tiden. hello tabellen nedan visas utdata från hello API. 

| utdata | Beskrivning |
| --- | --- |
| Tid |Tidsstämplar från rådata och aggregerade (eller) beräknade data om aggregering (eller) saknas data uppräkning tillämpas |
| Data |Värden från rådata och aggregerade (eller) beräknade data om aggregering (eller) saknas data uppräkning tillämpas |
| TSpike |Binärt värde tooindicate om en topp identifieras av TSpike detektor |
| ZSpike |Binärt värde tooindicate om en topp identifieras av ZSpike detektor |
| rpscore |En flytande tal som representerar avvikelseidentifiering poängsätta på dubbelriktad förändring |
| rpalert |1/0-värde som anger har en dubbelriktad ändra avvikelseidentifiering baserat på hello inkommande känslighet |
| tscore |En flytande tal som representerar avvikelseidentifiering poängsätta på positivt trend |
| talert |1/0-värde som anger det är en positiv trend avvikelseidentifiering baserat på hello inkommande känslighet |

## <a name="scorewithseasonality-api"></a>ScoreWithSeasonality API
Hej ScoreWithSeasonality API används för att köra avvikelseidentifiering på tidsserier som har säsongsbaserade mönster. Detta API är användbara toodetect avvikelser i säsongsbaserade mönster.  
hello visar följande bild ett exempel på avvikelser som identifierats i en tillfällig tidsserier. hello tidsserier har en topp (hello 1 svart punkt), två korta (hello 2 svart punkt och en hello slutet) och en förändring (röd punkt). Observera att både hello dip hello mitten av hello tidsserier och hello förändring är endast discernable efter när komponenterna tas bort från hello-serien.
![Säsongsvärdet API][2]

### <a name="detectors"></a>Detektorerna
hello detektorerna i hello säsongsvärdet slutpunkt är liknande toohello dem i hello icke säsongsvärdet slutpunkt, men med något annorlunda parameternamn (nedan).

### <a name="parameters"></a>Parametrar

Mer detaljerad information om dessa indataparametrar anges i hello nedan:

| Indataparametrar | Beskrivning | Standardinställningen | Typ | Giltigt intervall | Föreslagna intervallet |
| --- | --- | --- | --- | --- | --- |
| preprocess.aggregationInterval |Aggregeringsintervall i sekunder för sammanställningen indata tidsserier |0 (Ingen aggregering utförs) |heltal |0: annars hoppa över aggregering, > 0 |5 minuter too1 dag, tidsserier beroende |
| preprocess.aggregationFunc |Funktion som används för datainsamling till hello anges AggregationInterval |medelvärde |Räkna upp |medelvärde, sum, längd |Saknas |
| preprocess.replaceMissing |Värden används tooimpute data som saknas |lkv (senaste kända värdet) |Räkna upp |noll, lkv, medelvärde |Saknas |
| detectors.historyWindow |Historik (i antal datapunkter) som används för avvikelseidentifiering poäng beräkning |500 |heltal |10-2000 |Tidsserie beroende |
| detectors.spikesdips | Om toodetect bara ger spikar i diagrammet, endast korta eller båda |Båda |Räkna upp |Båda toppar, korta |Båda |
| bileveldetector.sensitivity |Känslighet för dubbelriktad nivå ändra detektor. |3.25 |dubbla |Ingen |3,25 5 (färre värden innebär mer känslig) |
| postrenddetector.sensitivity |Känslighet för positivt trend detektor. |3.25 |dubbla |Ingen |3,25 5 (färre värden innebär mer känslig) |
| negtrenddetector.sensitivity |Känslighet för negativa utvecklingen detektor. |3.25 |dubbla |Ingen |3,25 5 (färre värden innebär mer känslig) |
| tspikedetector.sensitivity |Känslighet för TSpike detektor |3 |heltal |1-10 |3-5 (färre värden innebär mer känslig) |
| zspikedetector.sensitivity |Känslighet för ZSpike detektor |3 |heltal |1-10 |3-5 (färre värden innebär mer känslig) |
| seasonality.enable |Om säsongsvärdet analysis toobe utförs |SANT |Booleskt värde |SANT, FALSKT |Tidsserie beroende |
| seasonality.numSeasonality |Maximalt antal periodiska cykler toobe upptäcktes |1 |heltal |1, 2 |1-2 |
| seasonality.transform |Om säsongsbaserade (och) trend komponenter bör tas bort innan du tillämpar avvikelseidentifiering |deseason |Räkna upp |Ingen, deseason deseasontrend |Saknas |
| postprocess.tailRows |Antal hello senaste data pekar toobe sparas i hello utdataresultat |0 |heltal |0 (Håll alla datapunkter), eller ange antal punkter tookeep i resultaten |Saknas |

### <a name="output"></a>Resultat
hello API körs alla detektorerna på gång series-data och returnerar avvikelseidentifiering resultat och binära topp indikatorer för varje punkt i tiden. hello tabellen nedan visas utdata från hello API. 

| utdata | Beskrivning |
| --- | --- |
| Tid |Tidsstämplar från rådata och aggregerade (eller) beräknade data om aggregering (eller) saknas data uppräkning tillämpas |
| OriginalData |Värden från rådata och aggregerade (eller) beräknade data om aggregering (eller) saknas data uppräkning tillämpas |
| ProcessedData |Något av följande hello: <ul><li>Säsongsvis justeras tidsserier om betydande säsongsvärdet har identifierats och deseason alternativet;</li><li>säsongsvis justeras och detrended tidsserier om betydande säsongsvärdet har upptäckts och deseasontrend alternativ som valts</li><li>i annat fall är detta hello samma som OriginalData</li> |
| TSpike |Binärt värde tooindicate om en topp identifieras av TSpike detektor |
| ZSpike |Binärt värde tooindicate om en topp identifieras av ZSpike detektor |
| BiLevelChangeScore |En flytande tal som representerar avvikelseidentifiering poängsätta på nivån ändring |
| BiLevelChangeAlert |1/0 värde som anger det är en förändring av avvikelseidentifiering baserat på hello inkommande känslighet |
| PosTrendScore |En flytande tal som representerar avvikelseidentifiering poängsätta på positivt trend |
| PosTrendAlert |1/0-värde som anger det är en positiv trend avvikelseidentifiering baserat på hello inkommande känslighet |
| NegTrendScore |En flytande tal som representerar avvikelseidentifiering poängsätta på negativa utvecklingen |
| NegTrendAlert |1/0-värde som anger det är en negativ trend avvikelseidentifiering baserat på hello inkommande känslighet |

[1]: ./media/machine-learning-apps-anomaly-detection-api/anomaly-detection-score.png
[2]: ./media/machine-learning-apps-anomaly-detection-api/anomaly-detection-seasonal.png

