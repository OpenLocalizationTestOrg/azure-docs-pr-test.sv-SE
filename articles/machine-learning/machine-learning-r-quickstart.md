---
title: "aaaQuickstart vägledning för R språk för Machine Learning | Microsoft Docs"
description: "Använd den här R programming självstudiekursen tooget igång snabbt med hello R språk med Azure Machine Learning Studio toocreate prognosmodellen lösning."
keywords: "Snabbstart, r språk, r programmeringsspråk, r programming självstudiekursen"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 99a3a0fd-b359-481a-b236-66868deccd96
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: garye
ms.openlocfilehash: 9995f8728f4d7bf9a5c15412015e4cf769cdac96
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="quickstart-tutorial-for-hello-r-programming-language-for-azure-machine-learning"></a>Snabbstartsguide för hello R programmeringsspråk för Azure Machine Learning

<!-- Stephen F Elston, Ph.D. -->

## <a name="introduction"></a>Introduktion
Den här snabbstartsguide hjälper dig att snabbt börja utöka Azure Machine Learning med hjälp av hello R-programmeringsspråket. Följ den här självstudiekursen toocreate för programmering R, testa och köra R-koden i Azure Machine Learning. När du arbetar igenom kursen skapar du en fullständig prognosmodellen lösning med hjälp av hello R språk i Azure Machine Learning.  

Microsoft Azure Machine Learning innehåller många kraftfulla machine learning och data manipulation moduler. hello kraftfulla R språk har beskrivits som hello lingua franca av analytics. Lyckligtvis kan analytics och data manipulation i Azure Machine Learning utökas med hjälp av R. Den här kombinationen ger hello skalbarhet och enkelhet för distribution av Azure Machine Learning hello flexibilitet och djupgående analys av R.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

### <a name="forecasting-and-hello-dataset"></a>Prognoser och hello dataset
Prognoser är en mycket anställda och ganska användbart analytiska metod. Vanliga användningsområden mellan förutsäga försäljning när objekt bestämma optimal inventering nivåer, toopredicting makroekonomiska variabler. Prognoser görs vanligtvis med tiden serie modeller.

Tid series-data är data som hello värden har ett index över tid. hello tid indexet kan vara Normal, t.ex. varje månad eller varje minut eller oregelbundna. En tidsseriemodell baseras på tidpunkt series-data. hello R programmeringsspråket innehåller ett flexibelt ramverk och omfattande analytics för tid series-data.

I den här snabbstartsguide kommer arbeta med California mjölkproduktion och priser data. Dessa data innehåller månatliga information på hello produktion av flera mejeriprodukter och hello pris mjölkfett, en vanlig prestandamått.

hello-data som används i den här artikeln, tillsammans med R-skript kan vara [hämtas här][download]. Dessa data har ursprungligen syntetiskt från information som är tillgängliga från hello University of Wisconsin på http://future.aae.wisc.edu/tab/production.html.

### <a name="organization"></a>Organisation
Vi kommer att gå igenom flera steg som du lär dig hur toocreate, testa och utföra analyser och data manipulation R-koden i hello Azure Machine Learning-miljö.  

* Först ska vi titta närmare hello grunderna i hello R språk i hello Azure Machine Learning Studio-miljön.
* Sedan vidare vi toodiscussing olika aspekter av i/o för data, R-koden och grafik i hello Azure Machine Learning-miljö.
* Vi kommer sedan att konstruera hello första delen av vår prognosmodellen lösning genom att skapa koden för Datarensning och omvandling.
* Med våra data förberedd kommer vi att utföra en analys av hello visar sambandet mellan flera hello variabler i vår datauppsättning.
* Slutligen skapar vi en säsongsbaserade prognosmodellen tidsseriemodell för mjölkproduktion.

## <a id="mlstudio"></a>Interagera med R språk i Machine Learning Studio
Det här avsnittet tar dig igenom grunderna interagerar med hello R programmeringsspråk i hello Machine Learning Studio-miljön. hello R språket tillhandahåller en kraftfullt verktyg toocreate anpassade analytics och data manipulation moduler i hello Azure Machine Learning-miljön.

Jag använder RStudio toodevelop, testa och felsöka R-koden i liten skala. Den här koden är sedan Klipp ut och klistra in i en [köra R-skriptet] [ execute-r-script] modul i Machine Learning Studio redo toorun.  

### <a name="hello-execute-r-script-module"></a>hello köra R-skriptet modul
I Machine Learning Studio R-skript körs inom hello [köra R-skriptet] [ execute-r-script] modul. Ett exempel på hello [köra R-skriptet] [ execute-r-script] modul i Machine Learning Studio illustreras i bild 1.

 ![R programmeringsspråket: hello köra R-skriptet modul har valts i Machine Learning Studio][1]

*Bild 1. hello Machine Learning Studio miljö som visar hello köra R-skriptet modul har valts.*

Hänvisar tooFigure 1 ska vi titta på några av hello viktiga delar av hello Machine Learning Studio-miljön för att arbeta med hello [köra R-skriptet] [ execute-r-script] modul.

* hello moduler i hello experiment som visas i mittenfönstret hello.
* hello övre delen av hello högra fönstret innehåller ett fönster tooview och redigera din R-skript.  
* hello längst ned i högra fönstret visar några egenskaper för hello [köra R-skriptet][execute-r-script]. Du kan visa hello fel och utdata loggar genom att klicka på hello lämpliga platser i den här rutan.

Vi kommer förstås att diskutera hello [köra R-skriptet] [ execute-r-script] mer detaljerat i hello resten av det här dokumentet.

När du arbetar med funktioner för komplexa R rekommenderar jag att redigera, testa och felsöka i RStudio. Precis som med alla programvaruutveckling utöka din kod inkrementellt och testa på små enkla testfall. Sedan Klipp ut och klistra in dina funktioner i fönstret för hello R-skript för hello [köra R-skriptet] [ execute-r-script] modul. Den här metoden kan du tooharness både hello RStudio integrerad utvecklingsmiljö (IDE) och hello kraften i Azure Machine Learning.  

#### <a name="execute-r-code"></a>Kör R-kod
Alla R-koden i hello [köra R-skriptet] [ execute-r-script] modulen utför när du kör hello experiment genom att klicka på hello **kör** knappen. När körningen har slutförts är markerat visas på hello [köra R-skriptet] [ execute-r-script] ikon.

#### <a name="defensive-r-coding-for-azure-machine-learning"></a>Defensiva R kodning för Azure Machine Learning
Om du utvecklar R-koden för, exempelvis en webbtjänst via Azure Machine Learning, bör du definitivt planera hur koden ska hantera indata oväntade data och undantag. toomaintain tydlighetens skull jag har inte inkluderat mycket hello sätt i kontrollerar eller undantagshantering i de flesta hello kodexempel visas. Men när vi går vidare får jag du några exempel på funktioner med hjälp av R: s funktion för undantagshantering.  

