---
title: "aaaGuide toohello Net # Neurala nätverk specifikationsspråk | Microsoft Docs"
description: "Syntaxen för hello Net # neurala nätverk specifikationsspråk, tillsammans med exempel på hur toocreate ett anpassat neurala nätverk modellen i Microsoft Azure ML med hjälp av Net #"
services: machine-learning
documentationcenter: 
author: jeannt
manager: jhubbard
editor: cgronlun
ms.assetid: cfd1454b-47df-4745-b064-ce5f9b3be303
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: jeannt
ms.openlocfilehash: 3493247ecc39ca3a1382510ad520d7017159ff62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="guide-toonet-neural-network-specification-language-for-azure-machine-learning"></a>Guiden tooNet # neurala nätverket specifikationsspråk för Azure Machine Learning
## <a name="overview"></a>Översikt
NET # är ett språk som har utvecklats av Microsoft som används toodefine neural nätverksarkitekturer. Du kan använda Net # i neurala nätverket moduler i Microsoft Azure Machine Learning.

<!-- This function doesn't currentlyappear in hello MicrosoftML documentation. If it is added in a future update, we can uncomment this text.

, or in hello `rxNeuralNetwork()` function in [MicrosoftML](https://msdn.microsoft.com/microsoft-r/microsoftml/microsoftml). 

-->

I den här artikeln får du lära dig grundläggande koncept behövs toodevelop en anpassad neurala nätverket: 

* Neural nätverkskrav och hur toodefine hello huvudkomponenter
* hello syntax och nyckelord för hello Net #-specifikationsspråk
* Exempel på anpassade neurala nätverk som skapats med hjälp av Net # 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="neural-network-basics"></a>Grunderna i neurala nätverket
En neurala nätverk som består av ***noder*** som är ordnade i ***lager***, och det vägda ***anslutningar*** (eller ***kanter***) mellan hello-noder. hello anslutningar är riktat och varje anslutning har en ***källa*** nod och en ***mål*** nod.  

Varje ***trainable layer*** (en dold eller ett lager för utdata) har en eller flera ***anslutning paket***. En anslutning bunt består av ett lager för källa och en specifikation av hello anslutningar från källan lagret. Alla hello-anslutningar i en viss paket-resurs hello samma ***källa layer*** och hello samma ***mållagret***. Ett paket med anslutning betraktas som tillhör toohello paket mållagret i Net #.  

NET # stöder olika typer av paket för anslutningen, vilket gör att du anpassar hello sätt indata är mappade toohidden lager och mappade toohello matar ut.   

hello standard eller standard paket är en **fullständig paket**, vilken varje nod i hello källa skiktet är anslutna tooevery nod i hello mållagret.  

Dessutom stöder Net # hello följande fyra typer av avancerad anslutning paket:  

* **Filtrerade paket**. hello användare kan definiera ett predikat hello platser för hello Källnoden lager och hello layer målnoden. Noder är anslutna när hello är True.
* **Convolutional paket**. hello användare kan definiera små neighborhoods av noder i hello källa lager. Varje nod i hello mållagret är anslutna tooone nätverket av noder i hello källa lagret.
* **Poolning paket** och **svar normalisering paket**. Dessa är liknande tooconvolutional paket i den hello användaren definierar små neighborhoods av noder i hello källa lagret. hello skillnaden är att hello vikten av hello kanter i dessa paket inte är trainable. I stället en fördefinierad funktion används toohello Källnoden värden toodetermine hello mål nodvärde.  

Med hjälp av Net # gör toodefine hello struktur för ett neurala nätverk det möjligt toodefine komplexa strukturer som djupa neurala nätverk eller faltningar av godtycklig dimensioner, som är kända tooimprove inlärning på data, till exempel bild, ljud och video.  

## <a name="supported-customizations"></a>Anpassningar som stöds
hello-arkitekturen för neurala nätverket modeller som du skapar i Azure Machine Learning kan stor utsträckning anpassas med hjälp av Net #. Du kan:  

* Skapa dolda lager och kontroll hello antalet noder i varje lager.
* Ange hur lager toobe anslutna tooeach andra.
* Definiera särskilda anslutningar, till exempel faltningar och vikt delning paket.
* Ange olika aktivering funktioner.  

Mer information om syntaxen för anspråksregelspråket hello-specifikationen finns [struktur specifikationen](#Structure-specifications).  

Exempel på definierar neurala nätverk för några vanliga uppgifter från simplex toocomplex för maskininlärning finns [exempel](#Examples-of-Net#-usage).  

## <a name="general-requirements"></a>Allmänna krav
* Det måste finnas exakt en utgående lager, minst ett indata lager och noll eller flera dolda lager. 
* Varje lager har ett fast antal noder, begreppsmässigt ordnade i en rektangulär matris av godtycklig dimensioner. 
* Inkommande lager saknar associerade utbildade parametrar som representerar hello punkt där instansdata anger hello nätverk. 
* Trainable lager (hello dolda och utdata lager) har associerade utbildade parametrar, kallas även eventuella fördomar och vikt. 
* hello käll- och noder måste vara i separata lager. 
* Anslutningar måste vara acykliska; med andra ord, får inte det vara en kedja av anslutningar som leder tillbaka toohello inledande Källnoden.
* hello utdata layer får inte vara ett källa lager för ett paket för anslutningen.  

## <a name="structure-specifications"></a>Specifikationer för struktur
En neurala nätverket struktur specifikation består av tre delar: hello **konstantdeklaration**, hello **layer deklaration**, hello **anslutning deklaration**. Det finns även en valfri **dela deklaration** avsnitt. hello avsnitt kan anges i valfri ordning.  

## <a name="constant-declaration"></a>Konstantdeklaration
En konstantdeklaration är valfritt. Det ger ett sätt toodefine värden används någon annanstans i definition av hello neurala nätverk. hello deklaration instruktion består av en identifierare som följt av ett likhetstecken och ett värdeuttryck.   

Till exempel hello efter uttrycket definierar en konstant **x**:  

    Const X = 28;  

toodefine två eller flera konstanter samtidigt, omges klammerparenteser hello identifierarnamn och värden och avgränsa dem med semikolon. Exempel:  

    Const { X = 28; Y = 4; }  

hello till höger på varje tilldelning uttrycket kan vara ett heltal, ett verkligt tal, ett booleskt värde (True eller False) eller ett matematiska uttryck. Exempel:  

    Const { X = 17 * 2; Y = true; }  

## <a name="layer-declaration"></a>Deklarationen för lager
hello layer deklaration krävs. Den definierar hello storlek och källa för hello-lagret, inklusive dess anslutning paket och attribut. Hej deklaration instruktionen börjar med hello namnet på hello lager (indata, dolda eller utdata), följt av hello dimensioner för hello layer (en tuppel positiva heltal). Exempel:  

    input Data auto;
    hidden Hidden[5,20] from Data all;
    output Result[2] from Hidden all;  

* hello produkten av hello dimensioner är hello antalet noder i hello lager. I det här exemplet finns det två dimensioner [5,20], vilket innebär att det finns 100 noder i hello lager.
* hello lager kan deklareras i vilken ordning som helst, med ett undantag: om fler än ett inkommande lager definieras hello ordning de deklareras måste matcha hello ordningen på funktioner i hello indata.  

toospecify hello antalet noder i ett lager att fastställa automatiskt, Använd hello **automatisk** nyckelord. Hej **automatisk** nyckelordet har olika effekter, beroende på hello lager:  

* I en inkommande layer-deklaration är hello antalet noder hello antal funktioner i hello indata.
* I en deklaration som dolda lagret hello antalet noder är hello-nummer som anges av hello parametervärdet för **antalet dolda noder**. 
* I en layer deklaration utdata är hello antalet noder 2 för tvåklass klassificering, 1 för regression och lika toohello antalet noder som utdata för multiklass-baserad klassificering.   

Kan till exempel hello följande nätverksdefinitionen hello storleken på alla lager toobe automatiskt:  

    input Data auto;
    hidden Hidden auto from Data all;
    output Result auto from Hidden all;  


En layer-deklaration för en trainable lager (hello dolda eller utdata lager) kan du också inkludera hello utdata funktionen (kallas också en funktion för aktivering), där standardinställningen för**sigmoid** för klassificering modeller och **linjär** för regression modeller. (Även om du använder hello standard du uttryckligen uppge hello aktivering funktion, om så önskas för tydlighetens skull.)

hello stöder utdata följande funktioner:  

* sigmoid
* linjär
* softmax
* rlinear
* Ruta
* rot
* srlinear
* ABS
* TANH 
* brlinear  

Hello följande deklaration använder exempelvis hello **softmax** funktionen:  

    output Result [100] softmax from Hidden all;  

## <a name="connection-declaration"></a>Deklarationen för anslutning
Du måste deklarera anslutningar mellan hello lager som du har definierat omedelbart när du har definierat hello trainable lager. hello anslutning paket deklaration börjar med nyckelordet hello **från**, följt av hello paket lager och hello datakälltypen av anslutningen paket toocreate hello namn.   

För närvarande stöds fem typer av paket för anslutningen:  

* **Fullständig** paket som anges av hello nyckelordet **alla**
* **Filtrerade** paket som anges av hello nyckelordet **där**, följt av ett predikat uttryck
* **Convolutional** paket som anges av hello nyckelordet **convolve**, följt av hello Faltning attribut
* **Poolning** paket som anges av hello nyckelord **max poolen** eller **innebär pool**
* **Svaret normalisering** paket som anges av hello nyckelordet **svar normen**      

## <a name="full-bundles"></a>Fullständig paket
En fullständig anslutning bunt innehåller en anslutning från varje nod i hello layer tooeach Källnoden i hello mållagret. Detta är hello standardanslutningstyp av nätverksanslutning.  

## <a name="filtered-bundles"></a>Filtrerade paket
En filtrerad anslutning paket specifikation innehåller ett predikat, uttryckt syntaktiskt, mycket som ett C# lambda-uttryck. hello definierar följande exempel två filtrerade paket:  

    input Pixels [10, 20];
    hidden ByRow[10, 12] from Pixels where (s,d) => s[0] == d[0];
    hidden ByCol[5, 20] from Pixels where (s,d) => abs(s[1] - d[1]) <= 1;  

* I hello predikatet för *ByRow*, **s** är en parameter som representerar ett index i hello rektangulär matris av noder av hello inkommande lagret, *bildpunkter*, och **d**  är en parameter som representerar ett index i hello array noder i hello dolda lagret, *ByRow*. Hej typ av både **s** och **d** är en tuppel med heltal med längden två. Begreppsmässigt **s** sträcker sig över alla par med heltal med *0 < = s [0] < 10* och *0 < = s[1] < 20*, och **d**  sträcker sig över alla par med heltal, med *0 < = d [0] < 10* och *0 < = d[1] < 12*. 
* På hello höger i hello predikatuttryck finns det ett villkor. I det här exemplet för varje värde i **s** och **d** så att hello villkoret är sant, det finns en gräns från hello källa layer nod toohello layer målnoden. Därför det här filteruttrycket anger att hello-paket innehåller en anslutning från hello-noden som definieras av **s** toohello nod som definieras av **d** i samtliga fall där s [0] är lika tood [0].  

Du kan också kan du ange en uppsättning vikterna för ett filtrerade paket. Hej värde för hello **vikterna** attributet måste vara en tuppel av flytande punktvärden med en längd som matchar hello antalet anslutningar som definieras av hello-paket. Som standard genereras slumpmässigt vikter.  

Värde som är grupperade efter hello index för noden. Det vill säga om hello första Målnoden är ansluten tog källa noder hello först *K* element i hello **vikterna** tuppel är hello vikterna för hello första målnoden i källan index ordning. hello samma gäller för hello återstående mål noder.  

Det är möjligt toospecify vikterna direkt som konstanta värden. Om du har lärt dig hello vikterna tidigare kan du exempelvis ange dem som konstanter med följande syntax:

    const Weights_1 = [0.0188045055, 0.130500451, ...]


## <a name="convolutional-bundles"></a>Convolutional paket
När hello träningsdata har en homogen struktur, är convolutional anslutningar används ofta toolearn avancerade funktioner hello data. Till exempel i bild, ljud- och data, spatial eller temporal dimensionalitet kan vara ganska enhetlig.  

Convolutional paket använder rektangulär **kärnor** som slid via hello dimensioner. I princip varje kernel definierar en uppsättning vikten som används i lokala neighborhoods, enligt tooas **kernel-program**. Varje kernel-program motsvarar tooa nod i hello källa lager som är refererad tooas hello **centrala nod**. hello vikten av en kernel delas av många anslutningar. I en convolutional bundle varje kernel är rektangulär och alla kernel-program är hello samma storlek.  

Convolutional paket stöder hello följande attribut:

**InputShape** definierar hello dimensionalitet för hello källa lager för hello convolutional paketet. hello-värdet måste vara en tuppel med positivt heltal. hello produkten av hello heltal måste vara lika med hello antalet noder i hello källa lagret, men i annat fall det behöver inte toomatch hello dimensionalitet har deklarerats för hello källa lager. hello längden på den här tuppeln blir hello **aritet** värde för hello convolutional paket. (Normalt aritet refererar toohello antal argument eller operander som en funktion kan ta)  

toodefine hello form och platserna för hello kärnor använda hello attribut **KernelShape**, **Stride**, **utfyllnad**, **LowerPad**, och **UpperPad**:   

* **KernelShape**: (obligatoriskt) anger hello dimensionaliteten för varje kernel för hello convolutional paket. hello-värdet måste vara en tuppel med positiva heltal med en längd som är lika med hello aritet för hello-paket. Varje komponent i den här tuppeln får inte vara större än motsvarande hello-komponenten i **InputShape**. 
* **STRIDE**: (valfritt) anger hello glidande steg storlekar på hello Faltning (ett Stegstorlek för varje dimension), som är hello avståndet mellan hello centrala noder. hello-värdet måste vara en tuppel med positiva heltal med en längd som är hello aritet för hello-paket. Varje komponent i den här tuppeln får inte vara större än motsvarande hello-komponenten i **KernelShape**. hello standardvärdet är en tuppel med alla komponenter lika tooone. 
* **Dela**: (valfritt) anger hello vikt för varje dimension hello Faltning. hello-värdet kan vara ett booleskt värde eller en tuppel med booleska värden med en längd som är hello aritet för hello-paket. Ett booleskt värde som är utökad toobe en tuppel hello rätt längd med alla komponenter lika toohello angivet värde. hello standardvärdet är en tuppel som består av alla värden som True. 
* **MapCount**: (valfritt) anger hello antalet funktionen mappar för hello convolutional paket. hello-värdet kan vara ett enda positivt heltal eller en tuppel med positiva heltal med en längd som är hello aritet för hello-paket. Ett enda heltal utökas toobe en tuppel hello rätt längd med hello första komponenter lika toohello anges värdet och alla hello återstående komponenter lika tooone. hello standardvärdet är en. hello Totalt antal funktionen maps är hello produkt hello komponenter i hello tuppel. hello factoring för detta totala antal för hello komponenter bestämmer hur hello funktionen mappa värden grupperas i hello mål noder. 
* **Vikterna**: (valfritt) anger hello inledande vikterna för hello-paket. hello-värdet måste vara en tuppel av flytande punktvärden med en längd som är hello antalet kärnor gånger hello vikterna per kernel, enligt nedan. Hej Standardvikterna genereras slumpmässigt.  

Det finns två uppsättningar med egenskaper som styr utfyllnad, hello egenskaper som inte anges samtidigt:

* **Utfyllnad**: (valfritt) avgör om hello indata ska bli utfyllt med hjälp av en **utfyllnad standardschema**. hello-värdet kan vara ett booleskt värde eller det kan vara en tuppel med booleska värden med en längd som är hello aritet för hello-paket. Ett booleskt värde som är utökad toobe en tuppel hello rätt längd med alla komponenter lika toohello angivet värde. Om hello värde för en dimension är True utfyllnad hello källa logiskt i dimensionen med cellerna noll-värden toosupport ytterligare kernel-program så att hello centrala noder i hello första och sista kernlar som är i den aktuella dimensionen är hello första och sista noder i den dimensionen i hello källa lager. Därför hello antalet ”falska” noder i varje dimension är fastställt automatiskt toofit exakt *(InputShape [d] - 1) / Stride [d] + 1* kärnor till hello utfyllnad källa lager. Om hello-värdet för en dimension är False, hello hello kernlar som är definierade så att hello antalet noder på varje sida som lämnas ut är samma (upp tooa skillnaden mellan 1). hello standardvärdet för det här attributet är en tuppel med alla komponenter lika tooFalse.
* **UpperPad** och **LowerPad**: (valfritt) Ange större kontroll över hello mängden utfyllnad toouse. **Viktigt:** attributen kan definieras om och bara om hello **utfyllnad** egenskapen ovan är ***inte*** definieras. hello-värden måste vara heltal enkelvärdesattribut tupplar med längd som är hello aritet för hello-paket. När dessa attribut har angetts ”falska” noder läggs toohello lägre och övre ändar av varje dimension hello ingående lager. hello antalet noder som lagts till toohello lägre och övre ends i varje dimension bestäms av **LowerPad**[i] och **UpperPad**[i] respektive. tooensure att kärnor överensstämmer endast för ”verkliga” noderna och inte för ”falska” noder hello följande villkor uppfyllas:
  * Varje komponent i **LowerPad** måste vara absolut mindre än KernelShape [d] / 2. 
  * Varje komponent i **UpperPad** får inte vara större än KernelShape [d] / 2. 
  * hello standardvärdet av dessa attribut är en tuppel med alla komponenter lika too0. 

hello inställningen **utfyllnad** = true gör det mycket utfyllnad som behövs tookeep hello ”mittpunkt” hello kernel inuti hello ”riktigt” indata. Hello matematiska lite för datoranvändning hello storlek ändras. I allmänhet storleken på utdata hello *D* beräknas som *D = (I - K) / S + 1*, där *jag* är hello inkommande storlek *K* hello kernel storlek *S* är hello stride och  */*  är Heltalsdivision (avrunda mot noll). Om du ställer in UpperPad = [1, 1] hello indata storlek *jag* är i praktiken 29, och därmed *D = (29-5) / 2 + 1 = 13*. Men när **utfyllnad** = true, i stort sett *jag* hämtar ökar av *K - 1*; därför *D = ((28 + 4) - 5) / 27 + 1 = 2 / 2 + 1 = 13 + 1 = 14*. Genom att ange värden för **UpperPad** och **LowerPad** du får mycket mer kontroll över hello utfyllnad än om du just ställt **utfyllnad** = true.

Mer information om convolutional nätverk och deras program finns i de här artiklarna:  

* [http://deeplearning.NET/Tutorial/lenet.HTML](http://deeplearning.net/tutorial/lenet.html)
* [http://Research.microsoft.com/pubs/68920/icdar03.PDF](http://research.microsoft.com/pubs/68920/icdar03.pdf) 
* [http://People.csail.MIT.edu/jvb/Papers/cnn_tutorial.PDF](http://people.csail.mit.edu/jvb/papers/cnn_tutorial.pdf)  

## <a name="pooling-bundles"></a>Poolning paket
En **poolning paket** gäller geometri liknande tooconvolutional anslutning, men med fördefinierade funktioner toosource nod värden tooderive hello mål nodvärde. Därför har anslutningspoolen paket inga trainable tillstånd (vikterna eller eventuella fördomar). Poolning paket stöd för alla hello convolutional attribut utom **delning**, **MapCount**, och **vikterna**.  

Normalt hello kärnor sammanfattning av intilliggande anslutningspoolen enheter inte överlappar varandra. Om Stride [d] lika tooKernelShape [d] i varje dimension är hello layer fick hello traditionella lokala anslutningspoolen lager, som ofta används i convolutional neurala nätverk. Varje målnoden beräknar hello maximala eller hello medelvärdet av hello aktiviteter för dess kernel i hello källa lager.  

hello följande exempel illustrerar ett anslutningspoolen paket: 

    hidden P1 [5, 12, 12]
      from C1 max pool {
        InputShape  = [ 5, 24, 24];
        KernelShape = [ 1,  2,  2];
        Stride      = [ 1,  2,  2];
      }  

* hello aritet för hello-paket är 3 (hello längden på hello tupplar **InputShape**, **KernelShape**, och **Stride**). 
* hello antalet noder i hello källa lagret är *5 * 24 * 24 = 2 880*. 
* Detta är ett vanligt lokala anslutningspoolen lager eftersom **KernelShape** och **Stride** är lika. 
* hello antalet noder i hello mållagret är *5 * 12 * 12 = 1440*.  

Mer information om anslutningspoolen lager finns dessa artiklar:  

* [http://www.CS.Toronto.edu/~hinton/absps/imagenet.PDF](http://www.cs.toronto.edu/~hinton/absps/imagenet.pdf) (punkt 3.4)
* [http://CS.nyu.edu/~koray/publis/lecun-iscas-10.PDF](http://cs.nyu.edu/~koray/publis/lecun-iscas-10.pdf) 
* [http://CS.nyu.edu/~koray/publis/jarrett-iccv-09.PDF](http://cs.nyu.edu/~koray/publis/jarrett-iccv-09.pdf)

## <a name="response-normalization-bundles"></a>Svaret normalisering paket
**Svaret normalisering** är en lokal normalisering schema som introducerades av Geoffrey Hinton et al i hello dokumentet [ImageNet Classiﬁcation med djupa Neurala nätverk för Convolutional](http://www.cs.toronto.edu/~hinton/absps/imagenet.pdf). Svaret normalisering är används tooaid generalisering i neural nät. När en neuron startar på en aktivering av mycket hög nivå, Undertrycker ett lager för lokala svar normalisering hello aktivering andelen hello omgivande neurons. Detta görs med hjälp av tre parametrar (***α***, ***β***, och ***k***) och en convolutional struktur (eller nätverket form). Varje neuron i hello mållagret ***y*** motsvarar tooa neuron ***x*** i hello källa lager. Hej aktivering andelen ***y*** ges genom följande formel, hello där ***f*** är hello aktivering i en neuron och ***Nx*** hello kernel (eller hello uppsättning som innehåller hello neurons i hello nätverket av ***x***), enligt följande convolutional struktur hello:  

![][1]  

Svaret normalisering paket stöder alla hello convolutional attribut utom **delning**, **MapCount**, och **vikterna**.  

* Om hello kernel innehåller neurons i hello mappa samma som ***x***, hello normalisering schemat är refererad tooas **samma mappa normalisering**. toodefine samma mappa normalisering hello första koordinaten i **InputShape** måste ha hello värdet 1.
* Om hello kernel innehåller neurons i hello samma spatial position som ***x***, men hello neurons finns i andra mappar, hello normalisering schemat kallas **tvärs över mappar normalisering**. Den här typen av svar normalisering implementerar en form av laterala inhibition inspirerat av hello typ hittades i verkliga neurons skapar konkurrens om stora aktivering nivåer bland neuron utdata beräknad på olika maps. toodefine över mappar normalisering hello första koordinat måste vara ett heltal större än ett och inte större än hello antalet maps och hello resten av hello koordinater måste ha hello värdet 1.  

Eftersom svaret normalisering paket gäller en fördefinierad funktion toosource nod värden toodetermine hello mål nodvärde, har de inget trainable tillstånd (vikt eller eventuella fördomar).   

**Varning**: hello noder i hello mållagret motsvarar tooneurons som är centrala hello-noder i hello kärnor. Om KernelShape [d] är udda, till exempel *KernelShape [d] / 2* motsvarar toohello centrala kernel-nod. Om *KernelShape [d]* är jämnt, hello centrala noden är på *KernelShape [d] / 2-1*. Därför om **utfyllnad**[d] har värdet False hello först och hello senast *KernelShape [d] / 2* noder har inte motsvarande noder i hello mållagret. tooavoid den här situationen kan definiera **utfyllnad** som [true, true,..., true].  

Dessutom beskrivs toohello fyra attribut tidigare svar normalisering paket också stöd hello följande attribut:  

* **Alpha**: (obligatoriskt) anger ett värde som motsvarar för***α*** i hello tidigare formel. 
* **Beta**: (obligatoriskt) anger ett värde som motsvarar för***β*** i hello tidigare formel. 
* **Förskjutning**: (valfritt) anger ett värde som motsvarar för***k*** i hello tidigare formel. Too1 standardvärdet.  

hello definierar följande exempel ett svar normalisering paket med hjälp av dessa attribut:  

    hidden RN1 [5, 10, 10]
      from P1 response norm {
        InputShape  = [ 5, 12, 12];
        KernelShape = [ 1,  3,  3];
        Alpha = 0.001;
        Beta = 0.75;
      }  

* hello källa layer innehåller fem kartor, med aof dimension 12 x 12 summering 1440 noder. 
* Hej värdet för **KernelShape** anger att detta är en samma normalisering kartskiktet, där hello nätverket är en rektangel 3 x 3. 
* Hej standardvärdet **utfyllnad** är False, vilket hello mållagret har bara 10 noder i varje dimension. tooinclude en nod i hello mållagret som motsvarar tooevery nod i hello källa layer utfyllnad = [true, true, true]; och ändra hello storleken på RN1 för [5, 12, 12].  

## <a name="share-declaration"></a>Dela förklaring
NET # stöder definiera flera paket med delade vikter. hello vikten av alla två paket kan delas om deras strukturer är hello samma. hello följande syntax definierar paket med delade vikter:  

    share-declaration:
        share    {    layer-list    }
        share    {    bundle-list    }
       share    {    bias-list    }

    layer-list:
        layer-name    ,    layer-name
        layer-list    ,    layer-name

    bundle-list:
       bundle-spec    ,    bundle-spec
        bundle-list    ,    bundle-spec

    bundle-spec:
       layer-name    =>     layer-name

    bias-list:
        bias-spec    ,    bias-spec
        bias-list    ,    bias-spec

    bias-spec:
        1    =>    layer-name

    layer-name:
        identifier  

Till exempel hello följande resurs-deklaration anger hello lagernamn som anger att både vikterna och eventuella fördomar ska delas:  

    Const {
      InputSize = 37;
      HiddenSize = 50;
    }
    input {
      Data1 [InputSize];
      Data2 [InputSize];
    }
    hidden {
      H1 [HiddenSize] from Data1 all;
      H2 [HiddenSize] from Data2 all;
    }
    output Result [2] {
      from H1 all;
      from H2 all;
    }
    share { H1, H2 } // share both weights and biases  

* hello inkommande funktioner delas upp i två lika storlek inkommande lager. 
* hello dolda lager för att beräkna högre nivå funktioner på hello två inkommande lager. 
* hello resurs-deklarationen anger att *H1* och *H2* måste beräknas i hello samma sätt från deras respektive indata.  

Du kan också anges detta med två olika resurs-deklarationer enligt följande:  

    share { Data1 => H1, Data2 => H2 } // share weights  

<!-- -->

    share { 1 => H1, 1 => H2 } // share biases  

Du kan använda hello kort form endast när hello lager innehåller ett paket. I allmänhet är delning endast möjligt när hello relevanta strukturen är identiska, vilket innebär att de har hello samma storlek, samma convolutional geometri, och så vidare.  

## <a name="examples-of-net-usage"></a>Exempel på användning av Net #
Det här avsnittet innehåller några exempel på hur du kan använda Net # tooadd dolda lager, definiera hello sätt att dolda lager interagera med andra lager och bygga convolutional nätverk.   

### <a name="define-a-simple-custom-neural-network-hello-world-example"></a>Definiera en enkel anpassade neurala nätverket: ”Hello World”-exempel
Det här enkla exemplet visar hur toocreate en neurala nätverk modellen som har ett dolt lager.  

    input Data auto;
    hidden H [200] from Data all;
    output Out [10] sigmoid from H all;  

hello exemplet vissa grundläggande kommandon på följande sätt:  

* hello första raden definierar hello inkommande lager (med namnet *Data*). När du använder hello **automatisk** nyckelordet hello neurala nätverket inkluderar automatiskt alla kolumner i funktionen i hello inkommande exempel. 
* hello andra raden skapar hello dolda lagret. hello namnet *H* tilldelas toohello dolda lagret, vilket har 200 noder. Det här lagret är fullt ansluten toohello inkommande lager.
* hello tredje raden definierar hello utdata layer (med namnet *O*), som innehåller 10 utdata-noder. Om hello neurala nätverket används för klassificering, är det en utdata nod per klass. Hej nyckelordet **sigmoid** anger hello utdata är tillämpade toohello utdata lager.   

### <a name="define-multiple-hidden-layers-computer-vision-example"></a>Definiera flera dolda lager: datorn vision exempel
hello exemplet nedan visar hur toodefine ett lite mer komplext neurala nätverk med flera anpassade dolda lager.  

    // Define hello input layers 
    input Pixels [10, 20];
    input MetaData [7];

    // Define hello first two hidden layers, using data only from hello Pixels input
    hidden ByRow [10, 12] from Pixels where (s,d) => s[0] == d[0];
    hidden ByCol [5, 20] from Pixels where (s,d) => abs(s[1] - d[1]) <= 1;

    // Define hello third hidden layer, which uses as source hello hidden layers ByRow and ByCol
    hidden Gather [100] 
    {
      from ByRow all;
      from ByCol all;
    }

    // Define hello output layer and its sources
    output Result [10]  
    {
      from Gather all;
      from MetaData all;
    }  

Det här exemplet visar flera funktioner i hello neurala nätverk specifikationsspråk:  

* hello har två inkommande lager *bildpunkter* och *MetaData*.
* Hej *bildpunkter* layer är ett skikt som källa för två anslutning paket, med målet lager *ByRow* och *ByCol*.
* Hej lager *samla in* och *resultatet* är målet lager i flera paket för anslutningen.
* hello utdata layer *resultatet*, är en mållagret i två anslutning paket; en med hello andra nivå dolda (samla) som en mållagret och hello andra hello inkommande nivå (MetaData) som ett mål-lager.
* Hej dolda lager *ByRow* och *ByCol*, ange filtrerad anslutning med hjälp av predikatuttryck. Mer exakt hello nod i *ByRow* på [x, y] är anslutna toohello noder i *bildpunkter* som har hello första index koordinaten lika toohello nodens första samordna x. På liknande sätt hello nod i *ByCol på [x, y] är anslutna toohello noder i _Pixels* som har hello andra index koordinat inom ett hello nodens andra koordinat, y.  

### <a name="define-a-convolutional-network-for-multiclass-classification-digit-recognition-example"></a>Ange ett convolutional nätverk för multiklass-baserad klassificering: siffra recognition exempel
hello definitionen av hello följande nätverk är utformad toorecognize siffror och det visar att vissa avancerade tekniker för att anpassa en neurala nätverket.  

    input Image [29, 29];
    hidden Conv1 [5, 13, 13] from Image convolve 
    {
       InputShape  = [29, 29];
       KernelShape = [ 5,  5];
       Stride      = [ 2,  2];
       MapCount    = 5;
    }
    hidden Conv2 [50, 5, 5]
    from Conv1 convolve 
    {
       InputShape  = [ 5, 13, 13];
       KernelShape = [ 1,  5,  5];
       Stride      = [ 1,  2,  2];
       Sharing     = [false, true, true];
       MapCount    = 10;
    }
    hidden Hid3 [100] from Conv2 all;
    output Digit [10] from Hid3 all;  


* hello-strukturen har ett enda inkommande lager *bild*.
* Hej nyckelordet **convolve** betyder att hello lager som heter *Conv1* och *Conv2* är convolutional lager. Var och en av dessa lager-deklarationer följs av en lista över hello Faltning attribut.
* hello net har en tredje dolda lagret, *Hid3*, som är fullständigt ansluten toohello andra dolda lagret, *Conv2*.
* hello utdata layer *siffra*, är anslutet endast toohello tredje dolda lagret, *Hid3*. Hej nyckelordet **alla** visar hello utdata lagret är helt ansluten för*Hid3*.
* hello aritet av hello Faltning är tre (hello längden på hello tupplar **InputShape**, **KernelShape**, **Stride**, och **delning**). 
* hello antalet vikter per kernel är *1 + **KernelShape**\[0] * **KernelShape**\[1] * **KernelShape** \[2] = 1 + 1 * 5 * 5 = 26. Eller 26 * 50 = 1300*.
* Du kan beräkna hello noder i varje dolda lagret på följande sätt:
  * **NodeCount**\[0] = (5 - 1) / 1 + 1 = 5.
  * **NodeCount**\[1] = (13-5) / 2 + 1 = 5. 
  * **NodeCount**\[2] = (13-5) / 2 + 1 = 5. 
* hello Totalt antal noder kan beräknas med hjälp av hello deklarerats dimensionaliteten för hello layer, [50, 5, 5], enligt följande:  ***MapCount** * **NodeCount** \[ 0] * **NodeCount**\[1] * **NodeCount**\[2] = 10 * 5 * 5 * 5*
* Eftersom **delning**[d] har värdet False för *d == 0*, hello antalet kärnor är  ***MapCount** * **NodeCount** \[0] = 10 * 5 = 50*. 

## <a name="acknowledgements"></a>Bekräftelser
hello Net # språk för att anpassa hello arkitekturen för neurala nätverk har utvecklats på Microsoft av Shon Katzenberger (systemarkitekt, Machine Learning) och Alexey Kamenev (programvara tekniker, Microsoft Research). Det används internt för maskininlärning projekt och program, från avbildningen identifiering tootext analytics. Mer information finns i [Neural nät i Azure ML - introduktion tooNet #](http://blogs.technet.com/b/machinelearning/archive/2015/02/16/neural-nets-in-azure-ml-introduction-to-net.aspx)

[1]:./media/machine-learning-azure-ml-netsharp-reference-guide/formula_large.gif

