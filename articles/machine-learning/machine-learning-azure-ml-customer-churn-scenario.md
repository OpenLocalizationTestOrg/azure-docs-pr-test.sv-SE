---
title: "aaaAnalyzing kunden Omsättningsuppdateringar med Machine Learning | Microsoft Docs"
description: "Fallstudie för att utveckla en integrerad modell för att analysera och bedömningen kunden omsättning"
services: machine-learning
documentationcenter: 
author: jeannt
manager: jhubbard
editor: cgronlun
ms.assetid: 1333ffe2-59b8-4f40-9be7-3bf1173fc38d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: jeannt
ms.openlocfilehash: 070e6a2ebe4f2fe439a42ffe1a3fa9d6d3788d62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analyzing-customer-churn-by-using-azure-machine-learning"></a>Analysera kundens omsättning med hjälp av Azure Machine Learning
## <a name="overview"></a>Översikt
Den här artikeln beskriver en för referensimplementering av en omsättning analys kundprojekt som skapats med hjälp av Azure Machine Learning. I den här artikeln tar vi upp associerade allmänna modeller för att lösa helhetsmässigt hello problemet med industriella kunden omsättning. Vi mäter också hello riktighet modeller som har skapats med hjälp av Machine Learning och vi bedöma anvisningarna för ytterligare utveckling.  

### <a name="acknowledgements"></a>Bekräftelser
Experimentet har utvecklats och testats av Serge Berger och huvudnamn Data forskare på Microsoft Roger Barga, tidigare produkten Manager för Microsoft Azure Machine Learning. hello Azure Dokumentationsteamet mycket bekräftar sina kunskaper och tack dem för att dela den här rapporten.