Om du behöver en mer komplett behandling av R-undantagshantering rekommenderar du läst hello tillämpliga avsnitt av hello boken av Wickham som anges i [bilaga B - ytterligare läsning](#appendixb).

#### <a name="debug-and-test-r-in-machine-learning-studio"></a>Felsöka och testa R i Machine Learning Studio
tooreiterate, som jag bör du testa och felsöka din R-koden i liten skala i RStudio. Det finns dock fall där du behöver tootrack ned R kodproblem i hello [köra R-skriptet] [ execute-r-script] sig själv. Dessutom är det bra toocheck resultaten i Machine Learning Studio.

Utdata från hello körning av R-koden och på hello Azure Machine Learning-plattformen finns främst i output.log. Ytterligare information som visas i error.log.  

Om ett fel uppstår i Machine Learning Studio när du kör din R-kod, vara din första erhåller toolook på error.log. Den här filen kan innehålla användbar fel meddelanden toohelp du förstå och åtgärda felet. tooview error.log, klicka på **visa felloggen** på hello **egenskapsrutan** för hello [köra R-skriptet] [ execute-r-script] som innehåller hello felet.

Till exempel kört hello efter R-koden med ett odefinierat variabel y i en [köra R-skriptet] [ execute-r-script] modulen:

    x <- 1.0
    z <- x + y

Den här koden misslyckas tooexecute, vilket resulterar i ett feltillstånd. Klicka på **visa felloggen** på hello **egenskapsrutan** ger hello visas i bild 2 visas.

  ![Felmeddelande popup][2]

*Bild 2. Popup-felmeddelandet.*

Det verkar som om vi behöver toolook i output.log toosee hello R felmeddelande. Klicka på hello [köra R-skriptet] [ execute-r-script] och klicka sedan på hello **visa output.log** artikeln på hello **egenskapsrutan** toohello höger. Öppnar ett nytt webbläsarfönster och hello följande visas.

    [Critical]     Error: Error 0063: hello following error occurred during evaluation of R script:
    ---------- Start of error message from R ----------
    object 'y' not found


    object 'y' not found
    ----------- End of error message from R -----------

Det här felmeddelandet innehåller inga överraskningar och tydligt identifierar hello problem.

tooinspect hello värdet för alla objekt i R, kan du skriva ut dessa värden toohello output.log fil. hello regler för att undersöka objektet värden är i stort sett hello samma som i en interaktiv R-session. Om du skriver ett variabelnamn på en rad, kommer hello värdet för hello objektet vara utskrivna toohello output.log filen.  

#### <a name="packages-in-machine-learning-studio"></a>Paket i Machine Learning Studio
Azure Machine Learning innehåller över 350 förinstallerade R språkpaketen. Du kan använda följande kod i hello hello [köra R-skriptet] [ execute-r-script] modulen tooretrieve en lista över hello förinstallerat paket.

    data.set <- data.frame(installed.packages())
    maml.mapOutputPort("data.set")

Om du inte förstår hello sista raden i den här koden för tillfället hello, läsa på. I hello resten av det här dokumentet diskuteras omfattande använda R i hello Azure Machine Learning-miljö.

### <a name="introduction-toorstudio"></a>Introduktion tooRStudio
RStudio är en mycket vanlig IDE för R. Jag använder RStudio för redigering, testa och felsöka vissa hello R-kod som används i den här snabbstartsguide. När R-koden har testats och är klara kan du bara klipp ut och klistra in från hello RStudio editor till en Machine Learning Studio [köra R-skriptet] [ execute-r-script] modul.  

Om du inte har hello R programmeringsspråk som är installerad på den stationära datorn rekommenderar jag du göra det nu. Kostnadsfri nedladdning med öppen källkod R språk finns på hello omfattande R Arkiv nätverk (CRAN) på [http://www.r-project.org/](http://www.r-project.org/). Det finns hämtningsbara filer för Windows, Mac OS x och Linux/UNIX. Välj en närliggande spegling och följer du anvisningarna för hello hämtning. Dessutom innehåller CRAN en mängd användbara analytics och data manipulation paket.

Om du är ny tooRStudio, bör du hämta och installera hello skrivbordsversionen. Du kan hitta hello RStudio ned för Windows, Mac OS x och Linux/UNIX vid http://www.rstudio.com/products/RStudio/. Följ hello-anvisningarna tooinstall RStudio på den stationära datorn.  

En självstudiekurs introduktion tooRStudio är tillgänglig på https://support.rstudio.com/hc/sections/200107586-Using-RStudio.

Jag ange ytterligare information om hur du använder RStudio i [bilaga A][appendixa].  

## <a id="scriptmodule"></a>Hämta data till och från hello köra R-skriptet modul
I det här avsnittet diskuteras hur du hämta data till och från hello [köra R-skriptet] [ execute-r-script] modul. Vi kommer att granska hur toohandle olika datatyper för att läsa in och ut från hello [köra R-skriptet] [ execute-r-script] modul.

hello fullständiga koden för det här avsnittet finns i hello zip-filen som du hämtade tidigare.

### <a name="load-and-check-data-in-machine-learning-studio"></a>Läsa in och kontrollera data i Machine Learning Studio
#### <a id="loading"></a>Läsa in hello dataset
Vi startar genom att läsa in hello **csdairydata.csv** filen till Azure Machine Learning Studio.

* Starta din Azure Machine Learning Studio-miljö.
* Klicka på **+ ny** på hello nedre vänstra på skärmen, Välj **Dataset**.
* Välj **från lokal fil**, och sedan **Bläddra** tooselect hello-filen.
* Kontrollera att du har valt **generiska CSV-fil med rubriken (.csv)** som hello typ för hello dataset.
* Klicka på kryssmarkeringen hello.
* Du bör se hello ny datamängd genom att klicka på hello efter hello datamängden har hämtats, **datauppsättningar** fliken.  

#### <a name="create-an-experiment"></a>Skapa ett experiment
Nu när vi har vissa data i Machine Learning Studio måste toocreate experiment toodo hello analys.  

* Klicka på **+ ny** på hello lägre vänster och välj **Experiment**, sedan **tomt Experiment**.
* Kan du namnge experimentet genom att välja och ändra, hello **Experiment skapas på...**  rubrik överst hello på hello-sidan. Till exempel ändra den för**CA Mejeri Analysis**.
* Hello vänster på sidan för hello experiment, expandera **sparade datauppsättningar**, och sedan **Mina datauppsättningar**. Du bör se hello **cadairydata.csv** som du överfört tidigare.
* Dra och släpp hello **csdairydata.csv dataset** till hello experiment.
* I hello **Sök experimentera objekt** rutan hello överkant hello till vänster och typen [köra R-skriptet][execute-r-script]. Hello-modulen som visas i söklistan hello visas.
* Dra och släpp hello [köra R-skriptet] [ execute-r-script] modul till din utbud.  
* Ansluta hello utdata från hello **csdairydata.csv dataset** toohello längst till vänster indata (**Dataset1**) av hello [köra R-skriptet][execute-r-script].
* **Glöm inte tooclick på 'Spara'!**  

Nu experimentet bör se ut ungefär som bild 3.

![hello CA Mejeri analys experimentera med datauppsättningen och köra R-skriptet modul][3]

*Bild 3. hello CA Mejeri analys experimentera med datauppsättningen och köra R-skriptet modulen.*

#### <a name="check-on-hello-data"></a>Kontrollera på hello data
Låt oss ta en titt på hello data som vi har lästs in i vårt experiment. I hello experiment, klickar du på hello utdata från hello **cadairydata.csv dataset** och välj **visualisera**. Du bör se något liknande bild 4.  

![Sammanfattning av hello cadairydata.csv dataset][4]

*Bild 4. Sammanfattning av hello cadairydata.csv dataset.*

I den här vyn visas mycket användbar information. Vi kan se hello första flera rader i denna dataset. Om vi Markera en kolumn visas hello statistik avsnitt mer information om hello-kolumn. Hello funktionstyp raden visar exempelvis oss vilka datatyper som Azure Machine Learning Studio tilldelade toohello kolumn. Med en snabb ser ut så här är en bra förstånd kontroll innan vi börjar toodo något allvarligt arbete.

### <a name="first-r-script"></a>Första R-skriptet
Nu ska vi skapa en enkel första R-skriptet tooexperiment med i Azure Machine Learning Studio. Jag har skapat och testat hello följande skript i RStudio.  

    ## Only one of hello following two lines should be used
    ## If running in Machine Learning Studio, use hello first line with maml.mapInputPort()
    ## If in RStudio, use hello second line with read.csv()
    cadairydata <- maml.mapInputPort(1)
    # cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
    str(cadairydata)
    pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata)
    ## hello following line should be executed only when running in
    ## Azure Machine Learning Studio
    maml.mapOutputPort('cadairydata')

Nu tootransfer måste det här skriptet tooAzure Machine Learning Studio. Jag kan bara klippa och klistra in. Men i det här fallet överför jag R-skriptet via en zip-fil.

### <a name="data-input-toohello-execute-r-script-module"></a>Data inkommande toohello köra R-skriptet modul
Låt oss ta en titt på hello indata toohello [köra R-skriptet] [ execute-r-script] modul. I det här exemplet ska vi läsa hello California mjölkproducerande data i hello [köra R-skriptet] [ execute-r-script] modul.  

Det finns tre möjliga indata för hello [köra R-skriptet] [ execute-r-script] modul. Du kan använda någon av eller alla dessa indata, beroende på ditt program. Det är perfekt rimliga toouse ett R-skript som tar inga indata alls.  

Nu ska vi titta på var och en av dessa indata från vänster tooright. Du kan se hello namnen på alla hello indata genom att placera markören över hello indata och läsa hello verktygstipset.  

#### <a name="script-bundle"></a>Skript-paket
hello skript paket indata kan du toopass hello innehållet i en zip-fil i [köra R-skriptet] [ execute-r-script] modul. Du kan använda något av följande kommandon tooread hello innehållet i hello zip-filen till din R-kod hello.

    source("src/yourfile.R") # Reads a zipped R script
    load("src/yourData.rdata") # Reads a zipped R data file

> [!NOTE]
> Azure Machine Learning behandlar filer i hello zip som om de finns i hello src / directory, så du måste tooprefix filnamnen med namnet på den här katalogen. Innehåller till exempel om hello zip hello filer `yourfile.R` och `yourData.rdata` i hello rot hello zip, skulle du lösa dessa som `src/yourfile.R` och `src/yourData.rdata` när du använder `source` och `load`.
> 
> 

Vi diskuterade redan inläsning datauppsättningar i [hämtar hello datauppsättning](#loading). När du har skapat och testat hello R-skriptet som visas i hello föregående avsnitt, hello följande:

1. Spara hello R-skriptet i en. R-fil. Jag anropa min skriptfilen ”simpleplot. R ”. Här är hello innehållet.
   
        ## Only one of hello following two lines should be used
        ## If running in Machine Learning Studio, use hello first line with maml.mapInputPort()
        ## If in RStudio, use hello second line with read.csv()
        cadairydata <- maml.mapInputPort(1)
        # cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
        str(cadairydata)
        pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata)
        ## hello following line should be executed only when running in
        ## Azure Machine Learning Studio
        maml.mapOutputPort('cadairydata')
2. Skapa en zip-fil och kopiera skriptet till den här zipfilen. I Windows, kan du högerklicka på hello-fil och välja **skicka till**, och sedan **komprimerad mapp**. Detta skapar en ny zip-fil som innehåller hello ”simpleplot. R ”-fil.
3. Lägg till din fil toohello **datauppsättningar** i Machine Learning Studio, att ange hello typ som **zip**. Du bör nu se hello zip-filen i dina datauppsättningar.
4. Dra och släpp hello zip-filen från **datauppsättningar** till hello **ML Studio arbetsytan**.
5. Ansluta hello utdata från hello **zip data** ikonen toohello **skript paket** indata av hello [köra R-skriptet] [ execute-r-script] modul.
6. Typen hello `source()` funktion i zip-filnamnet i hello kod-fönstret för hello [köra R-skriptet] [ execute-r-script] modul. Min om jag skrivit `source("src/simpleplot.R")`.  
7. Kontrollera att du klickar på **spara**.

När de här stegen är klar kan hello [köra R-skriptet] [ execute-r-script] modulen utför hello R-skriptet i hello zip-filen när hello experiment körs. Nu experimentet bör se ut ungefär som bild 5.

![Experimentera med komprimerade R-skriptet][6]

*Bild 5. Experimentera med komprimerade R-skriptet.*

#### <a name="dataset1"></a>Dataset1
Du kan överföra en rektangulär tabell data tooyour R kod med hello Dataset1 indata. I vår enkelt skript-hello `maml.mapInputPort(1)` funktionen läser hello data från port 1. Dessa data tilldelas sedan tooa dataframe variabelnamn i koden. I vår enkelt skript utför hello första kodrad hello tilldelning.

    cadairydata <- maml.mapInputPort(1)

Kör experimentet genom att klicka på hello **kör** knappen. När hello körningen har slutförts klickar du på hello [köra R-skriptet] [ execute-r-script] modul och klicka sedan på **Visa logg för utdata** på hello egenskapsrutan. En ny sida ska visas i webbläsaren visar hello innehållet i hello output.log fil. När du rullar bör du se något som liknar följande hello.

    [ModuleOutput] InputDataStructure
    [ModuleOutput]
    [ModuleOutput] {
    [ModuleOutput]  "InputName":Dataset1
    [ModuleOutput]  "Rows":228
    [ModuleOutput]  "Cols":9
    [ModuleOutput]  "ColumnTypes":System.Int32,3,System.Double,5,System.String,1
    [ModuleOutput] }

Längre ned hello är mer detaljerad information om hello kolumner som ser ut ungefär så hello följande.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput]
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput]
    [ModuleOutput]  $ Column 0         : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput]
    [ModuleOutput]  $ Year.Month       : num  1995 1995 1995 1995 1995 ...
    [ModuleOutput]
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput]
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput]
    [ModuleOutput]  $ Month            : chr  "Jan" "Feb" "Mar" "Apr" ...
    [ModuleOutput]
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput]
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput]
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput]
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...

