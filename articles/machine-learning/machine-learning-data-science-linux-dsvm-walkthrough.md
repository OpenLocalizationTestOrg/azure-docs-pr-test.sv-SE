---
title: "aaaData vetenskap på hello Linux datavetenskap virtuella | Microsoft Docs"
description: "Hur tooperform flera vanliga datavetenskap åtgärder med hello Linux datavetenskap VM."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 34ef0b10-9270-474f-8800-eecb183bbce4
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: bradsev;paulsh
ms.openlocfilehash: 78764825f2e834fa4ddb7fdc2f59418dbe736e1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="data-science-on-hello-linux-data-science-virtual-machine"></a>Datavetenskap på hello Linux datavetenskap virtuell dator
Den här genomgången visar hur tooperform flera vanliga datavetenskap aktiviteter med hello Linux datavetenskap VM. hello Linux Data vetenskap virtuell dator (DSVM) är en avbildning av virtuell dator som är tillgängliga på Azure som är förinstallerade med en samling verktyg som används ofta för dataanalys och maskininlärning. hello viktiga programvarukomponenter specificerade i hello [etablera hello Linux datavetenskap virtuella](machine-learning-data-science-linux-dsvm-intro.md) avsnittet. hello VM-avbildning gör det enkelt tooget igång gör datavetenskap i minuter, utan att behöva tooinstall och konfigurera var och en av hello verktyg separat. Du kan enkelt skala upp hello VM, om det behövs och stoppa den inte under användning. Den här resursen är därför både elastisk och kostnadseffektiv.

