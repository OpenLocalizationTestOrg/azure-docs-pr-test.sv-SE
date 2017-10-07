---
title: "aaaAzure Machine Learning vanliga frågor (FAQ) | Microsoft Docs"
description: "Introduktion till Azure Machine Learning: Vanliga frågor och svar om fakturering, funktioner och begränsningar i en molntjänst för effektiv förutsägelsemodellering."
keywords: "introduktion till maskininlärning, förutsägelsemodellering, vad är maskininlärning, machine learning"
services: machine-learning
documentationcenter: 
author: garyericson
manager: paulettm
editor: cgronlun
ms.assetid: a4a32a06-dbed-4727-a857-c10da774ce66
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/02/2017
ms.author: garye
ms.openlocfilehash: 3af84451dde064c3c9520ee520b541128b1eef92
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-machine-learning-frequently-asked-questions-billing-capabilities-limitations-and-support"></a>Vanliga frågor och svar om Azure Machine Learning: Fakturering, funktioner, begränsningar och support
Här får du svar på vanliga frågor om molntjänsten Azure Machine Learning som tillhandahåller förutsägelsemodeller och operationaliseringslösningar genom webbtjänster. Dessa vanliga frågor och svar innehåller frågor om hur toouse hello-tjänsten, som innehåller hello fakturering modellen, funktioner, begränsningar och support.

**Har en fråga som du inte kan hitta här?**

Azure Machine Learning har ett forum på MSDN där medlemmar i hello datavetenskap gemenskapen kan ställa frågor om Azure Machine Learning. hello Azure Machine Learning-teamet övervakar hello forum. Gå toohello [Azure Machine Learning-forumet](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) toosearch för svar eller toopost en ny fråga.

## <a name="general-questions"></a>Allmänna frågor
**Vad är Azure Machine Learning?**

Azure Machine Learning är en helt hanterad tjänst att du kan använda toocreate, testa, använda och hantera förutsägelseanalyslösningar i molnet hello. Du kan logga in, ladda upp data och genast starta maskininlärningsexperiment. Allt du behöver är en webbläsare. Förutsägelsemodellering med dra-och-släpp-funktioner, ett stort utbud av modeller och ett bibliotek med startmallar gör det enkelt att snabbt utföra vanliga maskininlärningsaktiviteter. Mer information finns i hello [översikt över Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/). En introduktion toomachine inlärning som förklarar viktiga termer och begrepp finns [introduktion tooAzure Machine Learning](machine-learning-what-is-machine-learning.md).

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**Vad är Machine Learning Studio?**

Machine Learning Studio är en arbetsstationsmiljö som du kommer åt via en webbläsare. Machine Learning Studio tillhandahåller ett utbud av moduler i visuella gränssnitt som hjälper dig att skapa en slutpunkt till slutpunkt, datavetenskap arbetsflöde i hello form av ett experiment.

Mer information om Machine Learning Studio finns i [Vad är Machine Learning Studio?](machine-learning-what-is-ml-studio.md)

**Vad är hello Machine Learning API-tjänsten?**

hello Machine Learning API-tjänsten kan du toodeploy förutsägelsemodeller som de som är inbyggda i Machine Learning Studio som skalbara, feltoleranta webbtjänster. hello-webbtjänster som skapar hello Machine Learning API-tjänsten är REST API: er som tillhandahåller ett gränssnitt för kommunikation mellan externa program och dina förutsägelseanalysmodeller.

Mer information finns i [hur tooconsume en Azure Machine Learning-webbtjänst](machine-learning-consume-web-services.md).

**Var visas mina klassiska webbtjänster? Var visas mina nya Azure Resource Manager-baserade webbtjänster?**