De här resultaten returneras är främst som förväntat med 228 observationer och 9 kolumner i hello dataframe. Vi kan se hello kolumnnamn, hello R-datatypen och ett exempel på varje kolumn.

> [!NOTE]
> Den här samma utskriften är bekvämt tillgänglig från hello R enheten utdata från hello [köra R-skriptet] [ execute-r-script] modul. Diskuteras hello utdata för hello [köra R-skriptet] [ execute-r-script] modul i hello nästa avsnitt.  
> 
> 

#### <a name="dataset2"></a>Dataset2
hello är beteendet för hello Dataset2 indata identiska toothat av Dataset1. Med den här indata skickar du en andra rektangulär tabell med data i R-koden. Hej funktionen `maml.mapInputPort(2)`, med hello argument 2 är används toopass dessa data.  

### <a name="execute-r-script-outputs"></a>Köra R-skriptet utdata
#### <a name="output-a-dataframe"></a>Utdata för en dataframe
Du kan spara hello innehållet i ett R-dataframe som en rektangulär tabell via hello resultatet Dataset1 port med hjälp av hello `maml.mapOutputPort()` funktion. Detta görs i vår enkla R-skriptet genom hello följande rad.

    maml.mapOutputPort('cadairydata')

Klicka på hello resultatet Dataset1 utdataporten efter körs hello experiment, och klicka sedan på **visualisera**. Du bör se något liknande bild 6.

![hello visualisering av hello utdata från hello California mjölkproducerande data][7]

*Bild 6. hello visualisering av hello utdata från hello California mjölkproducerande data.*

Den här utdatan ser ut identiska toohello indata, precis som förväntades.  

### <a name="r-device-output"></a>R-enheter
Hej enheten utdata från hello [köra R-skriptet] [ execute-r-script] modulen innehåller meddelanden och grafik. Både standard utdata och standardfel meddelanden från R skickas toohello utdataporten R-enhet.  

tooview hello R enheten utdata, klicka på hello porten och sedan på **visualisera**. Vi kan se hello standardutdata och standardfel från hello R-skriptet på bild 7.

![Standardutdata och standardfel från hello port R-enhet][8]

*Bild 7. Standardutdata och standardfel från hello R enheten port.*

Rulla nedåt vi se hello grafik utdata från våra R-skriptet i figur 8.  

![Grafik utdata från hello port R-enhet][9]

*Figur 8. Grafik utdata från hello R enheten port.*  

## <a id="filtering"></a>Filtrering av data och omvandling
I det här avsnittet ska vi utföra vissa grundläggande data filtrering och transformeringsåtgärder på hello California mjölkproducerande data. Hello slutet av det här avsnittet har vi data i ett format som är lämpliga för att skapa en modell för analys.  

I det här avsnittet kommer vi mer specifikt utföra flera gemensamma data rensning och omvandling av uppgifter: Skriv omvandling ger filtrering på dataframes, lägga till nya beräknade kolumner, och värdet transformationer. Den här bakgrunden hjälper dig att hantera hello många variationer påträffades i verkliga problem.

hello fullständiga R-koden för det här avsnittet finns i hello zip-filen som du hämtade tidigare.

### <a name="type-transformations"></a>Typen omvandlingar
Nu när vi kan läsa hello California mjölkproducerande data in hello R-koden i hello [köra R-skriptet] [ execute-r-script] modulen, behöver vi tooensure att hello data i hello kolumner har hello avsedd typ och format.  