> [!NOTE]
> hello-data som används för experimentet är inte tillgänglig. Ett exempel på hur toobuild en maskininlärning modell för omsättning analys, se: [Retail omsättningsuppdateringar modellen mallen](https://gallery.cortanaintelligence.com/Collection/Retail-Customer-Churn-Prediction-Template-1) i [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/)
> 
> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="hello-problem-of-customer-churn"></a>hello problemet med kunden omsättning
Företag i hello konsumentmarknaden och i alla enterprise sektorer har toodeal med omsättning. Ibland omsättningen är för lång tid och påverkar förpolicybeslut. hello traditionell lösning är toopredict hög benägenheten churners och deras behov via en concierge tjänst, marknadsföringskampanjer, eller genom att tillämpa särskilda dispens. Dessa metoder kan variera från branschen tooindustry samt även från en viss konsumenten klustret tooanother inom samma bransch (till exempel telekommunikation).

hello vanliga faktorn är att företag behöver toominimize dessa ansträngningar för kvarhållning av särskilda kunden. Därför skulle en naturlig metod tooscore varje kund med hello sannolikheten för omsättning och åtgärda hello främsta de. hello främsta kunder hello mest lönsamma de; i mer avancerade scenarier, till exempel används en vinst funktion under hello val av kandidater för särskilda dispens. Dessa överväganden är dock endast en del av hello heltäckande strategi för att hantera omsättning. Företag har också tootake till kontot risk (och associerade risktolerans) hello nivå och kostnaden för hello åtgärd och rimligt kunden segmentering.  

## <a name="industry-outlook-and-approaches"></a>Branschen outlook och metoder
Avancerad hantering av omsättning är ett tecken på en mogen bransch. hello klassiska exempel är hello telekommunikation där prenumeranter är kända toofrequently växlar du från en leverantör tooanother. Den här frivilliga omsättning är ett särskilda problem. Dessutom providers skett betydande kunskap om *omsättningsuppdateringar drivrutiner*, vilket är hello faktorer som kunder tooswitch för enheten.

Till exempel är luren eller enhet valet en välkänd kärnan i hello mobiltelefon business-drivrutin. Därför är en populär princip toosubsidize hello priset på en luren om nya prenumeranter och dra en fullständig pris tooexisting kunder för en uppgradering. Den här principen har tidigare lett toocustomers hopping från en leverantör tooanother tooget en ny rabatt som i sin tur har uppmanas providers toorefine sina strategier.

Hög volatil i luren erbjudanden är en faktor som snabbt upphäver modeller av omsättning som baseras på den aktuella luren modeller. Dessutom är mobiltelefoner inte bara telekommunikation enheter; de är också sätt rapporter (Överväg att hello iPhone) och dessa sociala predictors är utanför hello omfattning reguljära telekommunikation datauppsättningar.

hello slutresultatet för modellering är att du kan utforma en sund princip genom att eliminera kända orsaker till omsättning. Faktum är en kontinuerlig modellering strategi, inklusive klassiska modeller som kvantifiera kategoriska variabler (till exempel beslutsträd) är **obligatoriska**.

Med stora datauppsättningar på sina kunder, utför organisationer analyser av stordata (särskilt omsättning identifiering baserat på stordata) som ett effektivt sätt toohello problem. Du kan läsa mer om hello stordata metoden toohello problemet med kärnan i hello rekommendationer om ETL-avsnittet.  

## <a name="methodology-toomodel-customer-churn"></a>Metod toomodel kunden omsättning
En gemensam problemlösning processen toosolve kunden omsättning visas i figur 1-3:  

1. En modell för risk kan du tooconsider hur åtgärder som påverkar sannolikhet och risker.
2. En åtgärd modell kan du tooconsider hur hello andelen åtgärd kan påverka hello sannolikheten för omsättningen och hello mängden kund-livslängden (CLV).
3. Den här analysen lämpar sig tooa kvalitativ analys som är eskalerad tooa proaktiv marknadsföringskampanj som riktar sig till kunden segment toodeliver hello optimala erbjudandet.  

![][1]

Den här framåt söker metoden är hello bästa sätt tootreat omsättning, men det ingår komplexitet: Vi har toodevelop med flera modeller archetype och spåra beroenden mellan hello modeller. vara kan inkapslade hello interaktion mellan modeller som visas i följande diagram hello:  

![][2]

*Bild 4: Enhetlig flera modellen archetype*  

Interaktionen mellan hello modeller är nyckeln om vi toodeliver en heltäckande metoden toocustomer kvarhållning. Varje modell försämras nödvändigtvis med tiden. Därför hello arkitektur är en implicit loop (liknande toohello archetype som angetts av hello SKARPA DM data utvinningsmodellen standard [***3***]).  

hello är övergripande cykeln av risk-beslut-marknadsföring segmentering/uppdelning fortfarande en generaliserad struktur, som är tillämpliga toomany affärsproblem. Omsättning analys är en starkt representant i den här gruppen av problem eftersom den ger alla hello egenskaper för komplexa affärsproblem som inte tillåter att en förenklad förutsägelselösning. hello sociala aspekter av hello moderna metoden toochurn inte särskilt markeras i hello-metoden, men hello sociala aspekter som är inkapslade i hello modellering archetype som de skulle göra i någon modell.  

Ett intressant tillägg är stordata. Dagens telekommunikation och retail företag samla in fullständig information om sina kunder och vi kan enkelt vill att hello måste för flera modell anslutningen kommer att bli en gemensam trend, ges nya trender som hello Sakernas Internet och allt vanligare enheter som gör att företag tooemploy smarta lösningar på flera lager.  

 

## <a name="implementing-hello-modeling-archetype-in-machine-learning-studio"></a>Implementera hello modeling archetype i Machine Learning Studio
Angivna hello problemet som beskrivs bara vad är hello bästa sätt tooimplement en integrerad modellering och bedömningsprofil metoden? I det här avsnittet visar vi hur vi göra detta med Azure Machine Learning Studio.  

flera modellen hello-metoden är ett måste när du skapar en global archetype för omsättning. Även hello bedömningen (förutsägande) tillhör hello-metoden ska vara flera modellen.  

hello följande diagram visar hello prototyp vi skapade som använder fyra bedömningsprofil algoritmer i Machine Learning Studio toopredict omsättning. hello orsak till att använda flera olika modeller metoden är toocreate ensemble klassificerare tooincrease noggrannhet utan även tooprotect mot över passning och tooimprove normativ Funktionsurval.  

![][3]

*Bild 5: Prototyp av en omsättning modeling metod*  

hello innehåller följande avsnitt mer information om hello prototyp poängsättning av modellen som vi har implementerat med hjälp av Machine Learning Studio.  

### <a name="data-selection-and-preparation"></a>Dataurval och förberedelse
hello data används toobuild hello modeller och poäng kunder har hämtats från en lodrät CRM-lösning med hello data som har dolts tooprotect kundens integritet. hello data innehåller information om 8 000 prenumerationer i hello USA och det kombinerar tre källor: etablering data (prenumeration metadata), aktivitetsdata (användning av hello system) och kundinformation för support. hello data innehåller inte alla företag relaterad information om hello kunder. till exempel innehåller den inte förmåner metadata eller kredit resultat.  

För enkelhetens skull är ETL och rensa processer data utanför omfånget eftersom vi antar att förberedelse av data har redan gjorts på en annan plats.   

Val av funktioner för modellering baseras på preliminär signifikans poängsättning av hello uppsättning predictors som ingår i hello process som använder hello slumpmässiga skog modulen. Hello-implementering i Machine Learning Studio beräknade vi hello medelvärde, median och intervallen för representativt funktioner. Vi lagt till exempel mängder för hello kvalitativa data, till exempel lägsta och högsta värden för användaraktivitet.    

Vi också avbildas temporal information för hello de senaste sex månaderna. Vi analyserade data för ett år och det fastställs att även om det fanns statistiskt viktiga trender, minskat hello effekt på omsättning avsevärt efter sex månader.  

hello viktigaste är hello hela processen, inklusive ETL, val av funktioner och modellering implementerades i Machine Learning Studio, använda datakällor i Microsoft Azure.   

hello visar följande diagram hello data som har använts.  

![][4]

*Bild 6: Utdrag ur datakälla (dolt)*  

![][5]

*Bild 7: Funktioner som extraheras från datakällan*
 

> Observera att dessa data är privat och därför hello modellen och kan inte delas.
> Men en liknande modell med offentligt tillgängliga data, finns i det här exemplet experiment i hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/): [anger kunden Omsättningsuppdateringar](http://gallery.cortanaintelligence.com/Experiment/31c19425ee874f628c847f7e2d93e383).
> 
> Mer information om hur du kan implementera en modell för analys av omsättning med hjälp av Cortana Intelligence Suite toolearn, vi rekommenderar också att [den här videon](https://info.microsoft.com/Webinar-Harness-Predictive-Customer-Churn-Model.html) av programchef Wee Hyong Tok. 
> 
> 

### <a name="algorithms-used-in-hello-prototype"></a>Algoritmer som används i hello-prototyp
Vi använde hello följande fyra maskininlärningsalgoritmer toobuild hello prototyp (Ingen anpassning):  

1. Logistic regression (LR)
2. Tvåklassförhöjda beslutsträdet (BT)
3. Genomsnittlig perceptron (AP)
4. Support vector machine (SVM)  

hello illustrerar följande diagram en del av hello experiment designytan, som visar hello ordning i vilken hello modeller har skapats:  

![][6]  

*Bild 8: Skapa modeller i Machine Learning Studio*  

### <a name="scoring-methods"></a>Bedömningsprofil metoder
Vi bedömas hello fyra modeller med hjälp av en märkt utbildning dataset.  

Vi skickade också hello bedömningen dataset tooa jämförbara modellen skapas med hjälp av hello skrivbord utgåva av SAS Enterprise Miner 12. Vi mätt hello riktighet hello SAS-modellen och alla fyra Machine Learning Studio-modeller.  

## <a name="results"></a>Resultat
I det här avsnittet presenterar vi vårt resultat om hello riktighet hello modeller, baserat på hello bedömningen dataset.  

### <a name="accuracy-and-precision-of-scoring"></a>Noggrannhet och precision av bedömningen
I allmänhet är hello-implementering i Azure Machine Learning bakom SAS Precision med cirka 10 – 15% (område Under kurvan eller AUC).  

Hello viktigaste mått i omsättning är dock hello felklassificering hastighet: som är av hello främsta churners som förväntade av hello klassificerare, vilken av dem gjorde **inte** omsättningsuppdateringar och har fått någon särskild behandling? hello följande diagram jämförs felklassificering hastigheten för alla hello modeller:  

![][7]

*Bild 9: Passau prototyp området under kurva*

### <a name="using-auc-toocompare-results"></a>Med hjälp av AUC toocompare resultat
Området Under kurvan (AUC) är ett mått som representerar en global mått på *Avskiljbarhet* mellan hello distributioner av poängen för positiva och negativa population. Det är liknande toohello traditionella mottagaren operatorn egenskap (ROC) diagrammet, men en viktig skillnad är att hello AUC mått inte kräver toochoose ett tröskelvärde. I stället det sammanfattar hello resultat över **alla** möjliga alternativ. Däremot hello traditionella ROC diagrammet visar hello positiva satsen på hello lodrät axel och hello falska positiva på hello vågrät axel och hello klassificering tröskelvärde varierar.   

AUC används vanligtvis som en åtgärd för värd för olika algoritmer (eller olika system) eftersom den tillåter modeller toobe jämfört med hjälp av deras AUC värden. Det här är en populär metod i branscher som meteorologi och biosciences. Därför representerar AUC ett populära verktyg för att bedöma klassificerare prestanda.  

### <a name="comparing-misclassification-rates"></a>Jämföra felklassificering priser
Vi jämfört med hello felklassificering priser för hello datauppsättningen i fråga med hjälp av hello CRM-data över ca 8 000 prenumerationer.  

* hello SAS felklassificering hastighet var 10 – 15%.
* hello Machine Learning Studio felklassificering hastighet var 15-20% för hello översta 200 300 churners.  

Hello telekommunikation är det viktigt tooaddress endast kunder som har hello högsta risk toochurn genom att erbjuda dem en concierge tjänst eller andra särskilda behandling. I detta avseende uppnår hello Machine Learning Studio implementering resultat likvärdiga dem hello SAS-modellen.  

Av hello token samma, noggrannhet är viktigare än precisionen eftersom vi främst är intresserad av att klassificera potentiella churners korrekt.  

följande diagram från Wikipedia hello visar hello relation i en livlig, enkelt att förstå bild:  

![][8]

*Figur 10: Förhållandet mellan noggrannhet och precision*

### <a name="accuracy-and-precision-results-for-boosted-decision-tree-model"></a>Noggrannhet och precision resultat för förstärkta trädet modellen
följande diagram visar hello rådata hello resultatet av bedömningen med hello Machine Learning prototyp för hello ökat beslut trädet modell, som sker toobe hello mest korrekta bland hello fyra modeller:  

![][9]

*Figur 11: Förstärkta trädet modellen egenskaper*

## <a name="performance-comparison"></a>Jämförelse av prestanda
Vi jämfört med hello hastigheten som data har lyckats med hello Machine Learning Studio modeller och en jämförbar modell som skapats med hjälp av hello skrivbord utgåva av SAS Enterprise Miner 12.1.  

hello följande tabell sammanfattas hello prestanda för hello algoritmer:  

*Tabell 1. Allmänna prestanda (noggrannhet) hello algoritmer*

| LR | BT | AP | SVM |
| --- | --- | --- | --- |
| Genomsnittlig modellen |hello bästa modellen |Presterar som förväntat |Genomsnittlig modellen |

hello modeller finns i Machine Learning Studio gick bättre än förväntat SAS med 15 25% för hastigheten för körning, men noggrannhet utfördes i stort sett par.  

## <a name="discussion-and-recommendations"></a>Beskrivning och rekommendationer
I hello telekommunikation flera metoder har vuxit fram tooanalyze omsättningsuppdateringar, inklusive:  

* Härledd mätvärden för fyra grundläggande kategorier:
  * **Entitet (till exempel en prenumeration)**. Etablera grundläggande information om hello prenumeration och/eller kund som hello ämnet för omsättning.
  * **Aktiviteten**. Hämta alla möjliga användningsinformation som är relaterade toohello enhet, till exempel hello antal inloggningar.
  * **Teknisk support**. Samla information från kunden stöd loggar tooindicate om hello prenumerationen har problem eller interaktioner med kunden stöd.
  * **Konkurrenskraftiga-och**. Hämta möjliga information om hello kund (till exempel kan vara otillgänglig eller hårda tootrack).
* Använd vikten toodrive Funktionsurval. Detta innebär att hello ökat beslut trädet modellen är alltid en Orderlöfte metod.  

hello användning av dessa fyra kategorier skapar illusionen av hello som en enkel *deterministisk* metod, baserat på index format på rimliga faktorer per kategori, bör vara tillräckligt tooidentify kunder för risk för omsättning. Tyvärr kan är detta begrepp verkar rimligt, men false förstå. hello orsak är att omsättning är en temporal effekt hello faktorer bidrar toochurn är vanligtvis tillfälligt tillstånd. Vad leder en kund tooconsider lämnar idag skilja imorgon och det verkligen är olika sex månader från nu. Därför kan en *probabilistic* modell är nödvändigt.  

Den här viktiga observationer förbises ofta i företag, som vanligtvis föredrar en business intelligence-inriktade metoden tooanalytics, främst eftersom det är en enklare sälja och ger enkla automation.  

Hej molnapparnas självserviceanalys med hjälp av Machine Learning Studio är dock att hello fyra typer av information som klassificerats efter avdelning blir en viktig källa för machine learning om omsättning.  

En spännande funktion kommer i Azure Machine Learning är hello möjlighet tooadd en anpassad modul toohello databas med fördefinierade moduler som redan är tillgängliga. Den här funktionen i princip och skapar en möjlighet tooselect bibliotek och skapa mallar för vertikala marknader. Det är en viktiga fördelar med Azure Machine Learning i hello marknadsplatsen.  

Vi hoppas toocontinue det här avsnittet i hello framtida, särskilt relaterade toobig dataanalys.
  

## <a name="conclusion"></a>Slutsats
Det här dokumentet beskriver sensible metoden tootackling hello vanliga problem med kunden omsättning med hjälp av ett allmänt ramverk. Vi anses vara en prototyp för resultatfunktioner modeller och implementeras med hjälp av Azure Machine Learning. Slutligen vi för att utvärdera hello noggrannhet och prestanda för hello prototyp lösning med beaktande toocomparable algoritmer i SAS.  

**Mer information:**  

Det här dokumentet hjälpa dig? Ge oss din feedback. Berätta för oss på en skala från 1 (dåligt) too5 (utmärkt), vilket omdöme ger du det här dokumentet och varför har du gett den nivån? Exempel:  

* Är du klassificeringen av den höga på grund av toohaving bra exempel, utmärkt skärmdumpar, rensa skrivs, eller en annan orsak?
* Är du betygsätta det låga på grund av toopoor exempel, fuzzy skärmdumpar eller oklart skrivning?  

Denna feedback hjälper oss att förbättra hello kvaliteten på faktablad vi versionen.   

[Skicka feedback](mailto:sqlfback@microsoft.com).
 

## <a name="references"></a>Referenser
[1] Förutsägelseanalyser: utöver hello förutsägelser W. McKnight, hantering, juli/augusti 2011, s.18 20.  

[2] Wikipedia artikel: [noggrannhet och precision](http://en.wikipedia.org/wiki/Accuracy_and_precision)

[3] [SKARPA-DM 1.0: stegvisa Data Utvinningsmodellen Guide](http://www.the-modeling-agency.com/crisp-dm.pdf)   

[4] [Stordata marknadsföring: engagera kunderna mer effektivt och enheten värde](http://www.amazon.com/Big-Data-Marketing-Customers-Effectively/dp/1118733894/ref=sr_1_12?ie=UTF8&qid=1387541531&sr=8-12&keywords=customer+churn)

[5] [anger omsättningsuppdateringar modellen mallen](http://gallery.cortanaintelligence.com/Experiment/Telco-Customer-Churn-5) i [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/) 
 

## <a name="appendix"></a>Bilaga
![][10]

*Figur 12: Ögonblicksbild av en presentation på omsättning prototyp*

[1]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-1.png
[2]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-2.png
[3]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-3.png
[4]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-4.png
[5]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-5.png
[6]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-6.png
[7]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-7.png
[8]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-8.png
[9]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-9.png
[10]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-10.png
