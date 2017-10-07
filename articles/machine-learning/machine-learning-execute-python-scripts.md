---
title: "aaaExecute Python maskininlärning skript | Microsoft Docs"
description: "Ger en översikt över utforma principerna stöd för Python-skript i Azure Machine Learning och grundläggande Användningsscenarier, funktioner och begränsningar."
keywords: "Python maskininlärning pandas, python pandas, python-skript för körning av python-skript"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: ee9eb764-0d3e-4104-a797-19fc29345d39
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: bradsev
ms.openlocfilehash: 8d23aaa972a46cb1a07ea0f18cc1e24933fe3e6f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="execute-python-machine-learning-scripts-in-azure-machine-learning-studio"></a>Kör skript för Python-maskininlärning i Azure Machine Learning Studio

Det här avsnittet beskriver hello hello aktuella stöd för Python-skript i Azure Machine Learning för design-principerna. hello huvudsakliga funktionerna som också beskrivs, inklusive:

- köra grundläggande Användningsscenarier
- resultatet av ett experiment i en webbtjänst
- stöd för att importera befintlig kod
- Exportera visualiseringar
- utföra övervakat Funktionsurval
- Förstå vissa begränsningar

[Python](https://www.python.org/) är ett ovärderligt verktyg för hello verktyget bröstet många data forskare. Det har:

* en elegant och koncis syntax 
* plattformsoberoende stöd 
* en omfattande uppsättning kraftfulla bibliotek och 
* mogen utvecklingsverktyg. 

Python som används i alla faser av ett arbetsflöde som vanligtvis används i machine learning modellering:

- mata in data och bearbetning 
- funktionen konstruktion
- modell-utbildning 
- modellverifiering
- distribution av hello modeller

Azure Machine Learning Studio stöder inbäddning Python-skript till olika delar av en dator learning experiment och publicera dem också sömlöst som webbtjänster på Microsoft Azure.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## <a name="design-principles-of-python-scripts-in-machine-learning"></a>Designprinciperna för Python-skript i Machine Learning

hello primära gränssnittet tooPython i Azure Machine Learning Studio är via hello [köra Python-skriptet] [ execute-python-script] modulen som visas i bild 1.

![image1](./media/machine-learning-execute-python-scripts/execute-machine-learning-python-scripts-module.png)

![image2](./media/machine-learning-execute-python-scripts/embedded-machine-learning-python-script.png)

Bild 1. Hej **köra Python-skriptet** modul.

Hej [köra Python-skriptet] [ execute-python-script] modul i Azure ML Studio accepterar in toothree indata och ger dig tootwo utdata (beskrivs i följande avsnitt hello) som dess R-analog hello [ Köra R-skriptet] [ execute-r-script] modul. hello Python-kod toobe utförs registreras i hello parameterrutan som ett särskilt namngiven startpunkten anropade funktionen `azureml_main`. Här följer hello viktiga designbeslut principer används tooimplement för den här modulen:

1. *Måste vara idiomatiska för Python-användare.* De flesta Python användare factor koden som funktioner i moduler. Placera så mycket körbara instruktioner i en översta modul är relativt ovanligt. Därför använder hello skript kryssrutan också en särskild Python-funktion som skillnad från toojust en sekvens av rapporter. hello objekt visas i hello-funktionen är Standardtyper för Python-bibliotek som [Pandas](http://pandas.pydata.org/) dataramar och [NumPy](http://www.numpy.org/) matriser.
2. *Måste ha exakt mellan lokala och molntjänster körningar.* hello backend används tooexecute hello Python-kod är baserad på [Anaconda](https://store.continuum.io/cshop/anaconda/)en vanlig plattformsoberoende vetenskapliga Python-distribution. Den innehåller Stäng too200 hello vanligaste Python-paket. Därför dataanalytiker felsöka och kvalificera koden på sina lokala Azure Machine Learning-kompatibla Anaconda-miljön. Använd en befintlig utvecklingsmiljö som [IPython](http://ipython.org/) bärbar dator eller [Python Tools för Visual Studio](http://aka.ms/ptvs), toorun det som en del av ett experiment i Azure ML. Hej `azureml_main` startadressen är en vanliga Python-funktion och kan därför *** kan skapas utan Azure ML-specifik kod eller hello SDK är installerat.
3. *Måste vara sömlöst sammanställningsbara med andra Azure Machine Learning-moduler.* Hej [köra Python-skriptet] [ execute-python-script] modulen accepterar, som indata och utdata, standard Azure Machine Learning datauppsättningar. hello underliggande ramverk få transparent och effektivt sätt hello Azure ML och Python körningar. Så kan Python användas tillsammans med befintliga Azure ML-arbetsflöden, inklusive de som anropar R och SQLite. Resulterar i att data forskare kan skapa arbetsflöden som:
   * använda Python och Pandas för förbearbetning och rensning
   * feed hello tooa SQL DTS, ansluta till flera datauppsättningar tooform funktioner
   * Träna modeller med hello algoritmer i Azure Machine Learning 
   * utvärdera och efterbearbetning hello resultat med hjälp av R.


## <a name="basic-usage-scenarios-in-ml-for-python-scripts"></a>Grundläggande Användningsscenarier i ML för Python-skript

I det här avsnittet vi en undersökning vissa hello grundläggande användning av hello [köra Python-skriptet] [ execute-python-script] modul. Indata toohello Python-modulen exponeras som Pandas ramar. hello funktionen måste returnera en Pandas data bildruta paketeras i en Python [sekvens](https://docs.python.org/2/c-api/sequence.html) , till exempel en tuppel, lista eller NumPy matris. hello första elementet i den här sekvensen returneras sedan hello första utdataporten för modulen hello. Det här schemat visas i bild 2.

![image3](./media/machine-learning-execute-python-scripts/map-of-python-script-inputs-outputs.png)

Figur 2. Mappning av indataportar tooparameters och returnera värdet toooutput port.

Mer detaljerad semantiken för hur hello portar får mappade tooparameters av hello `azureml_main` funktionen visas i tabell 1:

![image1T](./media/machine-learning-execute-python-scripts/python-script-inputs-mapped-to-parameters.png)

Tabell 1. Mappning av portar toofunction parametrar.

hello mappning mellan portar och funktionsparametrar är namngivna:

- hello den första porten för anslutna är mappade toohello första parameter hello-funktionen. 
- hello andra indata (om ansluten) är mappade toohello andra parametern för hello-funktionen.

Se *Python för dataanalys* (O'Reilly 2012) av västra McKinney för mer information om Python Pandas och hur det kan vara används toomanipulate data effektivt. 


## <a name="translation-of-input-and-output-types"></a>Översättning av inkommande och utgående typer 
Indatauppsättningar i Azure ML är konverterade toodata ramar i Pandas. Utdata dataramar konverteras tillbaka tooAzure ML datauppsättningar. hello följande konverteringar utförs:

1. Sträng och numeriska kolumner konverteras som-är och värden som saknas i en dataset är konverterade too'NA som värden i Pandas. hello samma konvertering som händer på hello sätt tillbaka (NA värdena i Pandas är konverterade toomissing värden i Azure ML).
2. Index angreppsmetoderna i Pandas stöds inte i Azure ML. Alla indata ramar i hello Python-funktionen har alltid ett numeriskt index för 64-bitars från 0 toohello antal rader minus 1. 
3. Azure ML-datauppsättningar kan inte ha dubbletter av kolumnnamn och kolumnnamn som inte är strängar. Om en utgående data ram innehåller icke-numeriska kolumner, hello framework anrop `str` på hello kolumnnamn. På samma sätt några dubbletter av kolumnnamn är automatiskt felaktig tooinsure hello namnen är unika. hello-suffix (2) läggs toohello första dubblett, (3) toohello andra dubblett och så vidare.


## <a name="operationalizing-python-scripts"></a>Operationalizing Python-skript

Alla [köra Python-skriptet] [ execute-python-script] moduler som används i ett bedömningsprofil experiment anropas när publiceras som en webbtjänst. Bild 3 visar till exempel ett bedömningsprofil experiment som innehåller hello kod tooevaluate ett enda Python-uttryck. 

![image4](./media/machine-learning-execute-python-scripts/figure3a.png)

![image5](./media/machine-learning-execute-python-scripts/python-script-with-python-pandas.png)

Bild 3. Webbtjänsten för utvärdering av ett Python-uttryck.

En webbtjänst som skapas från den här experiment:

- tar som indata en Python-uttryck (som sträng)
- skickar det toohello Python-tolkning 
- Returnerar en tabell som innehåller både hello uttryck och hello utvärderas resultat.


## <a name="importing-existing-python-script-modules"></a>Importera befintlig Python skriptmoduler

Ett vanligt användningsområde för många datavetare är tooincorporate befintliga Python-skript i Azure ML experiment. I stället för att kräva att all kod som sammanfogas och klistras in i ett enda skript kryssrutan hello [köra Python-skriptet] [ execute-python-script] modulen accepterar en zip-fil som innehåller Python-moduler på hello tredje indataport. hello-filen är uppackade av hello körning framework vid körning och hello innehållet läggs toohello sökväg till bibliotek för hello Python-tolken. Hej `azureml_main` startpunkten funktion kan sedan importera dessa moduler direkt.

Exempelvis bör du hello filen Hello.py som innehåller en enkel ”Hello, World”-funktionen.

![image6](./media/machine-learning-execute-python-scripts/figure4.png)

Bild 4. Användardefinierad funktion i Hello.py-filen.

Nu ska skapa vi en fil Hello.zip som innehåller Hello.py:

![image7](./media/machine-learning-execute-python-scripts/figure5.png)

Bild 5. ZIP-filen som innehåller användardefinierade Python-kod.

Ladda upp hello zip-filen som en datamängd i Azure Machine Learning Studio. Sedan skapar och kör ett experiment som använder hello Python-kod i hello Hello.zip-filen genom att koppla det toohello tredje indataport av hello **köra Python-skriptet** modulen som visas i bilden.

![image8](./media/machine-learning-execute-python-scripts/figure6a.png)

![image9](./media/machine-learning-execute-python-scripts/figure6b.png)

Bild 6. Exempelexperimentet med användardefinierade Python-kod överförs som en zip-fil.

hello modulen utdata visar att hello zip-filen har anspråksregellogik och hello funktionen `print_hello` har körts.
 
![image10](./media/machine-learning-execute-python-scripts/figure7.png)

Bild 7. Användardefinierad funktion används i hello [köra Python-skriptet] [ execute-python-script] modul.


## <a name="working-with-visualizations"></a>Arbeta med grafik

Områden som skapats med hjälp av MatplotLib som kan vara visualiseras hello webbläsare kan returneras av hello [köra Python-skriptet][execute-python-script]. Men hello områden är inte automatiskt omdirigerade tooimages som de är när du använder R. Så hello användare måste uttryckligen spara alla områden tooPNG filer om de returnerade toobe tillbaka tooAzure Machine Learning. 

toogenerate bilder från MatplotLib, måste du utföra hello nedan:

* Växla hello serverdelen för ”%{AGG/” från hello standard Qt-baserade renderare 
* Skapa ett nytt objekt i bild 
* Hämta hello axel och generera alla områden i det. 
* Spara hello bild tooa PNG-fil 

Den här processen illustreras i följande figur 8 som skapar en punktdiagram ritytans matris med hello scatter_matrix funktionen i Pandas hello.

![image1v](./media/machine-learning-execute-python-scripts/figure-v1-8.png)

Figur 8. Felkod toosave MatplotLib förekommer tooimages.

Bild 9 illustrerar ett experiment som använder såg tidigare tooreturn ritas via hello andra utdataporten hello-skript.

![image2v](./media/machine-learning-execute-python-scripts/figure-v2-9a.png) 

![image2v](./media/machine-learning-execute-python-scripts/figure-v2-9b.png) 

Bild 9. Visualisera områden som genereras av Python-kod.

Det är möjligt tooreturn flera siffror genom att spara dem i olika bilder, hello Azure Machine Learning-körning tar upp alla bilder och sammanfogar dem för visualisering.


## <a name="advanced-examples"></a>Avancerade exempel

Hej Anaconda environment har installerats i Azure Machine Learning innehåller vanliga paket som NumPy, SciPy och Läs Scikits. Dessa paket kan effektivt användas för olika aktiviteter för databehandling i machine learning-pipeline. Exempelvis hello följande experiment och skript visar hello användning av ensemble deltagarna i Scikits Läs toocompute funktionen vikten resultat för en dataset. hello resultat kan vara används tooperform övervakad Funktionsurval innan som matas in en annan ML-modell.

Här är hello Python funktion som används för toocompute hello vikten resultat och ordning hello funktioner baserat på hello resultat:

![image11](./media/machine-learning-execute-python-scripts/figure8.png)

Bild 10. Funktionen toorank funktioner av resultat.
 hello följande experiment sedan beräknar och returnerar hello vikten resultat av funktioner i hello ”Pima indiska Diabetes” dataset i Azure Machine Learning:

![image12](./media/machine-learning-execute-python-scripts/figure9a.png)
![image13](./media/machine-learning-execute-python-scripts/figure9b.png)    

Figur 11. Experiment toorank funktioner i hello Pima indiska Diabetes dataset.

## <a name="limitations"></a>Begränsningar
Hej [köra Python-skriptet] [ execute-python-script] har för närvarande hello följande begränsningar:

1. *Begränsade körning.* hello Python-körning är för närvarande i begränsat läge och därför tillåter inte åtkomst toohello nätverks- eller toohello lokala filsystemet på ett beständigt sätt. Alla filer som sparats lokalt isolerats och tas bort när hello modulen har slutförts. hello Python-kod kan inte komma åt de flesta kataloger på hello datorn körs på, hello undantag som hello aktuell katalog och dess underkataloger.
2. *Ingen avancerade utveckling och felsökning support.* hello Python-modulen stöder för närvarande inte IDE-funktioner, till exempel intellisense och felsökning. Om hello-modulen misslyckas vid körning, är hello fullständig Python stack-spårning också tillgänglig. Men den visas i loggen för hello utdata för hello-modulen. Vi rekommenderar att utveckla och Felsök Python-skript i en miljö, till exempel IPython och sedan importera hello kod till hello-modulen för närvarande.
3. *Enskild ram utdata.* hello Python startadressen är endast tillåtna tooreturn en enda data-ram som utdata. Det är inte för närvarande kan tooreturn godtycklig Python objekt som tränade modeller direkt tillbaka toohello Azure Machine Learning runtime. Som [köra R-skriptet][execute-r-script], som har hello samma begränsningar som det är möjligt i många fall toopickle objekt till en byte array och gå sedan tillbaka som inuti en data-ram.
4. *När det gäller toocustomize Python installation*. Är för närvarande hello endast sätt tooadd anpassad Python-moduler via hello zip-filen mekanism som beskrivs ovan. Detta är möjligt för små moduler, är det besvärlig för stora moduler (särskilt de med inbyggda DLL-filer) eller ett stort antal moduler. 

## <a name="conclusions"></a>Slutsatser
Hej [köra Python-skriptet] [ execute-python-script] modulen gör en data-forskare tooincorporate befintlig Python-kod till molnbaserade machine learning arbetsflöden i Azure Machine Learning och tooseamlessly operationalisera dem som en del av en webbtjänst. hello Python skriptmodul samverkar naturligt med andra moduler i Azure Machine Learning. hello modul kan användas för ett antal uppgifter från utforskning toopre databehandling och extrahering av funktionen, och sedan tooevaluation och efter bearbetningen av hello resultat. hello backend-körningsmiljön som används för körning av baseras på Anaconda, en väl testade och vanliga Python-distribution. Den här backend gör det enkelt tooon board befintlig kod tillgångar i hello moln.

Vi räknar tooprovide ytterligare funktioner toohello [köra Python-skriptet] [ execute-python-script] modul som hello möjlighet tootrain och operationalisera modeller i Python och tooadd bättre stöd för hello utveckling och felsöka kod i Azure Machine Learning Studio.

## <a name="next-steps"></a>Nästa steg
Mer information finns i hello [Python Developer Center](https://azure.microsoft.com/develop/python/).

<!-- Module References -->
[execute-python-script]: https://msdn.microsoft.com/library/azure/cdb56f95-7f4c-404d-bde7-5bb972e6f232/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