R är ett dynamiskt skrivna språk, vilket innebär att datatyperna är tvingas från en tooanother som krävs. hello atomiska datatyper i R är numeriska logiska och tecknet. hello faktor typen är används toocompactly lagra kategoriska data. Du hittar mer information om datatyper i hello referenser i [bilaga B - ytterligare läsa](#appendixb).

När tabelldata läses in i R från en extern källa, är det alltid en bra idé toocheck hello resulterande typer i hello kolumner. Du kanske vill typtecknet i en kolumn, men i många fall detta kommer att visas som faktor eller vice versa. I annat fall en kolumn som du tror måste vara representeras numeriskt av teckendata, t.ex. Peka nummer '1,23' i stället för 1,23 som ett flyttal.  

Lyckligtvis är det enkelt tooconvert en typ tooanother så länge mappningen är möjliga. Exempelvis kan du konvertera 'Nevada' till ett numeriskt värde, men kan du konvertera den tooa faktor (kategoriska variabel). Ett annat exempel kan du konvertera numeriska 1 till tecknet '1' eller en faktor.  

hello syntax för någon av de här konverteringarna är enkel: `as.datatype()`. Dessa funktioner för konvertering av typen inkludera hello följande.

* `as.numeric()`
* `as.character()`
* `as.logical()`
* `as.factor()`

Titta på hello datatyper hello kolumner som vi indata i föregående avsnitt i hello: alla kolumner är av typen numeriska, förutom hello kolumn med rubriken ”månad”, vilket är av typen tecken. Vi konvertera denna tooa faktor och hello testresultat.  

Jag har tagit bort hello rad som skapats hello scatterplot matris och lägga till en rad konvertera hello ”månad” kolumnen tooa faktor. I mitt experiment kommer jag bara klipp ut och klistra in hello R-koden i hello kod-fönstret för hello [köra R-skriptet] [ execute-r-script] modul. Du kan också uppdatera hello zip-filen och överföra den tooAzure Machine Learning Studio, men det tar flera steg.  

    ## Only one of hello following two lines should be used
    ## If running in Machine Learning Studio, use hello first line with maml.mapInputPort()
    ## If in RStudio, use hello second line with read.csv()
    cadairydata <- maml.mapInputPort(1)
    # cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
    ## Ensure hello coding is consistent and convert column tooa factor
    cadairydata$Month <- as.factor(cadairydata$Month)
    str(cadairydata) # Check hello result
    ## hello following line should be executed only when running in
    ## Azure Machine Learning Studio
    maml.mapOutputPort('cadairydata')

Vi kör den här koden och titta i hello utdata logg för hello R-skriptet. hello relevanta data från hello loggen visas i bild 9.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Column 0         : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year.Month       : num  1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 14 levels "Apr","April",..: 6 5 9 1 11 8 7 3 14 13 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving hello following item(s):  .maml.oport1"

*Bild 9. Sammanfattning av hello dataframe med en faktor variabel.*

hello-typ för månaden ska nu stå '**faktor med 14 nivåer**'. Detta är ett problem Eftersom det finns endast 12 månader hello år. Du kan också kontrollera toosee som hello typ i **visualisera** hello resultatet Dataset port är '**Categorical**'.

hello problemet är att hello-månad-kolumn inte har har ett kodat systematiskt. I vissa fall kallas en månad April och i andra det förkortas april. Vi kan lösa detta problem genom att minska hello sträng too3 tecken. hello kodrad nu ser ut som följande hello:

    ## Ensure hello coding is consistent and convert column tooa factor
    cadairydata$Month <- as.factor(substr(cadairydata$Month, 1, 3))

Kör hello experimentet och visa hello logg för utdata. hello förväntat resultat visas i bild 10.  

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Column 0         : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year.Month       : num  1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving hello following item(s):  .maml.oport1"

*Bild 10. Sammanfattning av hello dataframe med rätt antal faktor nivåer.*

Vår faktor variabeln har nu hello önskad 12 nivåer.

### <a name="basic-data-frame-filtering"></a>Grundläggande data ram filtrering
R dataframes stöder kraftfulla filtreringsfunktioner. Datauppsättningar kan vara deluppsättning med hjälp av logiska filter på rader eller kolumner. I många fall måste avancerade filtervillkor utföras. hello refererar till i [bilaga B - ytterligare läsa](#appendixb) innehåller omfattande exempel på filtrering dataframes.  

Det finns en bit för filtrering ska vi gör i vår datauppsättning. Om du tittar på hello kolumner i hello cadairydata dataframe visas två onödiga kolumner. hello första kolumnen innehåller bara ett radnummer som inte är användbar. hello andra kolumnen Year.Month, innehåller redundant information. Vi kan enkelt utesluta dessa kolumner med hjälp av hello följande R-koden.

> [!NOTE]
> Från nu på i det här avsnittet jag bara visar du hello ytterligare kod som jag lägger till i hello [köra R-skriptet] [ execute-r-script] modul. Jag lägger till varje ny rad **innan** hello `str()` funktion. Jag använder den här funktionen tooverify Mina resultat i Azure Machine Learning Studio.
> 
> 

Jag lägga till följande rad toomy R-koden i hello hello [köra R-skriptet] [ execute-r-script] modul.

    # Remove two columns we do not need
    cadairydata <- cadairydata[, c(-1, -2)]

Kör den här koden i experimentet och kontrollera hello resultatet från hello utdata-loggen. Dessa resultat visas i figur 11.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  7 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving hello following item(s):  .maml.oport1"

*Figur 11. Sammanfattning av hello dataframe med två kolumner som har tagits bort.*

Goda nyheter! Vi får hello förväntat resultat.

### <a name="add-a-new-column"></a>Lägg till en ny kolumn
toocreate tid serie modeller kommer att vara praktiskt toohave en kolumn som innehåller hello månader sedan hello tidsserier hello startades. Vi skapar en ny kolumn 'Month.Count'.

toohelp organisera hello-kod som skapar vi vårt första enkla funktionen `num.month()`. Vi kommer sedan att tillämpa den här funktionen toocreate en ny kolumn i hello dataframe. hello ny kod är som följer.

    ## Create a new column with hello month count
    ## Function toofind hello number of months from hello first
    ## month of hello time series
    num.month <- function(Year, Month) {
      ## Find hello starting year
      min.year  <- min(Year)

      ## Compute hello number of months from hello start of hello time series
      12 * (Year - min.year) + Month - 1
    }

    ## Compute hello new column for hello dataframe
    cadairydata$Month.Count <- num.month(cadairydata$Year, cadairydata$Month.Number)

Nu kör hello uppdateras experiment och Använd hello loggen tooview hello resultatet. Dessa resultat visas i figur 12.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  8 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving hello following item(s):  .maml.oport1"

*Figur 12. Sammanfattning av hello dataframe med hello ytterligare kolumn.*

Det verkar som att allt fungerar. Vi har hello ny kolumn med hello förväntade värden i vår dataframe.

### <a name="value-transformations"></a>Värdet omvandlingar
I det här avsnittet utför vi några enkla omformningar på hello värden i vissa hello kolumner i vår dataframe. hello R-språket stöder nästan godtyckligt värde transformationer. hello refererar till i [bilaga B - ytterligare läsning](#appendixb) innehåller omfattande exempel.

Om du tittar på hello värden i hello sammanfattningar av våra dataframe bör du se något udda här. Flera glass än mjölk produceras i Kalifornien? Nej, förstås eftersom detta är meningslös tråkigt som detta faktum kan inte toosome oss glass älskare. hello enheter är olika. hello priset som anges i enheter av oss pund mjölk är i enheter om 1 miljon USA pund, glass är i enheter om 1 000 oss gallon och Keso är i enheter om 1 000 USA pund. Under förutsättning att glass väger om 6.5 pund per gallon, vi kan enkelt hello multiplikation tooconvert dessa värden så att de är på 1 000 pund lika enheter.

För vår prognosmodellen använder vi en Multiplicerande modell för trend och säsongsbaserade justering av dessa data. En logg omvandling kan vi toouse en linjär modell, förenkla den här processen. Vi kan använda hello loggen omvandling i hello samma fungerar där hello multiplikator används.

I följande kod hello, jag definierar en ny funktion `log.transform()`, och tillämpa det toohello rader som innehåller hello numeriska värden. hello R `Map()` funktionen är används tooapply hello `log.transform()` funktionen toohello valda kolumner i hello dataframe. `Map()`är liknande för`apply()` , men ger mer än en lista över argument toohello funktion. Observera att en lista över multiplikatorer tillhandahåller hello andra argumentet toohello `log.transform()` funktion. Hej `na.omit()` funktion används som en bit för rensning av tooensure vi har inte saknas eller odefinierad värden i hello dataframe.

    log.transform <- function(invec, multiplier = 1) {
      ## Function for hello transformation, which is hello log
      ## of hello input value times a multiplier

      warningmessages <- c("ERROR: Non-numeric argument encountered in function log.transform",
                           "ERROR: Arguments toofunction log.transform must be greate than zero",
                           "ERROR: Aggurment multiplier toofuncition log.transform must be a scaler",
                           "ERROR: Invalid time seies value encountered in function log.transform"
                           )

      ## Check hello input arguments
      if(!is.numeric(invec) | !is.numeric(multiplier)) {warning(warningmessages[1]); return(NA)}  
      if(any(invec < 0.0) | any(multiplier < 0.0)) {warning(warningmessages[2]); return(NA)}
      if(length(multiplier) != 1) {{warning(warningmessages[3]); return(NA)}}

      ## Wrap hello multiplication in tryCatch
      ## If there is an exception, print hello warningmessage to
      ## standard error and return NA
      tryCatch(log(multiplier * invec),
               error = function(e){warning(warningmessages[4]); NA})
    }


    ## Apply hello transformation function toohello 4 columns
    ## of hello dataframe with production data
    multipliers  <- list(1.0, 6.5, 1000.0, 1000.0)
    cadairydata[, 4:7] <- Map(log.transform, cadairydata[, 4:7], multipliers)

    ## Get rid of any rows with NA values
    cadairydata <- na.omit(cadairydata)  

Det finns en bit inträffar i hello `log.transform()` funktion. De flesta av den här koden söker efter potentiella problem med hello argument eller om undantag som fortfarande kan uppstå under hello beräkningar. Endast några få rader med den här koden kan faktiskt hello beräkningar.

hello målet hello defensiva programmering är tooprevent hello fel i en funktion som förhindrar bearbetningen fortsätter. Ett abrupt fel i en tidskrävande analys kan vara ganska frustrerande för användare. i den här situationen returvärden måste väljas som standard begränsar tooavoid skada toodownstream bearbetning. Ett meddelande är också producerade tooalert användare som något är fel.

Om du inte använt toodefensive programmering i R kan den här koden verka lite överväldigande. Jag tar dig igenom hello Huvudsteg:

1. En vektor med fyra meddelanden har definierats. Dessa meddelanden är används toocommunicate information om några av hello eventuella fel och undantag som kan uppstå med koden.
2. Jag returnera ett värde na för varje fall. Det finns många möjligheter som kan ha färre sidoeffekter. Jag kunde returnera en vektor för nollor eller hello ursprungliga inkommande vector, t.ex.
3. Kontroller körs på hello argument toohello funktion. Om ett fel upptäcks ett standardvärde returneras i varje fall och ett meddelande som genereras av hello `warning()` funktion. Jag använder `warning()` snarare än `stop()` som hello senare avslutas körning och exakt vad jag försök tooavoid. Observera att jag har skrivit koden i samma format som det här fallet en funktionell metod som visat sig komplexa och otydligt.
4. hello loggen beräkningar placeras i `tryCatch()` så att undantag som inte orsakar en abrupt stopp tooprocessing. Utan `tryCatch()` de flesta fel som skapats av R funktioner resultera i en stoppsignal som utför precis så.

Kör den här koden för R i experimentet och har en titt på hello ut utdata i hello output.log-filen. Du ser nu hello omvandlas värdena för hello fyra kolumner i hello loggar, som visas i figur 13.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  8 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving hello following item(s):  .maml.oport1"

*Figur 13. Sammanfattning av hello omvandlas värden i hello dataframe.*

Vi kan se hello värden har transformerats. Nu mjölkproduktion överskrider alla andra mejeriprodukter produktion, återkalla att vi nu titta på en logaritmisk skala.

Nu våra data rensas och vi är klara för vissa modellering. Titta på hello visualiseringen sammanfattning för hello resultatet Dataset som utdata för våra [köra R-skriptet] [ execute-r-script] modulen, ser du hello ”månad” kolumnen 'Categorical' med 12 unika värden igen, precis som vi vill .

## <a id="timeseries"></a>Tid serie objekt och korrelation analys
I det här avsnittet kommer vi utforska några grundläggande R tid serie objekt och analysera hello visar sambandet mellan vissa hello variabler. Vårt mål är toooutput en dataframe som innehåller hello pairwise korrelation information på flera beräkningstider.

hello fullständiga R-koden för det här avsnittet finns i hello zip-filen som du hämtade tidigare.

### <a name="time-series-objects-in-r"></a>Tid serie objekt i R
Serien är en serie datavärden som indexeras av tid som redan nämnts, tid. R tid serie objekt är används toocreate och hantera hello tid index. Det finns flera fördelar toousing tid serie objekt. Tid serie objekt ledigt du från hello många detaljer för att hantera hello tid index serievärden som är inkapslade i hello-objektet. Dessutom tid serien objekt gör toouse hello många tid serie metoder för att rita upp, skriva ut modellering osv.

Hej POSIXct tid serien klassen används ofta och är relativt enkel. Den här gången serien klassen mäter tid från hello början epok hello, 1 januari 1970. I det här exemplet ska vi använda POSIXct tid serie objekt. Andra vanliga objektklasser för R tid serien omfattar zoo och xts, extensible tidsserier.
<!-- Additional information on R time series objects is provided in hello references in Section 5.7. [commenting because this section doesn't exist, even in hello original] -->

### <a name="time-series-object-example"></a>Tid serien objektet exempel
Nu sätter vi igång med våra exempel. Dra och släpp en **nya** [köra R-skriptet] [ execute-r-script] modul i experimentet. Ansluta hello resultatet Dataset1 utdataporten för hello befintliga [köra R-skriptet] [ execute-r-script] modulen toohello Dataset1 ange port för hello nya [köra R-skriptet] [ execute-r-script] modul.

Som jag gjorde hello första exempel som vi förlopp genom hello exempel vid vissa tidpunkter som jag visar endast hello inkrementell fler rader med R-koden i varje steg.  

#### <a name="reading-hello-dataframe"></a>Läsa hello dataframe
Som ett första steg bör vi läses in en dataframe och se till att vi får hello förväntat resultat. hello följande kod ska göra hello jobb.

    # Comment hello following if using RStudio
    cadairydata <- maml.mapInputPort(1)
    str(cadairydata) # Check hello results

Kör nu hello experiment. hello logg över hello nya köra R-skriptet formen ska se ut som figur 14.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  8 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...

*Figur 14. Sammanfattning av hello dataframe i hello köra R-skriptet modul.*

Dessa data är av hello förväntade typer och format. Observera att hello ”månad” kolumnen är av typen faktor och har hello förväntat antal nivåer.

#### <a name="creating-a-time-series-object"></a>Skapa en serie tidsobjekt
Vi behöver tooadd en gång serien objektet tooour dataframe. Ersätt hello aktuella koden med följande hello, vilket lägger till en ny kolumn i klassen POSIXct.

    # Comment hello following if using RStudio
    cadairydata <- maml.mapInputPort(1)

    ## Create a new column as a POSIXct object
    Sys.setenv(TZ = "PST8PDT")
    cadairydata$Time <- as.POSIXct(strptime(paste(as.character(cadairydata$Year), "-", as.character(cadairydata$Month.Number), "-01 00:00:00", sep = ""), "%Y-%m-%d %H:%M:%S"))

    str(cadairydata) # Check hello results

Kontrollera nu hello-loggen. Det bör se ut figur 15.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Time             : POSIXct, format: "1995-01-01" "1995-02-01" ...

*Figur 15. Sammanfattning av hello dataframe med en serie tidsobjekt.*

Vi kan se från hello översikt över den nya kolumnen hello har i själva verket klassen POSIXct.

### <a name="exploring-and-transforming-hello-data"></a>Utforska och omvandla hello data
Vi utforska några av hello variabler i denna dataset. En matris med scatterplot är ett bra sätt tooproduce en titt på. Jag ersätta hello `str()` funktion i hello tidigare R-koden med följande rad hello.

    pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata, main = "Pairwise Scatterplots of dairy time series")

Kör den här koden och se vad som händer. hello ritytans produceras på hello R enheten port bör se ut som bild 16.

![Scatterplot matris för valda variabler][17]

*Bild 16. Scatterplot matris för valda variabler.*

Det finns vissa odd-looking strukturen i hello relationer mellan dessa variabler. Detta inträffar kanske från trender i hello data och hello faktum att vi inte har standardiserats hello variabler.

### <a name="correlation-analysis"></a>Korrelation analys
tooperform korrelation analys vi behöver tooboth Frigör trender och standardisera hello variabler. Vi kan bara använda hello R `scale()` funktion, vilket både datacenter och skalas variabler. Den här funktionen kan också snabbare. Men jag vill tooshow du ett exempel på defensiva programing i R.

Hej `ts.detrend()` funktionen nedan utför båda av dessa åtgärder. hello följande två rader med kod Frigör trend hello data och standardisera hello värden.

    ts.detrend <- function(ts, Time, min.length = 3){
      ## Function toode-trend and standardize a time series

      ## Define some messages if they are NULL  
      messages <- c('ERROR: ts.detrend requires arguments ts and Time toohave hello same length',
                    'ERROR: ts.detrend requires argument ts toobe of type numeric',
                    paste('WARNING: ts.detrend has encountered a time series with length less than', as.character(min.length)),
                    'ERROR: ts.detrend has encountered a Time argument not of class POSIXct',
                    'ERROR: Detrend regression has failed in ts.detrend',
                    'ERROR: Exception occurred in ts.detrend while standardizing time series in function ts.detrend'
      )
      # Create a vector of zeros tooreturn as a default in some cases
      zerovec  <- rep(length(ts), 0.0)

      # hello input arguments are not of hello same length, return ts and quit
      if(length(Time) != length(ts)) {warning(messages[1]); return(ts)}

      # If hello ts is not numeric, just return a zero vector and quit
      if(!is.numeric(ts)) {warning(messages[2]); return(zerovec)}

      # If hello ts is too short, just return it and quit
      if((ts.length <- length(ts)) < min.length) {warning(messages[3]); return(ts)}

      ## Check that hello Time variable is of class POSIXct
      if(class(cadairydata$Time)[[1]] != "POSIXct") {warning(messages[4]); return(ts)}

      ## De-trend hello time series by using a linear model
      ts.frame  <- data.frame(ts = ts, Time = Time)
      tryCatch({ts <- ts - fitted(lm(ts ~ Time, data = ts.frame))},
               error = function(e){warning(messages[5]); zerovec})

      tryCatch( {stdev <- sqrt(sum((ts - mean(ts))^2))/(ts.length - 1)
                 ts <- ts/stdev},
                error = function(e){warning(messages[6]); zerovec})

      ts
    }  
    ## Apply hello detrend.ts function toohello variables of interest
    df.detrend <- data.frame(lapply(cadairydata[, 4:7], ts.detrend, cadairydata$Time))

    ## Plot hello results toolook at hello relationships
    pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = df.detrend, main = "Pairwise Scatterplots of detrended standardized time series")

Det finns en bit inträffar i hello `ts.detrend()` funktion. De flesta av den här koden söker efter potentiella problem med hello argument eller om undantag som fortfarande kan uppstå under hello beräkningar. Endast några få rader med den här koden kan faktiskt hello beräkningar.

Vi har redan beskrivs ett exempel på defensiva programmering i [värdet transformationer](#valuetransformations). Båda beräkning block placeras i `tryCatch()`. För vissa fel gör det klokt tooreturn hello ursprungliga inkommande vector och i andra fall måste du återgår en vector nollor.  

Observera att en tid serien regression hello linjär regression används för att ta bort trender. hello ge prognoser variabeln är en serie tidsobjekt.  

En gång `ts.detrend()` definieras det använda toohello variabler av intresse för vår dataframe. Vi måste tvinga hello resulterande lista som skapats med `lapply()` toodata dataframe med hjälp av `as.data.frame()`. På grund av defensiva aspekter av `ts.detrend()`, fel tooprocess en av hello variabler inte kommer att korrigera bearbetningen av hello andra.  

hello slutliga kodrad skapar pairwise scatterplot. När du har kört hello R kod visas hello resultaten av hello scatterplot i bild 17.

![Pairwise scatterplot tidsseries Frigör daglig och standardiserad][18]

*Figur 17. Pairwise scatterplot tidsseries Frigör daglig och standardiserad.*

Du kan jämföra dessa resultat toothose som visas i bild 16. Med hello trend tas bort och hello variabler som har standardiserats vi se mycket mindre struktur i hello relationer mellan dessa variabler.

hello kod toocompute hello korrelationer som R ccf objekt är som följer.

    ## A function toocompute pairwise correlations from a
    ## list of time series value vectors
    pair.cor <- function(pair.ind, ts.list, lag.max = 1, plot = FALSE){
      ccf(ts.list[[pair.ind[1]]], ts.list[[pair.ind[2]]], lag.max = lag.max, plot = plot)
    }

    ## A list of hello pairwise indices
    corpairs <- list(c(1,2), c(1,3), c(1,4), c(2,3), c(2,4), c(3,4))

    ## Compute hello list of ccf objects
    cadairycorrelations <- lapply(corpairs, pair.cor, df.detrend)  

    cadairycorrelations

Kör den här koden genererar hello loggen visas i bild 18.

    [ModuleOutput] Loading objects:
    [ModuleOutput]   port1
    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] [[1]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]    -1     0     1 
    [ModuleOutput] 0.148 0.358 0.317 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[2]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]     -1      0      1 
    [ModuleOutput] -0.395 -0.186 -0.238 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[3]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]     -1      0      1 
    [ModuleOutput] -0.059 -0.089 -0.127 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[4]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]    -1     0     1 
    [ModuleOutput] 0.140 0.294 0.293 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[5]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]     -1      0      1 
    [ModuleOutput] -0.002 -0.074 -0.124 