hello datavetenskap aktiviteter visas i den här genomgången följer hello steg som beskrivs i hello [Team datavetenskap Process](https://azure.microsoft.com/documentation/learning-paths/data-science-process/). Denna process tillhandahåller en systematiskt toodata vetenskap som gör att grupper data forskare tooeffectively samarbeta över hello livscykeln för utveckling av intelligent program. hello av vetenskapliga data innehåller också en iterativ ramverk för datavetenskap som kan följas av en enskild.

Vi analysera hello [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) datauppsättning i den här genomgången. Det här är en uppsättning e-postmeddelanden som är markerade som skräppost eller skinka (vilket innebär att de inte är skräppost), och innehåller även vissa statistik på hello innehåll hello e-postmeddelanden. hello statistik ingår diskuteras hello bredvid men ett avsnitt.

## <a name="prerequisites"></a>Krav
Innan du kan använda en Linux datavetenskap virtuell dator, måste du ha hello följande:

* En **Azure-prenumeration**. Om du inte redan har en, se [skapa din kostnadsfria Azure-konto idag](https://azure.microsoft.com/free/).
* En [ **Linux datavetenskap VM**](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm). Information om att etablera den här virtuella datorn finns i [etablera hello Linux datavetenskap virtuella](machine-learning-data-science-linux-dsvm-intro.md).
* [X2Go](http://wiki.x2go.org/doku.php) installerat på datorn och öppna en XFCE-session. Information om hur du installerar och konfigurerar en **X2Go klienten**, se [installera och konfigurera X2Go klienten](machine-learning-data-science-linux-dsvm-intro.md#installing-and-configuring-x2go-client). 
* En **AzureML konto**. Om du inte redan har en registrering för nya på hello [AzureML webbsida](https://studio.azureml.net/). Det finns en fri användning nivå toohelp dig att komma igång.

## <a name="download-hello-spambase-dataset"></a>Hämta hello spambase dataset
Hej [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) dataset är en relativt liten uppsättning data som innehåller endast 4601 exempel. Detta är en lämplig storlek toouse när visar att vissa av hello viktiga funktioner i hello datavetenskap VM eftersom den håller hello resurskraven liten.

> [!NOTE]
> Den här genomgången skapades på en D2 v2-format Linux datavetenskap virtuell dator. Den här storleken DSVM är kan hantera hello procedurerna i den här genomgången.
>
>

Om du behöver mer lagringsutrymme kan du skapa ytterligare diskar och koppla dem tooyour VM. Diskarna använder beständiga Azure storage, så att deras data bevaras även om hello server etableras på grund av tooresizing eller är avstängd. tooadd en disk och bifoga den tooyour VM, följer du anvisningarna hello i [lägga till en disk tooa Linux VM](../virtual-machines/linux/add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). De här stegen använder hello Azure-kommandoradsgränssnittet (Azure CLI) som redan är installerad på hello DSVM. Så kan du göra de här procedurerna helt från hello Virtuella datorn. Ett annat alternativ tooincrease lagring är toouse [Azure files](../storage/files/storage-how-to-use-files-linux.md).

toodownload hello data, öppna ett terminalfönster och kör det här kommandot:

    wget http://archive.ics.uci.edu/ml/machine-learning-databases/spambase/spambase.data

hello nedladdade filen matchar inte innehåller en rubrikrad, så vi skapa en annan fil som har ett sidhuvud. Kör det här kommandot toocreate en fil med lämpliga hello-huvuden:

    echo 'word_freq_make, word_freq_address, word_freq_all, word_freq_3d,word_freq_our, word_freq_over, word_freq_remove, word_freq_internet,word_freq_order, word_freq_mail, word_freq_receive, word_freq_will,word_freq_people, word_freq_report, word_freq_addresses, word_freq_free,word_freq_business, word_freq_email, word_freq_you, word_freq_credit,word_freq_your, word_freq_font, word_freq_000, word_freq_money,word_freq_hp, word_freq_hpl, word_freq_george, word_freq_650, word_freq_lab,word_freq_labs, word_freq_telnet, word_freq_857, word_freq_data,word_freq_415, word_freq_85, word_freq_technology, word_freq_1999,word_freq_parts, word_freq_pm, word_freq_direct, word_freq_cs, word_freq_meeting,word_freq_original, word_freq_project, word_freq_re, word_freq_edu,word_freq_table, word_freq_conference, char_freq_semicolon, char_freq_leftParen,char_freq_leftBracket, char_freq_exclamation, char_freq_dollar, char_freq_pound, capital_run_length_average,capital_run_length_longest, capital_run_length_total, spam' > headers

Sedan Sammanfoga hello två filer tillsammans med hello-kommando:

    cat spambase.data >> headers
    mv headers spambaseHeaders.data

hello datamängden har flera typer av statistik på varje e-postmeddelande:

* Kolumner som ***word\_frekvens\_WORD*** ange hello procentandelen ord i hello e-post som matchar *WORD*. Till exempel om *word\_frekvens\_Se* är 1, 1% av alla ord i hello e-post har *Se*.
* Kolumner som ***char\_frekvens\_CHAR*** ange hello procent av alla tecken i hello e-postmeddelande som har *CHAR*.
* ***kapital\_kör\_längd\_längsta*** är hello längsta längden för en sekvens med stora bokstäver.
* ***kapital\_kör\_längd\_genomsnittlig*** är hello genomsnittliga längden på alla sekvenser av stora bokstäver.
* ***kapital\_kör\_längd\_totala*** är hello totala längden för alla sekvenser av stora bokstäver.
* ***skräppost*** anger om e-post hello ansågs skräppost eller inte (1 = skräppost, 0 = inte skräppost).

## <a name="explore-hello-dataset-with-microsoft-r-open"></a>Utforska hello datamängd med Microsoft R öppna
Vi undersöka hello data och utföra vissa grundläggande maskininlärning med R. hello datavetenskap VM medföljer [Microsoft R öppna](https://mran.revolutionanalytics.com/open/) förinstallerat. hello ger flertrådade matematiska bibliotek i den här versionen av R bättre prestanda än olika Enkeltrådig versioner. Microsoft R öppna ger också reproducerbara med hjälp av en ögonblicksbild av hello CRAN paketdatabasen.

tooget kopior av hello kodexempel som används i den här genomgången klona hello **Azure-Machine-Learning--datavetenskap** databas med hjälp av git, som har förinstallerats hello VM. Kör från hello git för kommandoraden:

    git clone https://github.com/Azure/Azure-MachineLearning-DataScience.git

Öppna ett terminalfönster och starta en ny R-session med interaktiva hello R-konsolen.

> [!NOTE]
> Du kan också använda RStudio för hello följande procedurer. tooinstall RStudio, kör kommandot på en terminal:`./Desktop/DSVM\ tools/installRStudio.sh`
>
>

tooimport hello data och skapa hello miljö, kör:

    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

toosee sammanfattande statistik om varje kolumn:

    summary(data)

För en annan vy av hello data:

    str(data)

Detta visar du hello typ för varje variabel och hello först få värden i hello dataset.

Hej *skräppost* lästes kolumnen som ett heltal, men det är faktiskt en kategoriska variabeln (eller faktor). tooset dess typ:

    data$spam <- as.factor(data$spam)

toodo vissa undersökande analyser, Använd hello [ggplot2](http://ggplot2.org/) paketet, ett populärt graphing bibliotek för R som redan är installerad på hello VM. Meddelande från hello sammanfattningsdata visas tidigare, vi har sammanfattande statistik på hello ofta hello utropstecken tecken. Vi ska rita dessa frekvenser här med hello följande kommandon:

    library(ggplot2)
    ggplot(data) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Eftersom hello noll-fältet skeva hello ritytans, ska vi ta bort det:

    email_with_exclamation = data[data$char_freq_exclamation > 0, ]
    ggplot(email_with_exclamation) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Det finns en icke-trivial densitet ovan 1 som ser ut intressant. Nu ska vi titta på bara data:

    ggplot(data[data$char_freq_exclamation > 1, ]) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Dela sedan upp den av skräppost vs skinka:

    ggplot(data[data$char_freq_exclamation > 1, ], aes(x=char_freq_exclamation)) +
    geom_density(lty=3) +
    geom_density(aes(fill=spam, colour=spam), alpha=0.55) +
    xlab("spam") +
    ggtitle("Distribution of spam \nby frequency of !") +
    labs(fill="spam", y="Density")

Exemplen ska aktivera du toomake liknande områden i hello andra kolumner tooexplore hello data i dem.

## <a name="train-and-test-an-ml-model"></a>Träna och testa en ML-modell
Nu vi train maskininlärning några modeller tooclassify hello e-post i hello datamängden innehåller antingen span- eller skinka. Vi träna en beslut trädet modell och en slumpmässigt Skogsmodell i det här avsnittet och testa sina korrektheten i sina förutsägelser.

> [!NOTE]
> Hej rpart (rekursiv partitionering och Regressionsträd) paketet används i hello följande kod är redan installerad på hello datavetenskap VM.
>
>

Först ska vi dela hello dataset i uppsättningar för träning och testning:

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

Och sedan skapa ett beslut trädet tooclassify hello e-postmeddelanden.

    require(rpart)
    model.rpart <- rpart(spam ~ ., method = "class", data = trainSet)
    plot(model.rpart)
    text(model.rpart)

Här är hello resultat:

![1](./media/machine-learning-data-science-linux-dsvm-walkthrough/decision-tree.png)

hur väl utförs på hello utbildning toodetermine ange använder hello följande kod:

    trainSetPred <- predict(model.rpart, newdata = trainSet, type = "class")
    t <- table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

toodetermine hur väl den utför hello test mängd:

    testSetPred <- predict(model.rpart, newdata = testSet, type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

Prova med en slumpmässig Skogsmodell också. Slumpmässiga skogar träna en mängd olika beslutsträd och utgående en klass som är hello klassificeringar från alla hello enskilda beslutsträd hello läge. De ger mer kraftfulla machine learning-metoden som de rätta för hello tendens av ett beslut trädet modellen toooverfit en utbildning dataset.

    require(randomForest)
    trainVars <- setdiff(colnames(data), 'spam')
    model.rf <- randomForest(x=trainSet[, trainVars], y=trainSet$spam)

    trainSetPred <- predict(model.rf, newdata = trainSet[, trainVars], type = "class")
    table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)

    testSetPred <- predict(model.rf, newdata = testSet[, trainVars], type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy


## <a name="deploy-a-model-tooazure-ml"></a>Distribuera en modell tooAzure ML
[Azure Machine Learning Studio](https://studio.azureml.net/) (AzureML) är en molnbaserad tjänst som gör det enkelt toobuild och distribuera förutsägelseanalysmodeller. En av hello bra funktioner i AzureML är dess möjlighet toopublish alla R fungera som en webbtjänst. Hej AzureML-R-paket gör distributionen enkelt toodo direkt från våra R-session på hello DSVM.

toodeploy hello beslut trädet kod från hello föregående avsnitt, behöver du toosign i tooAzure Machine Learning Studio. Du behöver ditt arbetsyte-ID och ett token toosign auktorisering i. toofind dessa värden och initiera hello AzureML variabler med dem:

Välj **inställningar** på hello vänstra menyn. Obs din **ARBETSYTE-ID**. ![2](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-id.png)

Välj **auktorisering token** från Administration hello-menyn och Observera din **primära auktorisering Token**.![ 3](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-token.png)

Läs in hello **AzureML** paketet och ange sedan värdena för variabler som hello med arbetsytan och token-ID i R-session på hello DSVM:

    require(AzureML)
    wsAuth = "<authorization-token>"
    wsID = "<workspace-id>"


Vi kan förenkla hello modellen toomake den här demonstrationen enklare tooimplement. Välj hello tre variabler i hello beslut närmaste toohello trädrot och skapa ett nytt träd med bara de här tre variablerna:

    colNames <- c("char_freq_dollar", "word_freq_remove", "word_freq_hp", "spam")
    smallTrainSet <- trainSet[, colNames]
    smallTestSet <- testSet[, colNames]
    model.rpart <- rpart(spam ~ ., method = "class", data = smallTrainSet)

Vi behöver en förutsägelsefunktionen som tar hello funktioner som indata och returnerar hello förutsagda värden:

    predictSpam <- function(char_freq_dollar, word_freq_remove, word_freq_hp) {
        predictDF <- predict(model.rpart, data.frame("char_freq_dollar" = char_freq_dollar,
        "word_freq_remove" = word_freq_remove, "word_freq_hp" = word_freq_hp))
        return(colnames(predictDF)[apply(predictDF, 1, which.max)])
    }

Publicera med hjälp av hello hello predictSpam funktionen tooAzureML **publishWebService** funktionen:

    spamWebService <- publishWebService("predictSpam",
        "spamWebService",
        list("char_freq_dollar"="float", "word_freq_remove"="float","word_freq_hp"="float"),
        list("spam"="int"),
        wsID, wsAuth)

Den här funktionen använder hello **predictSpam** fungera, skapar en webbtjänst med namnet **spamWebService** med definierade in- och utdataenheter och returnerar information om hello ny slutpunkt.

Visa information om hello publicerade webbtjänsten, inklusive dess API-slutpunkt och åtkomst nycklar med hello-kommando:

    spamWebService[[2]]

tootry den ut på hello första 10 raderna av hello test:

    consumeDataframe(spamWebService$endpoints[[1]]$PrimaryKey, spamWebService$endpoints[[1]]$ApiLocation, smallTestSet[1:10, 1:3])


## <a name="use-other-tools-available"></a>Använda andra verktyg som är tillgängliga
hello återstående avsnitten visar hur toouse vissa hello verktyg installerade på hello Linux datavetenskap VM. Här är hello lista över verktyg som beskrivs:

* XGBoost
* Python
* Jupyterhub
* Spännen
* PostgreSQL & Squirrel SQL
* SQL Server Data Warehouse

## <a name="xgboost"></a>XGBoost
[XGBoost](https://xgboost.readthedocs.org/en/latest/) är ett verktyg som ger en snabb och exakt ökat trädet implementering.

    require(xgboost)
    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

    bst <- xgboost(data = data.matrix(trainSet[,0:57]), label = trainSet$spam, nthread = 2, nrounds = 2, objective = "binary:logistic")

    pred <- predict(bst, data.matrix(testSet[, 0:57]))
    accuracy <- 1.0 - mean(as.numeric(pred > 0.5) != testSet$spam)
    print(paste("test accuracy = ", accuracy))

XGBoost kan också anropa från python eller en kommandorad.

## <a name="python"></a>Python
För utveckling med hjälp av Python, har hello Anaconda Python distributioner 2.7 och 3.5 installerats i hello DSVM.

> [!NOTE]
> Hej Anaconda distribution innehåller [Condas](http://conda.pydata.org/docs/index.html), vilket kan vara används toocreate anpassade miljöer för Python som har olika versioner och/eller paket som är installerade.
>
>

Vi läses in några hello spambase dataset och klassificera hello e-postmeddelanden med support vector datorer i scikit-Läs mer:

    import pandas
    from sklearn import svm    
    data = pandas.read_csv("spambaseHeaders.data", sep = ',\s*')
    X = data.ix[:, 0:57]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

förutsägelser för toomake:

    clf.predict(X.ix[0:20, :])

tooshow hur toopublish en AzureML-slutpunkt, vi gör en enklare modell hello tre variabler som vi gjorde när det tidigare publicerade hello R-modellen.

    X = data.ix[["char_freq_dollar", "word_freq_remove", "word_freq_hp"]]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

toopublish hello modellen tooAzureML:

    # Publish hello model.
    workspace_id = "<workspace-id>"
    workspace_token = "<workspace-token>"
    from azureml import services
    @services.publish(workspace_id, workspace_token)
    @services.types(char_freq_dollar = float, word_freq_remove = float, word_freq_hp = float)
    @services.returns(int) # 0 or 1
    def predictSpam(char_freq_dollar, word_freq_remove, word_freq_hp):
        inputArray = [char_freq_dollar, word_freq_remove, word_freq_hp]
        return clf.predict(inputArray)

    # Get some info about hello resulting model.
    predictSpam.service.url
    predictSpam.service.api_key

    # Call hello model
    predictSpam.service(1, 1, 1)

> [!NOTE]
> Detta är endast tillgängligt för python 2.7 och ännu stöds inte på 3.5. Kör med **/anaconda/bin/python2.7**.
>
>

## <a name="jupyterhub"></a>Jupyterhub
Hej Anaconda distribution i hello DSVM levereras med en Jupyter-anteckningsbok, plattformsoberoende miljö tooshare Python, R eller Julia koden och analys. Hej Jupyter-anteckningsbok sker via JupyterHub. Du loggar in med ditt lokala Linux-användarnamn och lösenord vid ***https://\<VM DNS-namn eller IP-adress\>: 8000 /***. Alla konfigurationsfiler för JupyterHub finns i katalogen **/etc/jupyterhub**.

Flera exempel bärbara datorer är redan installerad på hello VM:

* Se hello [IntroToJupyterPython.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroToJupyterPython.ipynb) för en bärbar dator Python exempel.
* Se [IntroTutorialinR](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroTutorialinR.ipynb) ett exempel **R** bärbar dator.
* Se hello [IrisClassifierPyMLWebService](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IrisClassifierPyMLWebService.ipynb) för en annan exemplet **Python** bärbar dator.

> [!NOTE]
> hello Julia språk är också tillgänglig från hello kommandoraden på hello Linux datavetenskap VM.
>
>

## <a name="rattle"></a>Spännen
[Rattle](https://cran.r-project.org/web/packages/rattle/index.html) (hello R analytiska verktyget tooLearn enkelt) är ett grafiskt verktyg för R för datautvinning. Den har ett intuitivt gränssnitt som gör det enkelt tooload, utforska, transformera data och skapa och utvärdera modellerna.  hello artikel [Rattle: A Data Mining GUI för R](https://journal.r-project.org/archive/2009-2/RJournal_2009-2_Williams.pdf) ger en genomgång som visar dess funktioner.

Installera och starta spännen med hello följande kommandon:

    if(!require("rattle")) install.packages("rattle")
    require(rattle)
    rattle()

> [!NOTE]
> Installation krävs inte på hello DSVM. Men spännen kan medföra att du tooinstall ytterligare paket när det läses in.
>
>

Spännen använder ett fliken-baserat gränssnitt. De flesta av hello flikar motsvarar toosteps i hello [datavetenskap Process](https://azure.microsoft.com/documentation/learning-paths/data-science-process/), t.ex. läser in data eller Utforska den. hello av vetenskapliga data flödar från vänster tooright hello flikarna. Men hello senaste fliken innehåller en logg över hello R-kommandon som körs av spännen.

tooload och konfigurera hello datauppsättningen:

* tooload hello fil, Välj hello **Data** sedan fliken
* Välj hello selector bredvid för**Filename** och välj **spambaseHeaders.data**.
* tooload hello-fil. Välj **Execute** i hello översta raderna. Du bör se en sammanfattning av varje kolumn, inklusive identifierade datatyp, oavsett om det är ett indata, ett mål eller annan typ av variabeln och hello antalet unika värden.
* Spännen har identifierats hello **skräppost** kolumnen som hello mål. Välj hello skräppost kolumn, och sedan ange hello **Data måltypen** för**Categoric**.

tooexplore hello data:

* Välj hello **utforska** fliken.
* Klicka på **sammanfattning**, sedan **kör**, toosee viss information om hello datatyper och vissa sammanfattande statistik.
* tooview andra typer av statistik om varje variabel kan välja andra alternativ som **beskriver** eller **grunderna**.

Hej **utforska** fliken kan du också toogenerate många insiktsfulla ritas. tooplot histogrammet hello data:

* Välj **distributioner**.
* Kontrollera **Histogram** för **word_freq_remove** och **word_freq_you**.
* Välj **köra**. Du bör se både densitet ritas i ett enda graph-fönster, där det är tydligt hello ordet ”du” oftare visas i e-postmeddelanden än ”ta bort”.

hello är korrelation också intressant. toocreate en:

* Välj **korrelation** som hello **typen**, sedan
* Välj **köra**.
* Spännen varnar rekommenderar högst 40 variabler. Välj **Ja** tooview hello ritytans.

Det finns vissa intressanta korrelationer som tas upp: ”teknik” korreleras starkt för ”HP” och ”övningar”, till exempel. Det är också starkt korrelerade för ”650”, eftersom hello riktnummer hello dataset bidragsgivare 650.

hello numeriska värden för hello visar sambandet mellan ord är tillgängliga i hello utforska fönster. Det är intressant toonote, till exempel ”teknik” korreleras negativt med ”din/ditt” och ”pengar”.

Spännen kan omvandla hello dataset toohandle några vanliga problem. Exempelvis kan du toorescale funktioner, sedan imputera värden som saknas, hantera avvikare och ta bort variabler eller observationer med data som saknas. Spännen kan även identifiera kopplingen regler mellan observationer och/eller variabler. De här flikarna ligger utanför omfånget för den här introduktionsgenomgång.

Spännen kan också utföra klustret analys. Vi ska utesluta vissa funktioner toomake hello utdata enklare tooread. På hello **Data** , Välj **Ignorera** nästa tooeach hello variabler Förutom dessa tio objekt:

* word_freq_hp
* word_freq_technology
* word_freq_george
* word_freq_remove
* word_freq_your
* word_freq_dollar
* word_freq_money
* capital_run_length_longest
* word_freq_business
* skräppost

Gå sedan tillbaka toohello **klustret** , Välj **KMeans**, och ange hello *antalet kluster* too4. Sedan **köra**. hello resultat visas i utdatafönstret hello. Ett kluster har hög frekvens av ”george” och ”hp” och beror troligen på ett legitimt företag e-postmeddelande.

toobuild en enkel beslut trädet machine learning-modell:

* Välj hello **modellen** fliken
* Välj **träd** som hello **typen**.
* Välj **kör** toodisplay hello trädet i textform hello utdata fönster.
* Välj hello **Rita** knappen tooview en grafisk version. Det ser ut ungefär toohello trädet vi fick tidigare med hjälp av *rpart*.

En av hello nice funktionerna i spännen är dess möjlighet toorun flera maskininlärning metoder och utvärdera snabbt dem. Här är hello proceduren:

* Välj **alla** för hello **typen**.
* Välj **köra**.
* När den är klar kan du klicka på varje enskild **typen**, till exempel **SVM**, och visa hello resultat.
* Du kan också jämföra hello prestanda för hello modeller på hello verifiering anges med hello **utvärdera** fliken. Till exempel hello **fel matrisen** markering visar hello förvirring matris, övergripande fel och genomsnittlig klass-fel för varje modell hello validering mängd.
* Du kan också rita ROC kurvor, utföra analyser av känslighet och göra andra typer av modellen.

När du är klar med att skapa modeller välja hello **loggen** fliken tooview hello R koden körs av spännen under din session. Du kan välja hello **exportera** knappen toosave den.

> [!NOTE]
> Det finns ett fel i den aktuella versionen av spännen. toomodify hello skript eller använda det toorepeat steg senare, måste du infoga tecknet # framför * exportera den här loggen... * i hello texten hello-loggen.
>
>

## <a name="postgresql--squirrel-sql"></a>PostgreSQL & Squirrel SQL
Hej DSVM medföljer PostgreSQL installerad. PostgreSQL är en avancerad relationsdatabas med öppen källkod. Det här avsnittet visas hur tooload våra skräppost dataset i PostgreSQL och frågar den.

Innan du kan läsa in hello data måste tooallow lösenordsautentisering från hello localhost. Vid en kommandotolk:

    sudo gedit /var/lib/pgsql/data/pg_hba.conf

Hello nedre delen av konfigurationsfilen hello är flera rader som innehåller information om hello tillåtna anslutningar:

    # "local" is for Unix domain socket connections only
    local   all             all                                     trust
    # IPv4 local connections:
    host    all             all             127.0.0.1/32            ident
    # IPv6 local connections:
    host    all             all             ::1/128                 ident

Ändra hello ”IPv4 lokala anslutningar” rad toouse md5 i stället för ident, så att vi kan logga in med ett användarnamn och lösenord:

    # IPv4 local connections:
    host    all             all             127.0.0.1/32            md5

Och starta om hello postgres tjänsten:

    sudo systemctl restart postgresql

toolaunch psql, en interaktiv terminal PostgreSQL som hello inbyggda postgres användare kör hello följande kommando från en kommandotolk:

    sudo -u postgres psql

Skapa ett nytt konto, använder hello samma användarnamn som hello Linux-konto som du för närvarande är inloggad som och ge det ett lösenord:

    CREATE USER <username> WITH CREATEDB;
    CREATE DATABASE <username>;
    ALTER USER <username> password '<password>';
    \quit

Logga sedan in toopsql som dina användare:

    psql

Och importera hello data till en ny databas:

    CREATE DATABASE spam;
    \c spam
    CREATE TABLE data (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer);
    \copy data FROM /home/<username>/spambase.data DELIMITER ',' CSV;
    \quit

Nu ska vi utforska hello data och köra några frågor med **Squirrel SQL**, ett grafiskt verktyg som gör att du kan interagera med databaser via en JDBC-drivrutinen.

tooget igång genom att starta Squirrel SQL hello program-menyn. tooset in hello-drivrutinen:

* Välj **Windows**, sedan **visa drivrutiner**.
* Högerklicka på **PostgreSQL** och välj **ändra drivrutinen**.
* Välj **Extra klassen sökvägen**, sedan **lägga till**.
* Ange ***/usr/share/java/jdbcdrivers/postgresql-9.4.1208.jre6.jar*** för hello **filnamn** och
* Välj **öppna**.
* Välj drivrutiner i listan och markera **org.postgresql.Driver** i **klassnamn**, och välj **OK**.

tooset hello anslutning toohello lokal server:

* Välj **Windows**, sedan **Visa alias.**
* Välj hello  **+**  knappen toomake ett nytt alias.
* Ge den namnet *skräppost databasen*, Välj **PostgreSQL** i hello **drivrutinen** listrutan.
* Ange hello-URL för*jdbc:postgresql://localhost/spam*.
* Ange din *användarnamn* och *lösenord*.
* Klicka på **OK**.
* tooopen hello **anslutning** fönstret dubbelklickar du på hello ***skräppost databasen*** alias.
* Välj **Anslut**.

toorun några frågor:

* Välj hello **SQL** fliken.
* Ange en enkel fråga som `SELECT * from data;` i hello frågan textrutan överst hello hello SQL-fliken.
* Tryck på **Ctrl-ange** toorun den. Som standard returnerar Squirrel SQL hello första 100 rader från frågan.

Det finns många fler frågor som du kan köra tooexplore dessa data. Hur fungerar till exempel hello frekvensen av hello word *Se* skiljer sig åt mellan skräppost och skinka?

    SELECT avg(word_freq_make), spam from data group by spam;

Eller vad är hello egenskaper för e-post ofta innehåller *3d*?

    SELECT * from data order by word_freq_3d desc;

De flesta e-postmeddelanden som har en hög förekomst av *3d* är uppenbarligen skräppost, så det kan vara en användbar funktion för att skapa en förutsägelsemodell tooclassify hello e-postmeddelanden.

Om du vill använda tooperform maskininlärning med data som lagras i en PostgreSQL-databas kan du använda [MADlib](http://madlib.incubator.apache.org/).

## <a name="sql-server-data-warehouse"></a>SQL Server Data Warehouse
Azure SQL Data Warehouse är en molnbaserad skalbar databas som kan bearbeta massiva mängder data, relationella såväl som icke-relationella. Mer information finns i [vad är Azure SQL Data Warehouse?](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md)

tooconnect toohello data warehouse och skapa hello tabell hello kör följande kommando från en kommandotolk:

    sqlcmd -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -I

Vid hello sqlcmd-kommandotolk:

    CREATE TABLE spam (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer) WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = ROUND_ROBIN);
    GO

Kopiera data med bcp:

    bcp spam in spambaseHeaders.data -q -c -t  ',' -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -F 1 -r "\r\n"

> [!NOTE]
> hello radbrytningar i hello nedladdade filen är Windows-format, men bcp förväntar UNIX-format, så vi behöver tootell bcp som med hello - r-flaggan.
>
>

Och fråga med sqlcmd:

    select top 10 spam, char_freq_dollar from spam;
    GO

Du kan också fråga med Squirrel SQL. Följa liknande steg för PostgreSQL, med hjälp av hello Microsoft MSSQL Server JDBC Driver som finns i ***/usr/share/java/jdbcdrivers/sqljdbc42.jar***.

## <a name="next-steps"></a>Nästa steg
En översikt över ämnen som vägleder dig genom hello aktiviteter som utgör hello datavetenskap processen i Azure finns [Team datavetenskap Process](http://aka.ms/datascienceprocess).

En beskrivning av andra slutpunkt till slutpunkt-genomgångar som visar hello stegen i hello Team datavetenskap Process för specifika scenarier finns [Team datavetenskap Process genomgång](data-science-process-walkthroughs.md). hello genomgång också illustrerar hur toocombine molnet och lokala verktyg och tjänster i ett arbetsflöde eller pipeline toocreate ett intelligent program.