Webbtjänsterna som skapas med hjälp av hello klassisk distribution modell och web services, skapas med hello nya Azure Resource Manager-distributionsmodellen visas i hello [Microsoft Azure Machine Learning-webbtjänster](https://services.azureml.net/) portal.

Klassiska webbtjänster visas också i [Machine Learning Studio](http://studio.azureml.net) på hello **webbtjänster** fliken.

## <a name="azure-machine-learning-questions"></a>Frågor om Azure Machine Learning
**Vad är Azure Machine Learning-webbtjänster?**

Machine Learning-webbtjänster är ett gränssnitt mellan ett program och en bedömningsmodell för ett Machine Learning-arbetsflöde. Ett externt program kan använda Azure Machine Learning toocommunicate med en Machine Learning arbetsflöde bedömningsprofil modell i realtid. Ett anrop tooa Machine Learning-webbtjänst returnerar förutsägelse resultat tooan externt program. toomake en webbtjänst för anropet tooa du skickar en API-nyckel som skapades när du har distribuerat hello-webbtjänsten. En Machine Learning-webbtjänst baseras på REST, ett populärt arkitekturval för programmeringsprojekt.

Azure Machine Learning har två typer av webbtjänster:

* Svar på begäranden tjänst (RR): En låg latens, mycket skalbar tjänst som tillhandahåller ett gränssnitt toohello tillståndslösa modeller skapas och distribueras med hjälp av Machine Learning Studio.
* BES (Batch Execution Service): En asynkron tjänst som poängsätter en batch med dataposter.

Det finns flera sätt tooconsume hello REST-API och åtkomst hello webbtjänsten. Du kan till exempel skriva ett program i C#, R eller Python med hjälp av hello exempelkod som genereras när du distribuerade hello-webbtjänsten.

hello exempelkod finns på:
- hello förbruka sidan för hello-webbtjänsten i hello Azure Machine Learning-webbtjänster portal
- hello API-hjälpsidan hello web service instrumentpanelen i Machine Learning Studio

Du kan också använda hello exempel Microsoft Excel-arbetsbok som har skapats för dig och är tillgänglig i hello web service instrumentpanelen i Machine Learning Studio.

**Vad är hello huvudsakliga uppdateringar tooAzure Machine Learning?**

Hello senaste uppdateringar, se [vad är nytt i Azure Machine Learning](machine-learning-whats-new.md).

## <a name="machine-learning-studio-questions"></a>Frågor om Machine Learning Studio
### <a name="import-and-export-data-for-machine-learning"></a>Importera och exportera data för Machine Learning
**Vilka datakällor stöder Machine Learning?**

Du kan hämta data tooa Machine Learning Studio-experiment på tre sätt:

- Överföra en lokal fil som en datauppsättning
- Använda en modul tooimport data från datatjänster i molnet
- Importera en datamängd som sparats i ett annat experiment

Mer om toolearn filformat som stöds, se [importera träningsdata till Machine Learning Studio](machine-learning-data-science-import-data.md).

#### <a id="ModuleLimit"></a>Hur stor kan datauppsättningen hello vara för Mina moduler?
Modulerna i Machine Learning Studio stöder datauppsättningar på upp too10 GB med kompakta numeriska data för vanliga användningsfall. Om en modul hämtar indata från mer än en, är hello 10 GB-värde hello summan av alla indata. Du kan också ta prover av större datauppsättningar med frågor från Hive eller Azure SQL Database, eller så kan du bearbeta data i förväg med modulen Inlärning med antal före införandet.  

hello följande typer av data kan expandera toolarger datauppsättningar under funktionsnormalisering och är begränsat tooless än 10 GB:

* Utspridda
* Kategoriska
* Strängar
* Binära data

hello följande moduler är begränsade toodatasets som är mindre än 10 GB:

* Moduler för rekommenderare
* Modulen SMOTE (Synthetic Minority Oversampling Technique)
* Skriptmoduler: R, Python, SQL
* Moduler där hello utgående datastorleken kan vara större än inkommande datastorleken, till exempel koppling eller funktions-hashning
* Korsvalidering, Hyperparametrar för Justeringsmodeller, Ordningstalsregression och en eller alla Multiclass när hello antalet iterationer är mycket stora

#### <a id="UploadLimit"></a>Vad är hello gränser för data överför?
För datauppsättningar som är större än ett par GB, ladda upp data tooAzure Storage eller Azure SQL Database eller använda Azure HDInsight i stället för att ladda upp direkt från en lokal fil.

**Kan jag läsa data från Amazon S3?**

Om du har en liten mängd data och vill tooexpose den via en HTTP-URL och du kan använda hello [importera Data] [ import-data] modul. För större mängder data, överför den tooAzure Storage först och sedan använda hello [importera Data] [ import-data] modulen toobring det i experimentet.
<!--

<SEE CLOUD DS PROCESS>
-->

**Finns det inbyggda bildinhämtningsfunktioner?**

Du kan lära dig om bildinhämtningsfunktioner i hello [importera bilder] [ image-reader] referens.

### <a name="modules"></a>Moduler
**hello-algoritmen, datakällan, dataformatet eller datatransformeringsåtgärden som jag letar efter finns inte i Azure Machine Learning Studio. Vad har jag för alternativ?**

Du kan gå toohello [Användarfeedback](http://go.microsoft.com/fwlink/?LinkId=404231) toosee funktionen begär vi följer upp. Lägg till din röst tooa begäran om en funktion som du letar efter redan har begärts. Skapa en ny begäran om hello-funktion som du letar efter inte finns. Du kan visa hello status för din begäran i det här forumet för. Vi spårar den här listan noggrant och uppdatera hello statusen för funktionstillgänglighet ofta. Du kan dessutom använda hello inbyggt stöd för R och Python toocreate anpassade transformeringar vid behov.

**Kan jag använda min befintliga kod i Machine Learning Studio?**

Ja, kan du hämta din befintliga R eller Python-kod till Machine Learning Studio, köra den i hello samma experimentera med Azure Machine Learning-inlärning och distribuera hello lösningen som en webbtjänst via Azure Machine Learning. Mer information finns i [Utöka ditt experiment med R](machine-learning-extend-your-experiment-with-r.md) och [Köra Python-maskininlärningsskript i Azure Machine Learning Studio](machine-learning-execute-python-scripts.md).

**Är det möjligt toouse liknande [PMML](http://en.wikipedia.org/wiki/Predictive_Model_Markup_Language) toodefine en modell?**

Nej, PMML (Predictive Model Markup Language) stöds inte. Du kan använda anpassade R och Python code toodefine en modul.

**Hur många moduler kan jag köra parallellt i mitt experiment?**  

Du kan köra upp toofour moduler parallellt i ett experiment.

### <a name="data-processing"></a>Databearbetning
**Finns det en möjlighet toovisualize data (utöver R-visualiseringar) interaktivt i experimentet hello?**

Klicka på hello utdata från en modul toovisualize hello data och få statistik.

**När jag förhandsgranskar resultatet eller data i en webbläsare, begränsas hello antalet rader och kolumner. Varför?**

Eftersom stora mängder data kan skickas tooa webbläsare, är datastorleken begränsad tooprevent Machine Learning Studio blir långsamt. toovisualize alla Hej data/resultat, bättre toodownload hello data och använda Excel eller något annat verktyg.

### <a name="algorithms"></a>Algoritmer
**Vilka befintliga algoritmer stöds i Machine Learning Studio?**

Machine Learning Studio tillhandahåller avancerade algoritmer som skalbara förstärkta beslutsträd, Bayesian Recommendation-system, djupa neurala nätverk och beslutsdjungler utvecklade av Microsoft Research. Skalbara maskininlärningspaket med öppen källkod, som Vowpal Wabbit, ingår också. Machine Learning Studio stöder maskininlärningsalgoritmer för multiklass-baserad och binär klassificering, regression och kluster. Visa hello fullständig lista över [Machine Learning-moduler][machine-learning-modules].

**Föreslå hello rätt Machine Learning algoritmen toouse för Mina data automatiskt?**

Nej, men Machine Learning Studio har olika sätt toocompare hello resultatet av varje algoritm toodetermine hello rätt typ för ditt problem.

**Har du några riktlinjer för att välja en algoritm framför en annan för hello tillhandahålls algoritmer?**

Se [hur toochoose en algoritm](machine-learning-algorithm-choice.md).

**Är hello tillhandahålls algoritmerna skrivna i R eller Python?**

Nej, dessa algoritmer är huvudsakligen skrivna i kompilerade språk tooprovide bättre prestanda.

**Finns det någon information av hello algoritmerna?**

hello-dokumentationen finns information om hello algoritmer och parametrar för att finjustera är beskrivs toooptimize hello algoritm för din användning.  

**Finns det stöd för onlineinlärning?**

Nej, för närvarande stöds endast omträning via programmering.

**Kan jag visualisera hello lager i en Neuronätverksmodell med hello inbyggda modulen?**

Nej.

**Kan jag skapa egna moduler i C# eller på ett annat språk?**

För närvarande kan du bara använda R toocreate nya anpassade moduler.

### <a name="r-module"></a>R-modulen
**Vilka R-paket är tillgängliga i Machine Learning Studio?**

Stöder Machine Learning Studio över 400 R CRAN-paket idag här är hello [aktuella listan](http://az754797.vo.msecnd.net/docs/RPackages.xlsx) för alla paket som ingår. Se även [Utöka ditt experiment med R](machine-learning-extend-your-experiment-with-r.md) toolearn hur tooretrieve den här listan själv. Om hello-paket som ska inte visas i listan anger hello namnet på hello-paketet på hello [Användarfeedback](http://go.microsoft.com/fwlink/?LinkId=404231).

**Är det möjligt toobuild en anpassad R-modul?**

Ja. Mer information finns i [Redigera anpassade R-moduler i Azure Machine Learning](machine-learning-custom-r-modules.md).

**Finns det en REPL-miljö för R?**

Nej, det finns ingen Läs-utvärdering-utskrifts-Loop (REPL)-miljö för R i studio hello.

### <a name="python-module"></a>Python-modulen
**Det är möjligt toobuild en anpassad Python-modul?**

För närvarande inte, men du kan använda en eller flera [köra Python-skriptet] [ python] moduler tooget hello samma resultat.

**Finns det en REPL-miljö för Python?**

Du kan använda hello Jupyter Notebooks i Machine Learning Studio. Mer information finns i [Introduktion till Jupyter Notebooks i Azure Machine Learning Studio](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).

## <a name="web-service"></a>Webbtjänst
### <a name="retrain"></a>Omträning
**Hur tränar jag om Azure Machine Learning-modeller via programmering?**

Använd hello omtränings-API: er. Mer information finns i [Träna om Machine Learning-modeller via programmering](machine-learning-retrain-models-programmatically.md). Exempelkod är också tillgängligt i hello [Microsoft Azure Machine Learning Omtränings-demonstration](https://azuremlretrain.codeplex.com/).

### <a name="create"></a>Skapa
**Kan jag distribuera hello modellen lokalt eller i ett program som inte har en Internetanslutning?**

Nej.

**Finns det en typisk svarstid som kan förväntas för alla webbtjänster?**

Se hello [Azure-prenumerationsbegränsningar](../azure-subscription-service-limits.md).

### <a name="use"></a>Användning
**När ska jag toorun min förutsägelsemodell som en Batch Execution service jämfört med en Request Response-tjänst?**

hello-tjänsten (RR) är låg latens och hög skalning webbtjänst som är används tooprovide ett gränssnitt toostateless modeller som skapas och distribueras från experimentmiljön hello. hello Batch Execution service (BES) är en tjänst som asynkront poäng en batch med poster. hello som indata för BES är t.ex. datainfogning som används i RRS. hello största skillnaden är att BES läser ett block med poster från olika källor, till exempel Azure Blob storage, Azure Table storage, Azure SQL Database, HDInsight (hive-fråga) och HTTP-källor. Mer information finns i [hur tooconsume en Azure Machine Learning-webbtjänst](machine-learning-consume-web-services.md).

**Hur uppdaterar hello modellen för hello distribuerade webbtjänsten?**

tooupdate en förutsägelsemodell för en redan distribuerad tjänst, ändra och köra hello experiment som du använde tooauthor och spara hello tränade modellen. När du har en ny version av hello tränade modellen tillfrågas Machine Learning Studio du om du vill tooupdate webbtjänsten. Mer information om hur tooupdate en distribuerad webbtjänst finns [distribuera en Machine Learning-webbtjänst](machine-learning-publish-a-machine-learning-web-service.md).

Du kan också använda hello Retraining API: er.
Mer information finns i [Träna om Machine Learning-modeller via programmering](machine-learning-retrain-models-programmatically.md). Exempelkod är också tillgängligt i hello [Microsoft Azure Machine Learning Omtränings-demonstration](https://azuremlretrain.codeplex.com/).

**Hur övervakar jag min webbtjänst när den har distribuerats i produktionsmiljön?**

När du har distribuerat en förutsägelsemodell du kan övervaka från hello Azure klassiska portal (endast klassiska web services) eller hello Azure Machine Learning-webbtjänster portal. Varje distribuerad tjänst har sin egen instrumentpanel där du kan se övervakningsinformation för tjänsten. Mer information om hur toomanage distribuerade webbtjänster, se [hantera en webbtjänst med hjälp av hello Azure Machine Learning-webbtjänster portal](machine-learning-manage-new-webservice.md) och [hantera en Azure Machine Learning-arbetsytan](machine-learning-manage-workspace.md).

**Finns det en plats där jag kan se hello utdata för min RRS/bes-tjänst?**

För RRS är hello webbtjänstsvaret vanligtvis där du ser hello resultat. Du kan också skriva det tooAzure Blob storage. För BES skrivs utdata hello tooa blob som standard. Du kan också skriva hello utdata tooa databas eller tabell med hjälp av hello [exportera Data] [ export-data] modul.

**Kan jag bara skapa webbtjänster från modeller som skapats i Machine Learning Studio?**

Nej, du kan också skapa webbtjänster direkt med Jupyter Notebooks och RStudio.

**Var hittar jag information om felkoder?**

En lista med felkoder och beskrivningar finns i [Felkoder för Machine Learning-moduler](https://msdn.microsoft.com/library/azure/dn905910.aspx).

## <a name="scalability"></a>Skalbarhet
**Vad är hello skalbarhet hello webbtjänsten?**

För närvarande etablerad hello standardslutpunkten med 20 samtidiga RRS-förfrågningar per slutpunkt. Du kan skala detta too200 samtidiga förfrågningar per slutpunkt och du kan skala varje web service too10, 000 slutpunkter per webbtjänst som beskrivs i [skalning en webbtjänst](machine-learning-scaling-webservice.md). För BES kan varje slutpunkt bearbeta 40 begäranden i taget. Ytterligare begäranden utöver dessa placeras i kö. Dessa köade förfrågningar körs automatiskt när hello kön krymper.

**Fördelas R-jobb mellan noder?**

Nej.  

**Hur mycket data kan jag använda för träning?**

Modulerna i Machine Learning Studio stöder datauppsättningar på upp too10 GB med kompakta numeriska data för vanliga användningsfall. Om en modul hämtar mer än ett inkommande, hello totala är storleken för alla indata 10 GB. Du kan också ta prov på större datauppsättningar via Hive-frågor, via Azure SQL Database-frågor eller genom att bearbeta data i förväg med modulen [Inlärning med antal][counts] före införandet.  

hello följande typer av data kan expandera toolarger datauppsättningar under funktionsnormalisering och är begränsat tooless än 10 GB:

* Utspridda
* Kategoriska
* Strängar
* Binära data

hello följande moduler är begränsade toodatasets som är mindre än 10 GB:

* Moduler för rekommenderare
* Modulen SMOTE (Synthetic Minority Oversampling Technique)
* Skriptmoduler: R, Python, SQL
* Moduler där hello utgående datastorleken kan vara större än inkommande datastorleken, till exempel koppling eller funktions-hashning
* Korsvalidering, hyperparametrar för justeringsmodeller, ordningstalsregression och ”en eller alla”-multiklasser, om antalet iterationer är mycket stort

För datauppsättningar som är större än några få GB, ladda upp data tooAzure Storage eller Azure SQL Database eller använda HDInsight i stället för att ladda upp direkt från en lokal fil.

**Finns det några begränsningar vad gäller vektorstorlek?**

Rader och kolumner har varje begränsad toohello .NET begränsning av Max Int: 2 147 483 647.

**Kan jag justera hello storleken på hello virtuell dator som kör hello webbtjänsten?**

Nej.  

## <a name="security-and-availability"></a>Säkerhet och tillgänglighet
**Vem som kan komma åt hello http-slutpunkten för hello-webbtjänsten som standard? Hur jag för att begränsa åtkomst toohello slutpunkten?**

När en webbtjänst har distribuerats skapas en standardslutpunkt för tjänsten. hello standardslutpunkten kan anropas med dess API-nyckel. Du kan lägga till fler slutpunkter med sina egna nycklar från hello klassiska Azure-portalen eller programmässigt med hjälp av hello Web Service Management API: er. Åtkomstnycklar behövs toomake samtal har toohello webbtjänsten. Mer information finns i [hur tooconsume en Azure Machine Learning-webbtjänst](machine-learning-consume-web-services.md).

**Vad händer om det inte går att hitta mitt Azure-lagringskonto?**

Machine Learning Studio förlitar sig på ett användardefinierat Azure storage-konto toosave mellanliggande data när hello arbetsflödet körs. Det här lagringskontot tillhandahålls tooMachine Learning Studio när en arbetsyta skapas. Efter hello skapas arbetsyta om hello storage-konto har tagits bort och kan inte längre hittas hello arbetsytan slutar att fungera och alla försök att arbetsytan misslyckas.

Om du av misstag tas bort hello lagringskonto återskapa hello storage-konto med samma namn i hello hello samma region som hello bort lagringskontot. Efter det att synkronisera hello snabbtangent.

**Vad händer om åtkomstnyckeln för mitt lagringskonto inte är synkroniserat?**

Machine Learning Studio förlitar sig på ett användardefinierat Azure storage-konto toostore mellanliggande data när hello arbetsflödet körs. Det här lagringskontot tillhandahålls tooMachine Learning Studio när en arbetsyta skapas och hello snabbtangenter är associerade med den arbetsytan. Om hello åtkomstnycklarna ändras efter hello arbetsytan har skapats, hello arbetsytan inte längre komma åt hello storage-konto. Det slutar fungera och alla experiment i arbetsytan misslyckas.

Om du har ändrat åtkomst lagringskontonycklar hello omsynkronisering hello snabbtangenter hello arbetsytan med klassiska Azure-portalen.  

## <a name="support-and-training"></a>Support och utbildning
**Var kan jag lära mig mer om Azure Machine Learning?**

Hej [Azure Machine Learning Documentation Center](https://azure.microsoft.com/services/machine-learning/) värd hittar du videokurser och hur tooguides. Dessa stegvisa guider införa hello tjänster och förklarar hello datalivscykeln för import av data, rensning, utveckling av förutsägelsemodeller och distribuera dem i produktion med hjälp av Azure Machine Learning.

Vi lägga till nytt material toohello Machine Learning Center med jämna mellanrum. Du kan skicka en begäran om ytterligare utbildningsmaterial på Machine Learning Center på hello [Användarfeedback](https://windowsazure.uservoice.com/forums/257792-machine-learning).

Du kan också hitta kurser på [Microsoft Virtual Academy](http://www.microsoftvirtualacademy.com/training-courses/getting-started-with-microsoft-azure-machine-learning).

**Hur får jag support för Azure Machine Learning?**

tooget teknisk support för Azure Machine Learning går för[Azure stöder](/support/options/), och välj **Maskininlärning**.

Azure Machine Learning har även ett community-forum på MSDN där du kan ställa frågor om Azure Machine Learning. hello Azure Machine Learning-teamet övervakar hello forum. Gå för[Azure-forumet](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning).

## <a name="billing-questions"></a>Frågor om fakturering
**Hur fungerar faktureringen av Machine Learning?**

Azure Machine Learning har två komponenter: Machine Learning Studio och Machine Learning-webbtjänster.

När du utvärderar Machine Learning Studio, kan du använda hello kostnadsfri fakturering nivå. hello kostnadsfria nivån kan du distribuera en klassisk-webbtjänst som har begränsad kapacitet.

Om du väljer att Azure Machine Learning uppfyller dina behov kan du registrera dig för hello standardnivån. toosign in måste du ha en Microsoft Azure-prenumeration.

I hello standardnivån debiteras du varje månad för varje arbetsyta som du definierar i Machine Learning Studio. När du kör ett experiment i studio hello, debiteras du för beräkningsresurser när du kör ett experiment. När du distribuerar en klassisk webbtjänst transaktioner och beräkna timmar debiteras på hello betala per basis.

De nya Resource Manager-baserade webbtjänsterna har faktureringsplaner som gör det lättare att förutse kostnaderna. Nivåindelad priser har rabatterade priser toocustomers som behöver en stor mängd kapacitet.

När du skapar en plan att tooa fast kostnad som medföljer en inkluderade API-beräkningstimmar och API-transaktioner. Om du behöver mer inkluderade kvantiteter kan du lägga till instanser tooyour plan. Om du behöver betydligt större kapacitet kan du välja en plan på en högre nivå med avsevärt mycket större kapacitet och rabatt.

När hello inkluderade kvantiteter i befintliga instanser förbrukas, debiteras ytterligare användning med hello överförbrukning hastighet som är kopplad till hello fakturering plan nivå.

> [!NOTE]
Inkluderade kvantiteter har omfördelats var 30: e dag Återställ, och oanvända inkluderade kvantiteter inte över toohello nästa period.

Mer information om fakturering och priser finns i [Machine Learning-priser](https://azure.microsoft.com/pricing/details/machine-learning/).

**Finns det någon kostnadsfri utvärderingsversion för Machine Learning?**

 Azure Machine Learning har ett kostnadsfritt prenumerationsalternativ som beskrivs i [Machine Learning-priser](https://azure.microsoft.com/pricing/details/machine-learning/). Machine Learning Studio har en utvärderingsversion av åtta timmar snabb utvärdering som är tillgänglig när du loggar in för[Machine Learning Studio](https://studio.azureml.net/?selectAccess=true&o=2).

 När du registrerar dig för en kostnadsfri utvärderingsversion av Azure kan du dessutom prova alla Azure-tjänster i en månad. toolearn mer om hello Azure kostnadsfri utvärderingsversion finns [Azure kostnadsfri utvärderingsversion vanliga frågor och svar](https://azure.microsoft.com/pricing/free-trial-faq/).

**Vad är en transaktion?**

En transaktion representerar ett API-anrop som Azure Machine Learning svarar på. Transaktioner från RRS- (Request-Response Service) och BES-anrop (Batch Execution Service) aggregeras och debiteras mot din faktureringsplan.

**Kan jag använda Transaktionsantal hello som ingår i en plan för både Resursposter och BES transaktioner?**

Ja, dina transaktioner från RRS och BES summeras och debiteras mot din faktureringsplan.

**Vad är en API-beräkningstimme?**

En API beräkning timme är hello fakturering enhet hello gången beräkningsresurser API-anrop vidta toorun med hjälp av Machine Learning. Alla dina anrop summeras för faktureringsändamål.

**Hur lång tid tar ett typiskt API-anrop i produktionsmiljön?**

Tider för produktions-API-anrop kan variera avsevärt, vanligtvis mellan hundratals millisekunder tooa några sekunder. Vissa API-anrop kan kräva minuter beroende på hur hello komplex hello databehandling och maskininlärning modellen. hello bästa sätt tooestimate produktions-API-anrop är toobenchmark en modell på hello tjänsten Machine Learning.

**Vad är en Studio-beräkningstimme?**

En timme för beräkning av Studio är hello fakturering enhet för hello sammanställd tid att experimenten använder beräkningsresurser i studio.

**I den nya (Azure Resource Manager-baserat) web services hello utveckling och testning nivå begreppet för?**

Resource Manager-baserade webbtjänster ange flera nivåer som du kan använda tooprovision faktureringsavtalet. hello ger utveckling och testning prisnivån begränsad, inkluderade kvantiteter som gör att du tootest experimentet som en webbtjänst utan att det medför kostnader. Du har hello möjlighet toosee hur det fungerar.

**Finns det separata lagringskostnader?**

hello Machine Learning kostnadsfria nivån inte kräver eller tillåter separat lagringsutrymme. hello Machine Learning standardnivån kräver användare toohave ett Azure storage-konto. Azure Storage [faktureras separat](https://azure.microsoft.com/pricing/details/storage/).

**Hur stöder Machine Learning arbete som kräver hög tillgänglighet?**

Ja. Mer information finns i [Machine Learning-priser](https://azure.microsoft.com/en-us/pricing/details/machine-learning/) en beskrivning av hello servicenivåavtal (SLA).

**Vilka specifika beräkningsresurser körs mina API-anrop på i produktionsmiljön?**

hello tjänsten Machine Learning är en multitenant. Faktiska beräkningsresurser som används på hello serverdel variera och har optimerats för prestanda och förutsägbarhet.

### <a name="management-of-new-resource-manager-based-web-services"></a>Hantering av de nya Resource Manager-baserade webbtjänsterna
**Vad händer om jag bort tar bort min plan?**

hello plan tas bort från din prenumeration och du debiteras för användning i proportionellt fördelad.

> [!NOTE]
Du kan inte ta bort en plan som används av en webbtjänst. toodelete hello plan, måste du antingen tilldela en ny planera toohello webbtjänst eller ta bort hello-webbtjänsten.

**Vad är en planinstans?**

En instans av planen är en enhet med mängder som du lägger till tooyour fakturering plan. När du väljer en faktureringsnivå för din faktureringsplan medföljer en instans. Om du behöver mer inkluderade kvantiteter kan du lägga till instanser av hello valda nivån tooyour faktureringsavtal.

**Hur många planinstanser kan jag lägga till?**

Du kan ha en instans av hello prisnivån för utveckling och testning i en prenumeration.

För nivåerna Standard S1, Standard S2 och Standard S3 kan du lägga till så många du behöver.

> [!NOTE]
Beroende på din användning av förväntade det kan vara mer kostnadseffektivt tooupgrade tooa nivå som har fler inkluderat kvantiteter i stället lägga till instanser toohello aktuell nivå.

**Vad händer om jag byter till en betalningsplan på en annan nivå (uppgraderar/nedgraderar)?**

hello tidigare plan tas bort och hello aktuell användning faktureras på proportionellt. En ny plan med hello fullständig inkluderade mängder hello uppgraderas/nedgraderas nivån skapas för hello resten av hello period.

> [!NOTE]
Det inkluderade antalet tilldelas per period och eventuell outnyttjad användning förs inte över till nästa period.

**Vad händer när jag öka hello instanser i en plan?**

Ingår i proportionellt och kan ta 24 timmar toobe effektivt.

**Vad händer om jag tar bort en instans i en plan?**

hello instansen tas bort från prenumerationen och du debiteras för användning i proportionellt fördelad.

### <a name="sign-up-for-new-resource-manager-based-web-services-plans"></a>Registrera dig för planerna i de nya Resource Manager-baserade webbtjänsterna
**Hur registrera jag mig för en plan?**

Du har två sätt toocreate fakturering planer.

Första gången du distribuerar en ny Resource Manager-baserad webbtjänst kan du välja en befintlig plan eller skapa en ny plan.

Planer som du skapar i det här sättet är i standardregion och webbtjänsten blir distribuerade toothat region.

Om du vill toodeploy services tooregions än standardregion måste kanske du vill toodefine fakturering planerna innan du distribuerar din tjänst.

I så fall kan du logga in toohello Azure Machine Learning-webbtjänster portal och gå toohello planer sidan. Där kan du lägga till och ta bort planer och ändra befintliga planer.

**Vilken ska jag välja toostart av med?**

Vi rekommenderar att du börjar med hello Standard S1 tjänstnivån och övervaka din tjänst för användning. Om du upptäcker att du använder din inkluderade kvantiteter snabbt kan du lägga till instanser eller flytta tooa högre nivå och få bättre rabatterade priser. Du kan justera din faktureringsplan efter behov genom faktureringscykeln.

**Vilka regioner är hello nya scheman?**

hello nya fakturering planer är tillgängliga i hello tre regioner där vi stöder hello nya webbtjänster:

* Södra centrala USA
* Västra Europa
* Sydostasien

**Jag har webbtjänster i flera regioner. Behöver jag en plan för varje region?**

Ja. Priserna för planer varierar beroende på region. När du distribuerar en web service tooanother region måste tooassign it en plan som är specifika toothat region. Mer information finns i [Produkttillgänglighet per region]( https://azure.microsoft.com/regions/services/).

### <a name="new-web-services-overages"></a>De nya webbtjänsterna – överförbrukning
**Hur kontrollerar jag om jag överskrider webbtjänstanvändningen?**

Du kan visa hello användning på alla planer på hello planer sida i portalen för hello Azure Machine Learning-webbtjänster. Logga in toohello portal och klicka sedan på hello **planer** menyalternativet.

I hello **transaktioner** och **Compute** kolumnerna i tabell med hello som du kan se hello ingår mängder hello plan och hello procent som används.

**Vad händer när jag använder upp hello ska omfatta i hello prisnivån för utveckling och testning?**

Tjänster som har en utveckling och testning priser nivå som tilldelats toothem stoppas tills hello nästa period eller tills du flyttar tooa betald nivå.

**Hur beräknas priser för RRS- och BES-arbetsbelastningar för de klassiska webbtjänsterna och överförbrukning för de nya Resource Manager-baserade webbtjänsterna?**

För en RR-arbetsbelastning debiteras du för varje transaktion API-anrop som du gör och för hello beräkning som associeras med dessa förfrågningar. Resursposter för produktions-API transaktionskostnader beräknas som hello Totalt antal API-anrop som du gör multiplicerat med hello pris per 1 000 transaktioner (linjärt av enskilda transaktion). Din Resursposter API produktions-API beräkning timme beräknas som hello mängden tid som krävs för varje API-anrop toorun, multiplicerat med hello totala antalet API-transaktioner, multiplicerat med hello pris per timme för beräkning av produktions-API.

Till exempel för Standard S1-överförbrukning 1 000 000 API-transaktioner som äger 0.72 sekunder varje toorun leder (1 000 000 * 0,50 $/ 1K API-transaktioner) i 500 USD i produktions-API transaktionskostnader och (1 000 000 * 0.72 sek * $2 / tim) $400 i produktions-API-beräkningstimmar timmar för $900 totalt.

För en BES-arbetsbelastning debiteras du i hello samma sätt. Dock hello API transaktionskostnader representerar hello antalet batchjobb som du skickar och hello beräkning kostnader representera hello beräkning tid som är kopplad till dessa batchjobb. BES för produktions-API transaktionskostnader beräknas som hello Totalt antal jobb som skickats multiplicerat med hello pris per 1 000 transaktioner (linjärt av enskilda transaktion). Din BES API produktions-API beräkning timme beräknas som hello mängden tid som krävs för varje rad i dina jobb toorun multiplicerat med hello Totalt antal rader i jobbet multiplicerat med hello totala antalet jobb som multiplicerat med hello pris per produktions-API Beräkna timme. När du använder hello Machine Learning Kalkylatorn kombineras hello transaktion mätaren representerar hello antalet jobb som du planerar toosubmit och hello tid per transaktion fältet motsvarar hello tid som behövs för alla rader i varje jobb toorun.

Anta att du har en överförbrukning på standardnivån S1. Du skickar 100 jobb per dag och varje jobb består av 500 rader som tar 0,72 sekunder vardera. Din månatliga överförbrukning skulle vara (100 jobb per dag = 3 100 jobb/månad  0,50 USD/1 000 API-transaktioner) 1,55 USD i API-transaktionskostnader i produktionsmiljön och (500 rader  0,72 sek  3 100 jobb  2 USD/tim) 620 USD i API-beräkningstimmar i produktionsmiljön, vilket ger en totalsumma på 621,55 USD.

### <a name="azure-machine-learning-classic-web-services"></a>Klassiska Azure Machine Learning-webbtjänster
**Kan jag fortfarande betala per användning?**

Ja, de klassiska webbtjänsterna finns kvar i Azure Machine Learning.  

### <a name="azure-machine-learning-free-and-standard-tier"></a>Den kostnadsfria nivån och standardnivån för Azure Machine Learning
**Vad ingår i hello Azure Machine Learning kostnadsfria nivån?**

hello Azure Machine Learning kostnadsfria nivån är avsedda tooprovide en detaljerad introduktion toohello Azure Machine Learning Studio. Allt du behöver är en Microsoft-konto toosign upp. hello kostnadsfria nivån innehåller tillträde tooone Azure Machine Learning Studio arbetsytan per [Microsoft-konto](https://www.microsoft.com/account/default.aspx). Du kan använda upp too10 GB lagringsutrymme och operationalisera modeller som fristående API: er i det här skiktet. Arbetsbelastningar på den kostnadsfria nivån omfattas inte av något SLA och är endast avsedda för utveckling och personligt bruk. 

Kostnadsfri nivå arbetsytor har hello följande begränsningar:

* Arbetsbelastningar kan inte komma åt data genom att ansluta tooan lokal server som kör SQL Server.
* Du kan inte distribuera nya grundläggande Resource Manager-webbtjänster.


**Vad ingår i hello Azure Machine Learning standardnivån och planer?**

hello Azure Machine Learning standardnivån är en betald produktionsversionen av Azure Machine Learning Studio. hello Azure Machine Learning Studio månadsavgift faktureras på en per arbetsytan per månad basis och proportionellt fördelad i partiella månader. Experimenttimmar i Studio för Azure Machine Learning debiteras per beräkningstimme för aktiva experiment. Faktureringen beräknas proportionellt för ofullständiga timmar.  

beroende på om det är en klassisk webbtjänst eller en ny (Resource Manager-baserat) webbtjänst faktureras hello Azure Machine Learning API-tjänsten.

hello följande avgifter samman per arbetsytan för din prenumeration.

* Machine Learning-arbetsytan prenumeration: hello Machine Learning-arbetsytan prenumeration är en månadsavgift som ger åtkomst tooa Machine Learning Studio-arbetsytan. hello prenumerationen är obligatoriska toorun experiment i hello studio och tooutilize hello produktions-API: er.
* Experimenttimmar i Studio: den här mätaren aggregerar alla datorkostnader som uppstått genom att köra experiment i Machine Learning Studio och körs produktions-API-anrop i hello mellanlagring miljö.
* Åtkomst till data genom att ansluta tooan lokal server som kör SQL Server i din modeller för din utbildning och bedömningen.
* För de klassiska webbtjänsterna:
  * Beräkningstimmar för produktions-API: Den här mätaren visar beräkningsrelaterade avgifter som ackumuleras av webbtjänster som körs i produktionsmiljön.
  * Transaktioner i produktions-API (i 1 000-tal): den här mätaren innehåller avgifter som påförs per anrop tooyour produktions-webbtjänsten.

Avgifterna är aggregerade toohello valda planen förutom hello före kostnader, i hello fallet med Resource Manager-baserad webbtjänst:

* Standard S3-S1/S2 API planera (enheter): Den här mätaren representerar hello typ av instans som har valts för Resource Manager-baserade webbtjänster.
* Standard S3-S1/S2 överförbrukning API-Beräkningstimmar: Den här mätaren innehåller datorkostnader påförs av Resource Manager-baserade webbtjänster som körs i produktion efter antalet hello som ingår i befintliga instanser förbrukas. hello ytterligare användning debiteras med hello overate hastighet som är associerad med S3-S1/S2-plan nivå.
* Standard S3-S1/S2-överförbrukning av API-transaktioner (i 1 000-tal): den här mätaren innehåller avgifter som påförs per anrop tooyour produktion Resource Manager-baserad webbtjänst efter hello ingår kvantiteter i befintliga instanser förbrukas. hello extra debiteras med hello overate hastighet som är associerade med S3-S1/S2-plan nivå.
* Inkluderade antalet API-Beräkningstimmar: Med Resource Manager-baserade webbtjänster den här mätaren representerar hello ingår antal API-beräkningstimmar.
* Med antalet API-transaktioner (i 1 000-tal): med Resource Manager-baserade webbtjänster, den här mätaren representerar hello ingår antal API-transaktioner.

**Hur jag registrerar jag mig för den kostnadsfria Azure Machine Learning-nivån?**

Allt du behöver är ett Microsoft-konto. Gå för[Azure Machine Learning home](https://azure.microsoft.com/services/machine-learning/), och klicka sedan på **starta nu**. Logga in med ditt Microsoft-konto så skapas en arbetsyta på den kostnadsfria nivån. Du kan starta tooexplore och skapa Machine Learning-experiment direkt.

**Hur jag registrerar jag mig för Azure Machine Learning-standardnivån?**

Du måste först ha åtkomst tooan Azure-prenumeration toocreate en Standard Machine Learning-arbetsytan. Du kan registrera dig för en 30-dagars kostnadsfri utvärderingsversion Azure-prenumeration och senare uppgradera tooa betald Azure-prenumeration du kan köpa en betald Azure-prenumeration direkt Du kan sedan skapa en Machine Learning-arbetsytan från hello Microsoft Azure klassiska portal när du har kommit åtkomst toohello prenumeration. Visa hello [stegvisa instruktioner](https://azure.microsoft.com/trial/get-started-machine-learning-b/).

Alternativt kan du uppmanas av en Standard Machine Learning arbetsytan ägare tooaccess hello ägarens arbetsyta.

**Kan jag ange toouse min egen Azure Blob storage-konto med hello kostnadsfria nivån?**

Nej, hello standardnivån är likvärdiga toohello version av hello Machine Learning-tjänst som fanns innan hello nivåer introducerades.

**Kan jag distribuera min maskininlärning modeller som API: er på hello kostnadsfria nivån?**

Ja, operationalisera machine learning-modeller toostaging API-tjänster som en del av hello kostnadsfria nivån. tooput hello mellanlagring API-tjänsten till produktion och få en slutpunkt för produktion för hello operationalized tjänsten måste du använda hello standardnivån.

**Vad är hello skillnaden mellan kostnadsfri utvärderingsversion av Azure och Azure Machine Learning kostnadsfria nivån?**

Hej [kostnadsfri utvärderingsversion av Microsoft Azure](https://azure.microsoft.com/free/) erbjuder krediter som du kan använda tooany Azure-tjänst för en månad. hello Azure Machine Learning kostnadsfritt nivå erbjudanden kontinuerlig tillgång till specifikt tooAzure Machine Learning för icke-produktionsarbetsbelastningar.

**Hur flytta ett experiment från hello kostnadsfria nivån toohello standardnivån?**

toocopy experimenten från hello kostnadsfria nivån toohello standardnivån:

1. Logga in tooAzure Machine Learning Studio och se till att du kan se hello Standard arbetsytan i hello arbetsytan selector i hello övre navigeringsfältet och hello ledigt arbetsytan.
2. Växla tooFree arbetsytan om du är i hello Standard arbetsyta.
3. Välj ett experiment som du skulle som toocopy och klicka sedan på hello i listvyn för hello experiment **kopiera** kommandoknapp.
4. Välj hello Standard arbetsyta från hello dialogrutan som öppnas och klicka sedan på hello **kopiera** knappen.
   Alla Hej associerade datauppsättningar, tränad modell, etc. kopieras tillsammans med hello experiment på hello Standard arbetsyta.
5. Du behöver toorerun hello experiment och publicera om webbtjänsten hello Standard arbetsytan.

### <a name="studio-workspace"></a>Studio-arbetsytan
**Får jag olika fakturor för olika arbetsytor?**

Avgifterna för en arbetsyta visas separat för varje tillämplig mätare på samma faktura.

**Vilka specifika typer av beräkningsresurser körs mina experiment på?**

hello tjänsten Machine Learning är en multitenant. Faktiska beräkningsresurser som används på hello serverdel variera och har optimerats för prestanda och förutsägbarhet.

### <a name="guest-access"></a>Gäståtkomst
**Vad är gästbehörighet tooAzure Machine Learning Studio?**

Gäståtkomst är en begränsad utvärderingsmiljö. Du kan skapa och köra experiment i Azure Machine Learning Studio utan kostnad och utan autentisering. Gästsessioner är icke-beständig (går inte att spara) och begränsad tooeight timmar. Exempel på andra begränsningar är avsaknaden av stöd för R och Python, frånvaron av mellanlagrings-API:er samt storleksbegränsningar för datauppsättningar och lagringskapacitet. Jämförelse kan har användare väljer toosign in med ett Microsoft-konto fullständig åtkomst toohello kostnadsfria nivån i Machine Learning Studio som beskrivs tidigare, vilket innefattar en beständig arbetsyta och mer omfattande funktioner. toochoose får din kostnadsfria Machine Learning, klickar du på **Kom igång** på [https://studio.azureml.net](https://studio.azureml.net), och välj sedan **gissa åtkomst** eller logga in med ett Microsoft konto.

<!-- Module References -->
[image-reader]: https://msdn.microsoft.com/library/azure/893f8c57-1d36-456d-a47b-d29ae67f5d84/
[join]: https://msdn.microsoft.com/library/azure/124865f7-e901-4656-adac-f4cb08248099/
[machine-learning-modules]: https://msdn.microsoft.com/library/azure/6d9e2516-1343-4859-a3dc-9673ccec9edc/
[partition-and-sample]: https://msdn.microsoft.com/library/azure/a8726e34-1b3e-4515-b59a-3e4a475654b8/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[python]: https://msdn.microsoft.com/library/azure/CDB56F95-7F4C-404D-BDE7-5BB972E6F232
[counts]: https://msdn.microsoft.com/library/azure/dn913056.aspx