*Bild 18. Lista över ccf objekt från hello pairwise korrelation analys.*

Det finns en correlation-värdet för varje fördröjning. Inget av värdena korrelation är tillräckligt stor toobe betydande. Vi kan därför ingå att vi kan modellen varje variabel oberoende av varandra.

### <a name="output-a-dataframe"></a>Utdata för en dataframe
Vi har beräknats hello pairwise korrelationer som en lista över R ccf objekt. Detta innebär lite problem som hello resultatet Dataset utdataporten verkligen kräver en dataframe. Dessutom hello ccf objekt i sin tur är en lista och vi vill bara hello värden i hello första elementet i den här listan, hello korrelationer på hello olika beräkningstider.

hello följande utdrag hello fördröjning värden från hello listan över ccf objekt som själva listor.

    df.correlations <- data.frame(do.call(rbind, lapply(cadairycorrelations, '[[', 1)))

    c.names <- c("correlation pair", "-1 lag", "0 lag", "+1 lag")
    r.names  <- c("Corr Cot Cheese - Ice Cream",
                  "Corr Cot Cheese - Milk Prod",
                  "Corr Cot Cheese - Fat Price",
                  "Corr Ice Cream - Mik Prod",
                  "Corr Ice Cream - Fat Price",
                  "Corr Milk Prod - Fat Price")

    ## Build a dataframe with hello row names column and the
    ## correlation data frame and assign hello column names
    outframe <- cbind(r.names, df.correlations)
    colnames(outframe) <- c.names
    outframe


    ## WARNING!
    ## hello following line works only in Azure Machine Learning
    ## When running in RStudio, this code will result in an error
    #maml.mapOutputPort('outframe')

hello första rad med kod är helt lätt och förklaras kan hjälpa dig att förstå den. Arbeta från hello utan och innan har vi hello följande:

1. hello '**[[**-operator med hello argumentet'**1**' väljer hello vektor med korrelationer på hello LACP från hello första elementet i listan över hello ccf grupprincipobjekt.
2. Hej `do.call()` funktion gäller hello `rbind()` över hello element i listan över hello returneras av `lapply()`.
3. Hej `data.frame()` funktionen tvingar hello resultatet som produceras av `do.call()` tooa dataframe.

Observera att hello raden namn i en kolumn i hello dataframe. Detta bevarar hello raden namn när de är utdata från hello [köra R-skriptet][execute-r-script].

Köra hello kod ger hello utdata som visas i figur 19 när jag **visualisera** hello utdata på hello resultatet Dataset-port. hello raden namn är i första kolumnen hello som avsett.

![Resultaten utdata från hello korrelation analys][20]

*Bild 19. Utdata från hello korrelation analys resultat.*

## <a id="seasonalforecasting"></a>Tid serien exempel: när prognoser
Våra data är nu i ett formulär som är lämplig för analys och vi gjort bedömningen att det finns inga betydande korrelationer mellan hello variabler. Nu ska vi gå vidare och skapa en tidsserie prognoser modellen. Den här modellen kommer vi prognos California mjölkproduktion för hello 12 månaders 2013.

Vår prognosmodellen har två komponenter, en komponent som trend och när komponenten. hello fullständig prognos är hello produkten av dessa två komponenter. Den här typen av modellen kallas en Multiplicerande modell. hello alternativ är en additiva modell. Vi har redan tillämpats loggen omvandling toohello variabler av intresse, vilket gör den här analysen tractable.

hello fullständiga R-koden för det här avsnittet finns i hello zip-filen som du hämtade tidigare.

### <a name="creating-hello-dataframe-for-analysis"></a>Skapa hello dataframe för analys
Starta genom att lägga till en **nya** [köra R-skriptet] [ execute-r-script] modulen tooyour experiment. Ansluta hello **resultatet Dataset** utdata från hello befintliga [köra R-skriptet] [ execute-r-script] modulen toohello **Dataset1** indata av hello ny modul. hello resultatet ska se ut ungefär 20 bild.

![hello experimentera med hello nya köra R-skriptet modulen som lagts till][21]

*Figur 20. hello experimentera med hello nya köra R-skriptet modulen som lagts till.*

Som med hello korrelation analysen vi precis slutfört måste vi tooadd en kolumn med en serie tidsobjekt POSIXct. följande kod hello görs bara detta.

    # If running in Machine Learning Studio, uncomment hello first line with maml.mapInputPort()
    cadairydata <- maml.mapInputPort(1)

    ## Create a new column as a POSIXct object
    Sys.setenv(TZ = "PST8PDT")
    cadairydata$Time <- as.POSIXct(strptime(paste(as.character(cadairydata$Year), "-", as.character(cadairydata$Month.Number), "-01 00:00:00", sep = ""), "%Y-%m-%d %H:%M:%S"))

    str(cadairydata)

Kör den här koden och titta i hello logg. hello resultatet bör se ut som bild 21.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Time             : POSIXct, format: "1995-01-01" "1995-02-01" ...

*Figur 21. En sammanfattning av hello dataframe.*

Med det här resultatet vi är klara toostart vår analys.

### <a name="create-a-training-dataset"></a>Skapa en datauppsättning för träning
Vi behöver toocreate en datauppsättning för träning med hello dataframe konstrueras. Dessa data tas alla hello observationer förutom hello senaste 12 årets hello 2013, vilket är vår testdata. hello följande kod delmängder hello dataframe och skapar områden av hello mjölkproducerande produktions- och variabler. Jag skapar sedan områden i hello fyra produktions- och variabler. En anonym funktion är används toodefine vissa förstärker för område, och sedan iterera över hello lista över hello andra två argument med `Map()`. Om du tänker som en för loop skulle har arbetat bra här, är korrekta. Men eftersom R är ett funktionellt språk jag visar du en funktionell metod.

    cadairytrain <- cadairydata[1:216, ]

    Ylabs  <- list("Log CA Cotage Cheese Production, 1000s lb",
                   "Log CA Ice Cream Production, 1000s lb",
                   "Log CA Milk Production 1000s lb",
                   "Log North CA Milk Milk Fat Price per 1000 lb")

    Map(function(y, Ylabs){plot(cadairytrain$Time, y, xlab = "Time", ylab = Ylabs, type = "l")}, cadairytrain[, 4:7], Ylabs)

Köra hello kod producerar hello serie tidsserier ritas från hello R enheten utdata som visas i figur 22. Observera att hello tidsaxeln i enheter av datum, en bra fördel hello tid serien ritas metod.

![Första gången serien områden i Kalifornien mjölkproducerande produktions- och data](./media/machine-learning-r-quickstart/unnamed-chunk-161.png)

![Andra av tid serien områden i Kalifornien mjölkproducerande produktions- och data](./media/machine-learning-r-quickstart/unnamed-chunk-162.png)

![Tredje tid serien områden av California mjölkproducerande produktions- och data](./media/machine-learning-r-quickstart/unnamed-chunk-163.png)

![Fjärde tid serien områden av California mjölkproducerande produktions- och data](./media/machine-learning-r-quickstart/unnamed-chunk-164.png)

*Figur 22. Tid serien områden av California mjölkproduktion och price data.*

### <a name="a-trend-model"></a>En modell för trend
Att ha skapat en serie tidsobjekt och har haft en titt på hello data kan börja tooconstruct en trend modell för hello California mjölk produktionsdata. Vi kan göra detta med en tid serien regression. Det är dock av hello ritytans att vi kommer behöver mer än en lutning och skärningspunkt tooaccurately modell hello observerats trend i hello utbildningsdata.

Angivna hello liten skala hello data kommer jag skapa hello modell för trender i RStudio och sedan klippa och klistra in hello resulterande modellen i Azure Machine Learning. RStudio ger en interaktiv miljö för den här typen av interaktiv analys.

Som ett första försöket försöker jag en polynom regression med startas too3. Det finns en verklig risk för anpassning över dessa typer av modeller. Därför är det bästa tooavoid högsta villkoren. Hej `I()` funktionen hindrar beslutsträdets tolkning av hello innehållet (tolkar hello innehållet 'som är') och tillåter toowrite bokstavligt tolkad funktion i en regression formel.

    milk.lm <- lm(Milk.Prod ~ Time + I(Month.Count^2) + I(Month.Count^3), data = cadairytrain)
    summary(milk.lm)

Detta genererar hello följande.

    ##
    ## Call:
    ## lm(formula = Milk.Prod ~ Time + I(Month.Count^2) + I(Month.Count^3),
    ##     data = cadairytrain)
    ##
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max
    ## -0.12667 -0.02730  0.00236  0.02943  0.10586
    ##
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)
    ## (Intercept)       6.33e+00   1.45e-01   43.60   <2e-16 ***
    ## Time              1.63e-09   1.72e-10    9.47   <2e-16 ***
    ## I(Month.Count^2) -1.71e-06   4.89e-06   -0.35    0.726
    ## I(Month.Count^3) -3.24e-08   1.49e-08   -2.17    0.031 *  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ##
    ## Residual standard error: 0.0418 on 212 degrees of freedom
    ## Multiple R-squared:  0.941,    Adjusted R-squared:  0.94
    ## F-statistic: 1.12e+03 on 3 and 212 DF,  p-value: <2e-16

Från P värden (Pr (> | t |)) i utdata, kan vi se att hello kvadraten termen får inte vara betydande. Jag använder hello `update()` fungerar toomodify som den här modellen genom att släppa hello kvadrat har löpt ut.

    milk.lm <- update(milk.lm, . ~ . - I(Month.Count^2))
    summary(milk.lm)

Detta genererar hello följande.

    ##
    ## Call:
    ## lm(formula = Milk.Prod ~ Time + I(Month.Count^3), data = cadairytrain)
    ##
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max
    ## -0.12597 -0.02659  0.00185  0.02963  0.10696
    ##
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)
    ## (Intercept)       6.38e+00   4.07e-02   156.6   <2e-16 ***
    ## Time              1.57e-09   4.32e-11    36.3   <2e-16 ***
    ## I(Month.Count^3) -3.76e-08   2.50e-09   -15.1   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ##
    ## Residual standard error: 0.0417 on 213 degrees of freedom
    ## Multiple R-squared:  0.941,  Adjusted R-squared:  0.94
    ## F-statistic: 1.69e+03 on 2 and 213 DF,  p-value: <2e-16

Detta ser bättre ut. Alla hello villkoren är betydande. Dock hello 2e 16 värde är ett standardvärde och ska inte tas med för allvarligt.  

Förstånd test, se en serie åker tid hello California mjölkproducerande data med hello trend kurva visas. Jag har lagt till följande kod i hello Azure Machine Learning hello [köra R-skriptet] [ execute-r-script] modellen (inte RStudio) toocreate hello modellen och göra en rityta. hello resultat visas i figur 23.

    milk.lm <- lm(Milk.Prod ~ Time + I(Month.Count^3), data = cadairytrain)

    plot(cadairytrain$Time, cadairytrain$Milk.Prod, xlab = "Time", ylab = "Log CA Milk Production 1000s lb", type = "l")
    lines(cadairytrain$Time, predict(milk.lm, cadairytrain), lty = 2, col = 2)

![California mjölk produktionsdata med trend modell visas](./media/machine-learning-r-quickstart/unnamed-chunk-18.png)

*Figur 23. California mjölk produktionsdata med trend modell visas.*

Det verkar som hello trend modell passar data hello ganska bra. Dessutom verkar det inte toobe bevis av överdrivet passning som udda wiggles i hello modellen kurvan.  

### <a name="seasonal-model"></a>När modellen
Med en trend modell i hand vi behöver toopush på och inkludera hello säsongsbaserade effekter. Vi använder hello månad hello som en dummy variabel i hello linjär modell toocapture hello per månad effekt. Observera att när du införa faktor variabler i en modell hello skärningspunkt inte måste beräknas. Om du inte göra detta, hello formeln är felaktigt angivna och R släpper en hello önskad faktorer men behålla hello skärningspunkt termen.

Eftersom vi har en tillfredsställande trend modell kan vi använda hello `update()` funktionen tooadd hello nya termer toohello befintlig modell. hello -1 i hello update formel utelämnar hello skärningspunkt termen. Om du fortsätter i RStudio hello ögonblick:

    milk.lm2 <- update(milk.lm, . ~ . + Month - 1)
    summary(milk.lm2)

Detta genererar hello följande.

    ##
    ## Call:
    ## lm(formula = Milk.Prod ~ Time + I(Month.Count^3) + Month - 1,
    ##     data = cadairytrain)
    ##
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max
    ## -0.06879 -0.01693  0.00346  0.01543  0.08726
    ##
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)
    ## Time              1.57e-09   2.72e-11    57.7   <2e-16 ***
    ## I(Month.Count^3) -3.74e-08   1.57e-09   -23.8   <2e-16 ***
    ## MonthApr          6.40e+00   2.63e-02   243.3   <2e-16 ***
    ## MonthAug          6.38e+00   2.63e-02   242.2   <2e-16 ***
    ## MonthDec          6.38e+00   2.64e-02   241.9   <2e-16 ***
    ## MonthFeb          6.31e+00   2.63e-02   240.1   <2e-16 ***
    ## MonthJan          6.39e+00   2.63e-02   243.1   <2e-16 ***
    ## MonthJul          6.39e+00   2.63e-02   242.6   <2e-16 ***
    ## MonthJun          6.38e+00   2.63e-02   242.4   <2e-16 ***
    ## MonthMar          6.42e+00   2.63e-02   244.2   <2e-16 ***
    ## MonthMay          6.43e+00   2.63e-02   244.3   <2e-16 ***
    ## MonthNov          6.34e+00   2.63e-02   240.6   <2e-16 ***
    ## MonthOct          6.37e+00   2.63e-02   241.8   <2e-16 ***
    ## MonthSep          6.34e+00   2.63e-02   240.6   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ##
    ## Residual standard error: 0.0263 on 202 degrees of freedom
    ## Multiple R-squared:     1,    Adjusted R-squared:     1
    ## F-statistic: 1.42e+06 on 14 and 202 DF,  p-value: <2e-16

Vi se den hello modellen inte längre har en skärningspunkt term och 12 betydande månad faktorer. Detta är exakt vad vi vill toosee.

Vi behöver kontrollera en annan tid serien område för hello California mjölkproducerande data toosee hur väl hello när modellen fungerar. Jag har lagt till följande kod i hello Azure Machine Learning hello [köra R-skriptet] [ execute-r-script] toocreate hello modellen och göra en rityta.

    milk.lm2 <- lm(Milk.Prod ~ Time + I(Month.Count^3) + Month - 1, data = cadairytrain)

    plot(cadairytrain$Time, cadairytrain$Milk.Prod, xlab = "Time", ylab = "Log CA Milk Production 1000s lb", type = "l")
    lines(cadairytrain$Time, predict(milk.lm2, cadairytrain), lty = 2, col = 2)

Kör den här koden i Azure Machine Learning ger hello ritytans visas i figur 24.

![California mjölkproduktion med modellen inklusive säsongsbaserade effekter](./media/machine-learning-r-quickstart/unnamed-chunk-20.png)

*Figur 24. California mjölkproduktion med modellen inklusive säsongsbaserade effekter.*

hello är anpassa toohello data som visas i figur 24 ganska uppmuntra. Både hello trend och hello när gälla (månatliga variation) ser rimliga.

Som en annan kontroll av vår modell ska vi ta en titt på hello residualer. hello följande kod beräknar hello förutsagda värden från våra två modeller, beräknar hello residualer för hello när modellen och ritar dessa residualer för hello utbildningsdata.

    ## Compute predictions from our models
    predict1  <- predict(milk.lm, cadairydata)
    predict2  <- predict(milk.lm2, cadairydata)

    ## Compute and plot hello residuals
    residuals <- cadairydata$Milk.Prod - predict2
    plot(cadairytrain$Time, residuals[1:216], xlab = "Time", ylab ="Residuals of Seasonal Model")

hello kvarvarande ritytans illustreras i bild 25.

![Residualer i hello när modellen för hello utbildningsdata](./media/machine-learning-r-quickstart/unnamed-chunk-21.png)

*Bild 25. Residualer i hello när modellen för hello utbildningsdata.*

Dessa residualer leta rimliga. Det finns inga särskilda struktur, utom hello effekten av hello 2008-2009 nedgång som vår modell ingen hänsyn tas till särskilt väl.

hello ritytans visas i bild 25 är användbart för att upptäcka eventuella tid beroende mönster i hello residualer. hello explicit metod datoranvändning och rita hello restvärden jag använde placerar hello residualer i tid ordning på hello ritytans. Om på hello däremot hade funktion `milk.lm$residuals`, skulle inte ha varit hello ritytans i tid ordning.

Du kan också använda `plot.lm()` tooproduce en serie av diagnostiska områden.

    ## Show hello diagnostic plots for hello model
    plot(milk.lm2, ask = FALSE)

Den här koden genererar en serie av diagnostiska områden som visas i bild 26.

![Första av diagnostiska områden för hello när modellen](./media/machine-learning-r-quickstart/unnamed-chunk-221.png)

![Andra av diagnostiska områden för hello när modellen](./media/machine-learning-r-quickstart/unnamed-chunk-222.png)

![Tredje av diagnostiska områden för hello när modellen](./media/machine-learning-r-quickstart/unnamed-chunk-223.png)

![Fjärde av diagnostiska områden för hello när modellen](./media/machine-learning-r-quickstart/unnamed-chunk-224.png)

*Bild 26. Diagnostik ritas för hello när modellen.*

Det finns några hög inflytelserik punkter som anges i dessa områden, men inget toocause viktig för oss. Dessutom kan vi se från hello Normal Q-Q ritytans att hello residualer är nära toonormally distribueras, ett viktigt antagande för linjära modeller.

### <a name="forecasting-and-model-evaluation"></a>Prognosmodellen och modellen utvärdering
Det finns en mer sak toodo toocomplete vårt exempel. Vi behöver toocompute prognoser och mäta hello fel mot hello faktiska data. Vår prognos blir för hello 12 månaders 2013. Vi kan beräkna ett fel mått för den här prognosen toohello faktiska data som inte är en del av vår datauppsättning för träning. Dessutom kan kan vi jämföra prestanda på hello 18 år utbildning data toohello 12 månader från testdata.  

Ett antal mått används toomeasure hello prestanda över tid serie modeller. I vårt fall ska vi använda hello strömeffektivvärde (RMS)-fel. hello beräknar följande funktion hello RMS fel mellan två serier.  

    RMS.error <- function(series1, series2, is.log = TRUE, min.length = 2){
      ## Function toocompute hello RMS error or difference between two
      ## series or vectors

      messages <- c("ERROR: Input arguments toofunction RMS.error of wrong type encountered",
                    "ERROR: Input vector toofunction RMS.error is too short",
                    "ERROR: Input vectors toofunction RMS.error must be of same length",
                    "WARNING: Funtion rms.error has received invald input time series.")

      ## Check hello arguments
      if(!is.numeric(series1) | !is.numeric(series2) | !is.logical(is.log) | !is.numeric(min.length)) {
        warning(messages[1])
        return(NA)}

      if(length(series1) < min.length) {
        warning(messages[2])
        return(NA)}

      if((length(series1) != length(series2))) {
           warning(messages[3])
        return(NA)}

      ## If is.log is TRUE exponentiate hello values, else just copy
      if(is.log) {
        tryCatch( {
          temp1 <- exp(series1)
          temp2 <- exp(series2) },
          error = function(e){warning(messages[4]); NA}
        )
      } else {
        temp1 <- series1
        temp2 <- series2
      }

     ## Compute predictions from our models
    predict1  <- predict(milk.lm, cadairydata)
    predict2  <- predict(milk.lm2, cadairydata)

    ## Compute hello RMS error in a dataframe
      tryCatch( {
        sqrt(sum((temp1 - temp2)^2) / length(temp1))},
        error = function(e){warning(messages[4]); NA})
    }

Precis som med hello `log.transform()` funktion som beskrevs i hello ”värdet transformationer” avsnittet, finns det en hel del kontroll och undantag recovery felkod i den här funktionen. hello principer anställda är hello samma. hello arbetet på två platser kapslas in i `tryCatch()`. Hello tidsserier är först exponentiated, eftersom vi har arbetat med hello loggar hello värden. Dessutom beräknas hello faktiska RMS-fel.  

Utrustad med en RMS-fel med funktionen toomeasure hello kan vi skapa och utdata en dataframe som innehåller hello RMS-fel. Vi innehåller villkoren för hello trend modellen enbart och hello fullständig modellen med säsongsbaserade faktorer. hello följande kod hello jobb med hjälp av hello två linjära modeller vi har skapats.

    ## Compute hello RMS error in a dataframe
    ## Include hello row names in hello first column so they will
    ## appear in hello output of hello Execute R Script
    RMS.df  <-  data.frame(
    rowNames = c("Trend Model", "Seasonal Model"),
      Traing = c(
      RMS.error(predict1[1:216], cadairydata$Milk.Prod[1:216]),
      RMS.error(predict2[1:216], cadairydata$Milk.Prod[1:216])),
      Forecast = c(
        RMS.error(predict1[217:228], cadairydata$Milk.Prod[217:228]),
        RMS.error(predict2[217:228], cadairydata$Milk.Prod[217:228]))
    )
    RMS.df

    ## hello following line should be executed only when running in
    ## Azure Machine Learning Studio
    maml.mapOutputPort('RMS.df')

Kör den här koden ger hello utdata som visas i bild 27 vid hello utdataporten för datauppsättningen resultat.

![Jämförelse av RMS-fel för hello modeller][26]

*Bild 27. Jämförelse av RMS-fel för hello modeller.*

Från de här resultaten returneras se att lägga till hello när faktorer toohello modellen minskar hello RMS fel avsevärt. För förstås, hello RMS-fel för hello utbildning data är lite mindre än hello prognos.

## <a id="appendixa"></a>BILAGA A: Guiden tooRStudio
RStudio är ganska väl dokumenterat, så i den här bilagan jag ger vissa länkar toohello delar av hello RStudio dokumentationen tooget du startade.

1. Skapa projekt
   
   Du kan organisera och hantera din R-koden i projekt med hjälp av RStudio. hello-dokumentation som använder projekt finns på https://support.rstudio.com/hc/articles/200526207-Using-Projects.
   
   Jag rekommenderar att du följer de här anvisningarna och skapar ett projekt för hello R-kodexempel i det här dokumentet.  
2. Redigera och köra R-koden
   
   RStudio tillhandahåller en integrerad miljö för redigering och R kod körs. Dokumentation finns på https://support.rstudio.com/hc/articles/200484448-Editing-and-Executing-Code.
3. Felsökning
   
   RStudio innehåller kraftfulla felsökningsfunktioner. Dokumentationen för de här funktionerna finns på https://support.rstudio.com/hc/articles/200713843-Debugging-with-RStudio.
   
   hello brytpunkt felsökningsfunktioner dokumenteras i https://support.rstudio.com/hc/articles/200534337-Breakpoint-Troubleshooting.

## <a id="appendixb"></a>BILAGA B: Kan läsa
Den här självstudiekursen omfattar hello grunderna för programmering R av vad du behöver toouse hello R språk med Azure Machine Learning Studio. Om du inte är bekant med R är två introduktioner tillgängliga på CRAN:

* R för nybörjare av Emmanuel Paradis är en bra toostart på http://cran.r-project.org/doc/contrib/Paradis-rdebuts_en.pdf.  
* En introduktion tooR av W. N. Venables et. al. försätts i lite mer djup för http://cran.r-project.org/doc/manuals/R-intro.html.

Det finns många böcker på R som kan hjälpa dig att komma igång. Här är några jag användbara:

* hello bilder av R Programming: A visningen av statistiska programvara Design med Norman Matloff är en utmärkt introduktion tooprogramming i R.  
* R Cookbook av Paul Teetor ger en problemet och lösningen metod-toousing R.  
* R i praktiken av Robert Kabacoff är en annan användbar inledande bok. hello tillhörande snabb R webbplats är en användbar resurs på http://www.statmethods.net/.
* R Inferno av Patrick Burns är en förstås humoristiskt bok som hanterar ett antal komplicerade och svår avsnitt som kan uppstå vid programmering i R. hello bok är tillgängliga gratis http://www.burns-stat.com/documents/books/the-r-inferno/.
* Om du vill att en djupdykning i avancerade ämnen i R ta en titt på hello book Avancerat R av Hadley Wickham. hello onlineversionen av boken är tillgänglig kostnadsfritt på http://adv-r.had.co.nz/.

En förteckning över R tid serie paket finns i hello CRAN aktivitetsvyn för analys av tidsserier: http://cran.r-project.org/web/views/TimeSeries.html. Information om specifika tid serie paket vända toohello dokumentationen för det paketet.

hello boken inledande tidsserier med R av Paul Cowpertwait och Andrew Metcalfe innehåller en introduktion toousing R för analys av tidsserier. Många fler teoretisk texter innehåller R-exempel.

Vissa bra internet-resurser:

* DataCamp: DataCamp Lär R hello bekvämt i webbläsaren med video erfarenheter och kodning övningarna. Det finns interaktiva självstudier om hello senaste R-teknik och paket. Vidta hello ledigt interaktiva R-självstudierna på https://www.datacamp.com/courses/introduction-to-r
* En guide i komma igång med R från Programiz https://www.programiz.com/r-programming
* En snabb R självstudiekursen Kelly Black från Clarkson University http://www.cyclismo.org/tutorial/R/
* 60 + R över resurser i http://www.computerworld.com/article/2497464/business-intelligence-60-r-resources-to-improve-your-data-skills.html

<!--Image references-->
[1]: ./media/machine-learning-r-quickstart/fig1.png
[2]: ./media/machine-learning-r-quickstart/fig2.png
[3]: ./media/machine-learning-r-quickstart/fig3.png
[4]: ./media/machine-learning-r-quickstart/fig4.png
[5]: ./media/machine-learning-r-quickstart/fig5.png
[6]: ./media/machine-learning-r-quickstart/fig6.png
[7]: ./media/machine-learning-r-quickstart/fig7.png
[8]: ./media/machine-learning-r-quickstart/fig8.png
[9]: ./media/machine-learning-r-quickstart/fig9.png
[10]: ./media/machine-learning-r-quickstart/fig10.png
[11]: ./media/machine-learning-r-quickstart/fig11.png
[12]: ./media/machine-learning-r-quickstart/fig12.png
[13]: ./media/machine-learning-r-quickstart/fig13.png
[14]: ./media/machine-learning-r-quickstart/fig14.png
[15]: ./media/machine-learning-r-quickstart/fig15.png
[16]: ./media/machine-learning-r-quickstart/fig16.png
[17]: ./media/machine-learning-r-quickstart/fig17.png
[18]: ./media/machine-learning-r-quickstart/fig18.png
[19]: ./media/machine-learning-r-quickstart/fig19.png
[20]: ./media/machine-learning-r-quickstart/fig20.png
[21]: ./media/machine-learning-r-quickstart/fig21.png
[22]: ./media/machine-learning-r-quickstart/fig22.png

[26]: ./media/machine-learning-r-quickstart/fig26.png

<!--links-->
[appendixa]: #appendixa
[download]: https://azurebigdatatutorials.blob.core.windows.net/rquickstart/RFiles.zip


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
