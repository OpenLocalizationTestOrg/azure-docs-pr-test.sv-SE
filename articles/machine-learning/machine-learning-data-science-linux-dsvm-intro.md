---
title: aaaProvision hello Linux datavetenskap virtuella | Microsoft Docs
description: "Konfigurera och skapa en Linux datavetenskap virtuell dator på Azure toodo analytics och maskininlärning."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 3bab0ab9-3ea5-41a6-a62a-8c44fdbae43b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: bradsev
ms.openlocfilehash: 81dfa90f6cd4b4f33535a20fb97442bf9152d829
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="provision-hello-linux-data-science-virtual-machine"></a>Etablera hello Linux datavetenskap virtuell dator
hello Linux datavetenskap virtuella datorn är en CentOS-baserad Azure virtuell dator som ingår i en samling förinstallerade verktyg. Verktygen används ofta för att göra dataanalys och maskininlärning. viktiga hello programvarukomponenter som ingår är:

* Operativsystem: CentOS Linux-distribution.
* Microsoft R Server Developer Edition
* Anaconda Python-distribution (version 2.7 och 3.5), inklusive populära data analysbibliotek
* JuliaPro - en granskad fördelning av Julia språk med populära vetenskapliga och data analytics-bibliotek
* Standalone Spark-instans och nod Hadoop (HDFS, Yarn)
* JupyterHub - en fleranvändar Jupyter-anteckningsbok server som stöder R, Python, PySpark, Julia kärnor
* Azure Lagringsutforskaren
* Azure-kommandoradsgränssnittet (CLI) för att hantera Azure-resurser
* PostgresSQL databas
* Machine learning-verktyg
  * [Beräkningar nätverket Toolkit (CNTK)](https://github.com/Microsoft/CNTK): en djup learning toolkit för programvara från Microsoft Research.
  * [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): snabb machine learning-system som har stöd för tekniker som online, hash, allreduce, minskning, learning2search, aktiv, och interaktiva learning.
  * [XGBoost](https://xgboost.readthedocs.org/en/latest/): ett verktyg som ger snabb och exakt ökat trädet implementering.
  * [Rattle](http://rattle.togaware.com/) (hello R analytiska verktyget tooLearn enkelt): ett verktyg som gör att komma igång med dataanalys och machine learning i R enkelt med GUI-baserade datagranskning och modellering med automatisk kodgenerering för R.
* Azure SDK i Java, Python, node.js, Ruby, PHP
* Bibliotek i R och Python för använder i Azure Machine Learning och andra Azure-tjänster
* Utvecklingsverktyg och redigerare (RStudio, PyCharm, IntelliJ, Emacs, gedit, vi)


Gör datavetenskap omfattar iteration av en serie uppgifter:

1. Hitta, läsa in och förbearbetning av data
2. Skapa och testa modeller
3. Distribuera hello modeller för användning i intelligent program

Datavetare använda olika verktyg toocomplete dessa uppgifter. Det kan vara ganska tidskrävande toofind hello lämpliga programversioner hello och sedan toodownload, kompilera och installera dessa versioner.

hello Linux datavetenskap virtuell dator kan avsevärt underlättar det här problemet. Använda den toojump starta projektet analytics. Den låter dig toowork på uppgifter på olika språk, inklusive R, Python, SQL, Java och C++. Eclipse innehåller en IDE-toodevelop och testa din kod som är lätt toouse. hello Azure SDK ingår i hello VM kan du toobuild dina program med hjälp av olika tjänster på Linux för hello Microsoft cloud-plattformen. Dessutom har du åtkomst tooother språk som Ruby, Perl, PHP och node.js som också har förinstallerat.

Det finns inga avgifter för programvara för den här datavetenskap VM-avbildning. Du betalar endast hello användningen av Azure maskinvara avgifter som utvärderas baserat på hello storleken på hello virtuella datorn som du etablerar med hello VM-avbildning. Mer information om hello compute avgifter kan hittas på hello [VM lista sida på hello Azure Marketplace ](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/).

## <a name="other-versions-of-hello-data-science-virtual-machine"></a>Andra versioner av hello datavetenskap virtuell dator
En [Ubuntu](machine-learning-data-science-dsvm-ubuntu-intro.md) bilden är också tillgänglig, med många av hello samma verktyg som hello CentOS avbildningen och djup learning ramverk. En [Windows](machine-learning-data-science-provision-vm.md) bilden är också tillgänglig.

## <a name="prerequisites"></a>Krav
Innan du kan skapa en Linux datavetenskap virtuell dator, måste du ha hello följande:

* **En Azure-prenumeration**: tooobtain en, se [hämta kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/free/).
* **Ett Azure storage-konto**: toocreate en, se [skapa ett Azure storage-konto](../storage/common/storage-create-storage-account.md#create-a-storage-account). Du kan också kan hello storage-konto skapas som en del av hello processen att skapa hello VM, om du inte vill att toouse ett befintligt konto.

## <a name="create-your-linux-data-science-virtual-machine"></a>Skapa din Linux Data vetenskap virtuell dator
Här följer hello steg toocreate en instans av hello Linux datavetenskap virtuell dator:

1. Navigera toohello virtuella lista på hello [Azure-portalen](https://portal.azure.com/#create/microsoft-ads.linux-data-science-vmlinuxdsvm).
2. Klicka på **skapa** (längst ned hello) toobring hello guiden Installera.![ Konfigurera-data-vetenskap-vm](./media/machine-learning-data-science-linux-dsvm-intro/configure-linux-data-science-virtual-machine.png)
3. hello följande avsnitt ger hello indata för varje hello stegen i guiden för hello (räknas upp på hello höger i föregående bild hello) använda toocreate hello Microsoft datavetenskap Virtual Machine. Här följer hello indata behövs tooconfigure varje av de här stegen:
   
   a. **Grunderna**:
   
   * **Namnet**: namnet på din server för vetenskap av data som du skapar.
   * **Användarnamnet**: första kontot inloggning ID.
   * **Lösenordet**: första kontolösenord (du kan använda offentliga SSH-nyckeln i stället för lösenord).
   * **Prenumerationen**: Om du har mer än en prenumeration väljer hello en på vilka hello datorn är toobe skapas och debiteras. Du måste ha behörighet för resursen skapas för den här prenumerationen.
   * **Resursgruppen**: du kan skapa en ny eller Använd en befintlig grupp.
   * **Plats**: Välj hello datacenter som är mest lämplig. Det är vanligtvis hello datacenter som har de flesta av dina data, eller som är närmast tooyour fysiska platsen för snabbaste nätverksåtkomst.
   
   b. **Storlek**:
   
   * Välj en av hello servertyper som uppfyller dina krav på funktionsnivå och kostnaden begränsningar. Välj **visa alla** toosee VM-storlekar på fler sätt.
   
   c. **Inställningar för**:
   
   * **Disktyp**: Välj **Premium** om du föredrar Solid-State-hårddisk (SSD). Annars väljer du **Standard**.
   * **Lagringskontot**: du kan skapa ett nytt Azure storage-konto i din prenumeration, eller använda en befintlig i hello samma plats som har valts på hello **grunderna** steg hello guiden.
   * **Andra parametrar**: I de flesta fall kan du bara använda hello standardvärden. tooconsider icke-standardvärden, hovra över hello informationsmeddelande länk för hjälp om hello specifika fält.
   
   d. **Sammanfattning**:
   
   * Kontrollera att all information du angett är korrekt.
   
   e. **Köpa**:
   
   * toostart Hej etablering, klickar du på **köpa**. En länk som toohello användningsvillkor hello transaktion. hello VM har inte några ytterligare kostnader utöver hello beräkning för hello server storlek som du valde i hello **storlek** steg.

hello etablering bör ta ungefär 10 20 minuter. hello status för etablering av hello visas på hello Azure-portalen.

## <a name="how-tooaccess-hello-linux-data-science-virtual-machine"></a>Hur tooaccess hello Linux datavetenskap virtuell dator
Efter hello virtuell dator skapas, kan du logga in tooit med hjälp av SSH. Använd hello autentiseringsuppgifter som du skapade i hello **grunderna** avsnitt i steg 3 för hello text shell-gränssnittet. På Windows, kan du ladda ned en SSH-klientverktyg som [Putty](http://www.putty.org). Du kan använda X11 vidarebefordran på Putty eller installera hello X2Go klienten om du föredrar en grafisk skrivbord (X Windows System).

> [!NOTE]
> Hej X2Go klienten utföra betydligt bättre än X11 vidarebefordran i testning. Vi rekommenderar att du använder hello X2Go klienten för ett grafiskt gränssnitt för fjärrskrivbord.
> 
> 

## <a name="installing-and-configuring-x2go-client"></a>Installera och konfigurera X2Go klienten
hello Linux VM har redan etablerats med X2Go server och redo tooaccept klientanslutningar. tooconnect toohello Linux VM grafiska skrivbordet hello följa på klienten:

1. Hämta och installera hello X2Go klienten för din klientplattform från [X2Go](http://wiki.x2go.org/doku.php/doc:installation:x2goclient).    
2. Köra hello X2Go klienten och välja **ny Session**. Öppnar ett fönster för konfiguration med flera flikar. Ange hello följande konfigurationsparametrar:
   * **Sessionen fliken**:
     * **Värden**: hello värdnamn eller IP-adressen för din Linux datavetenskap VM.
     * **Inloggningen**: användarnamn på hello Linux VM.
     * **SSH-Port**: lämnar den 22 hello standardvärdet.
     * **Sessionstyp**: ändra hello värdet tooXFCE. Hello Linux VM stöder för närvarande endast XFCE skrivbordet.
   * **Fliken Media**: du kan inaktivera stöd för ljud och klienten utskrift om du inte behöver toouse dem.
   * **Delade mappar**: Om du vill kataloger från dina klientdatorer monteras på hello Linux VM, lägga till hello klienten datorn kataloger som du vill tooshare med hello VM på den här fliken.

När du loggar in toohello VM med hjälp av hello SSH-klienten eller XFCE grafiska via hello X2Go klienten är klar toostart använda hello verktyg som installerats och konfigurerats på hello VM. På XFCE, kan du se program menyn genvägar och ikoner på skrivbordet för många hello verktyg.

## <a name="tools-installed-on-hello-linux-data-science-virtual-machine"></a>Verktygen som installeras på hello Linux datavetenskap virtuell dator
### <a name="microsoft-r-server"></a>Microsoft R Server
R är en av hello vanligaste språken för dataanalys och maskininlärning. Om du vill toouse R för din analytics har hello VM Microsoft R Server (FRU) med hello Microsoft R öppna (MRO) och matematiska Kernel-biblioteket (MKL). Hej MKL optimerar matematiska operations vanligt i analytiska algoritmer. MRO är 100 procent kompatibel med R CRAN och någon av hello R-bibliotek som publicerats i CRAN kan installeras på hello MRO. FRU ger dig skalning och operationalization R modeller till webbtjänster. Du kan redigera din R-program i ett hello standard redigerare som RStudio, vi, Emacs eller gedit. Om du använder hello Emacs editor Observera hello Emacs paketet EDDELANDEN (Emacs talar statistik), har som förenklar arbetar med R filer hello Emacs Editor redan installerats.

toolaunch R konsolen du skriver **R** hello Shell. Då kommer du tooan interaktiv miljö. toodevelop R-program du använder vanligtvis en redigerare som Emacs eller vi eller gedit och sedan köra hello skript i R. Med RStudio har ett fullständigt grafiskt IDE-miljö toodevelop R-programmet.

Det finns också ett R-skript för tooinstall hello [upp 20 R-paket](http://www.kdnuggets.com/2015/06/top-20-r-packages.html) om du vill. Det här skriptet kan köras när du är i interaktiva hello R-gränssnittet, som kan anges (som tidigare nämnts) genom att skriva **R** hello Shell.  

### <a name="python"></a>Python
För utveckling med hjälp av Python, har distribution av Anaconda Python 2.7 och 3.5 installerats. Den här distributionen innehåller hello basera Python tillsammans med ungefär 300 hello populäraste matematiska, ingenjörer och data analytics paket. Du kan använda hello standard textredigerare. Du kan dessutom använda Spyder, en Python IDE som medföljer Anaconda Python-distributioner. Spyder måste ha ett grafiskt skrivbord eller X11 vidarebefordran. En genväg tooSpyder tillhandahålls i hello grafiska desktop.

Eftersom vi har både Python 2.7 och 3.5, behöver toospecifically aktivera hello önskad Python-version (conda environment) du vill använda toowork på i hello aktuell session. hello aktiveringen anger hello sökväg variabeln toohello önskade versionen av Python.

tooactivate hello Python 2.7 conda miljö, kör hello följande från hello shell:

    source /anaconda/bin/activate root

Python 2.7 är installerad på */anaconda/bin*.

tooactivate hello Python 3.5 conda miljö, kör hello följande från hello shell:

    source /anaconda/bin/activate py35


Python 3.5 är installerad på */anaconda/envs/py35/bin*.

tooinvoke en Python interaktiva sessionen, skriver du **python** hello Shell. Du kan ange om du är på ett grafiskt gränssnitt eller har X11 vidarebefordran set in **pycharm** toolaunch hello PyCharm Python IDE.

tooinstall ytterligare Python-bibliotek, behöver du toorun ```conda``` eller ````pip```` kommandot under sudo och ange sökvägen till hello Python package manager (conda eller pip) tooinstall toohello rätt Python-miljö. Exempel:

    sudo /anaconda/bin/pip install <package> #for Python 2.7 environment
    sudo /anaconda/envs/py35/bin/pip install <package> # for Python 3.5 environment


### <a name="jupyter-notebook"></a>Jupyter-anteckningsbok
Hej Anaconda distribution innehåller också en Jupyter-anteckningsbok, en miljö tooshare koden och analys. Hej Jupyter-anteckningsbok sker via JupyterHub. Du loggar in med ditt lokala Linux-användarnamn och lösenord.

hello Jupyter-anteckningsbok server har konfigurerats med Python 2, 3 Python och R kärnor före. Det finns en skrivbordsikon med namnet ”Jupyter-anteckningsbok” toolaunch hello webbläsare tooaccess hello anteckningsboken server. Om du är på hello VM via SSH eller X2Go klient kan du kan också besöka [https://localhost:8000 /](https://localhost:8000/) tooaccess hello Jupyter-anteckningsbok server.

> [!NOTE]
> Fortsätt om du får några certifikatvarningar.
> 
> 

Du kan komma åt hello Jupyter-anteckningsbok server från valfri värddator. Skriv *https://\<VM DNS-namn eller IP-adressen\>: 8000 /*

> [!NOTE]
> Port 8000 öppnas i hello-brandväggen som standard när hello VM etableras.
> 
> 

Vi har paketerat exempel anteckningsböcker--en i Python och en i R. Du kan se hello länken toohello exempel på startsidan för hello bärbar dator när du autentiserar toohello Jupyter-anteckningsbok med hjälp av din lokala Linux-användarnamn och lösenord. Du kan skapa en ny anteckningsbok genom att välja **ny**, och sedan hello språket kernel. Om du inte ser hello **ny** , klicka på hello **Jupyter** ikon på startsidan för hello övre vänstra toogo toohello hello anteckningsboken Server.

### <a name="apache-spark-standalone"></a>Apache Väck fristående 
En fristående instans av Apache Spark är förinstallerat på hello Linux DSVM toohelp först utveckla lokalt Spark-program innan du testar och distribuerar stora kluster. Du kan köra PySpark program via hello Jupyter kernel. När du öppnar Jupyter och klicka på knappen för hello ”nytt” bör du se en lista över tillgängliga kärnor. Hej ”Spark – Python” är hello PySpark-kerneln som låter dig skapa Spark program som använder Python språk. Du kan också använda en Python IDE som PyCharm eller Spyder toobuild du Väck program. Eftersom detta är en fristående instans, hello Spark stacken körs inom hello anropar klientprogram. Detta gör det snabbare och enklare tootroubleshoot problem jämfört med toodeveloping på ett Spark-kluster. 

En bärbar dator PySpark exempel finns på Jupyter som du hittar i hello ”SparkML” katalog under hello arbetskatalogen för Jupyter ($HOME/anteckningsböcker/SparkML/pySpark). 

Om programmering i R Spark ska använda du Microsoft R Server, SparkR eller sparklyr. 

Innan du kör i Spark-kontexten i Microsoft R Server, måste toodo en en tid installationen steg tooenable en lokal nod Hadoop HDFS och Yarn-instans. Som standard installeras men inaktiverat på hello DSVM Hadoop-tjänster. I ordning tooenable, behöver du toorun hello följande kommandon som rot hello första gången:

    echo -e 'y\n' | ssh-keygen -t rsa -P '' -f ~hadoop/.ssh/id_rsa
    cat ~hadoop/.ssh/id_rsa.pub >> ~hadoop/.ssh/authorized_keys
    chmod 0600 ~hadoop/.ssh/authorized_keys
    chown hadoop:hadoop ~hadoop/.ssh/id_rsa
    chown hadoop:hadoop ~hadoop/.ssh/id_rsa.pub
    chown hadoop:hadoop ~hadoop/.ssh/authorized_keys
    systemctl start hadoop-namenode hadoop-datanode hadoop-yarn

Du kan stoppa hello Hadoop-relaterade tjänster när du inte behöver dem genom att köra ````systemctl stop hadoop-namenode hadoop-datanode hadoop-yarn```` ett exempel som visar hur toodevelop och testa FRU i fjärranslutna Spark-kontexten (vilket är hello fristående Spark-instans på hello DSVM) har angetts och är tillgängliga i hello `/dsvm/samples/MRS` directory. 

### <a name="ides-and-editors"></a>IDEs och redigerare
Du kan välja mellan flera kod redigerare. Detta inkluderar vi/VIM Emacs, gEdit PyCharm, RStudio, Eclipse och IntelliJ. gEdit Eclipse, IntelliJ, RStudio och PyCharm är grafiska redigerare och måste du toobe inloggad tooa grafiska skrivbord toouse dem. Dessa redigerare har skrivbord och menyn genvägar toolaunch dem.

**VIM** och **Emacs** är textbaserade redigerare. Vi har installerat ett tillägg paket kallas Emacs talar statistik (EDDELANDEN) som underlättar arbetet med R hello Emacs Editor Emacs. Mer information finns på [EDDELANDEN](http://ess.r-project.org/).

**Eclipse** är en öppen källa extensible IDE som har stöd för flera språk. hello Java-utvecklare edition är hello-instans installerad på hello VM. Plugin-program är tillgängligt för flera populära språk som kan vara installerade tooextend hello miljö. Vi har också ett plugin-program som installerats i Eclipse kallas **Azure Toolkit för Eclipse**. Det gör att du toocreate, utveckla, testa och distribuera Azure med hjälp av hello Eclipse-utvecklingsmiljön som har stöd för språk som Java-program. Det finns också en **Azure SDK för Java** som tillåter åtkomst toodifferent Azure-tjänster från en Java-miljö. Mer information om Azure toolkit för Eclipse kan hittas på [Azure Toolkit för Eclipse](../azure-toolkit-for-eclipse.md).

**LaTex** installeras via hello texlive paket tillsammans med tillägget Emacs [auctex](https://www.gnu.org/software/auctex/manual/auctex/auctex.html) paket, vilket förenklar redigering LaTex dokument inom Emacs.  

### <a name="databases"></a>Databaser
#### <a name="postgres"></a>Postgres
hello öppna källdatabasen **Postgres** är tillgänglig på hello virtuell dator med hello-tjänster som körs och initdb redan slutförts. Du måste fortfarande toocreate databaser och användare. Mer information finns i hello [Postgres dokumentationen](https://www.postgresql.org/docs/).  

#### <a name="graphical-sql-client"></a>Grafisk SQL-klient
**SQuirrel SQL**, en grafisk SQL-klient har angetts tooconnect toodifferent databaser (till exempel Microsoft SQL Server, Postgres och MySQL) och toorun SQL-frågor. Du kan du köra från en grafisk session (med hello X2Go klienten, till exempel). tooinvoke SQuirrel SQL, kan du antingen starta från hello-ikonen på hello skrivbordet eller kör följande kommando på hello shell hello.

    /usr/local/squirrel-sql-3.7/squirrel-sql.sh

Innan hello först använda ställa in drivrutiner och databasen alias. hello JDBC-drivrutiner finns på:

*/usr/Share/Java/jdbcdrivers*

Mer information finns i [SQuirrel SQL](http://squirrel-sql.sourceforge.net/index.php?page=screenshots).

#### <a name="command-line-tools-for-accessing-microsoft-sql-server"></a>Kommandoradsverktyg för att komma åt Microsoft SQL Server
hello ODBC drivrutinspaket för SQL Server levereras med två verktyg för kommandoraden:

**BCP**: hello bcp-verktyget bulk kopierar data mellan en instans av Microsoft SQL Server och en fil i ett format som angetts av användaren. hello bcp-verktyget kan vara används tooimport stort antal nya rader i SQL Server-tabeller eller tooexport data från tabeller till datafiler. tooimport data i en tabell, du måste använda en fil för den tabellen, eller förstå hello struktur hello tabell och hello typer av data som är giltiga för dess kolumner.

Mer information finns i [ansluter med bcp](https://msdn.microsoft.com/library/hh568446.aspx).

**SQLCMD**: du kan ange Transact-SQL-uttryck med hello sqlcmd-verktyget, samt system procedurer och skriptfiler hello Kommandotolken. Det här verktyget används ODBC tooexecute Transact-SQL-batchar.

Mer information finns i [ansluter med sqlcmd](https://msdn.microsoft.com/library/hh568447.aspx).

> [!NOTE]
> Det finns vissa skillnader i det här verktyget mellan Linux och Windows-plattformar. Hello dokumentationen för information.
> 
> 

#### <a name="database-access-libraries"></a>Bibliotek för åtkomst av databasen
Det finns bibliotek i R och Python tooaccess databaser.

* I R hello **RODBC** paketet eller **dplyr** paketet kan du tooquery eller köra SQL-uttryck på hello databasserver.
* I Python, hello **pyodbc** biblioteket ger åtkomst till databasen med ODBC som hello underliggande lager.  

tooaccess **Postgres**:

* Från R: Använd hello paket **RPostgreSQL**.
* Från Python: Använd hello **psycopg2** bibliotek.

### <a name="azure-tools"></a>Azure-verktyg
hello följande Azure-verktyg är installerade på hello VM:

* **Azure-kommandoradsgränssnittet**: hello Azure CLI kan du toocreate och hantera Azure-resurser via shell-kommandon. tooinvoke hello Azure-verktyg, skriver du **azure hjälp**. Mer information finns i hello [dokumentationssidan för Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).
* **Microsoft Azure Lagringsutforskaren**: Microsoft Azure Lagringsutforskaren är ett grafiskt verktyg som används toobrowse via hello-objekt som du har lagrat i Azure storage-konto och tooand för tooupload och hämta data från Azure BLOB. Du kan öppna Lagringsutforskaren från hello genväg på skrivbordet ikon. Du kan anropa den från Kommandotolken i gränssnittet genom att skriva **StorageExplorer**. Du behöver toobe loggar in från en klient X2Go eller ha X11 vidarebefordran set upp.
* **Azure-bibliotek**: hello nedan visas några av hello förinstallerade bibliotek.
  
  * **Python**: hello Azure-relaterade bibliotek i Python som installeras är **azure**, **azureml**, **pydocumentdb**, och **pyodbc** . Hej tre första bibliotek med, du kan komma åt Azure storage-tjänster, Azure Machine Learning och Azure Cosmos DB (en NoSQL-databas på Azure). hello fjärde bibliotek, pyodbc (tillsammans med hello Microsoft ODBC-drivrutin för SQL Server), kan åtkomst tooSQL Server, Azure SQL Database och Azure SQL Data Warehouse från Python med hjälp av en ODBC-gränssnittet. Ange **pip listan** toosee alla hello visas bibliotek. Vara säker på att toorun 3.5 miljöer och det här kommandot i båda hello Python 2.7.
  * **R**: hello Azure-relaterade bibliotek i R som installeras är **AzureML** och **RODBC**.
  * **Java**: hello lista över Azure Java-bibliotek finns i hello directory **/dsvm/sdk/AzureSDKJava** på hello VM. hello viktiga bibliotek är Azure lagring och hantering av API: er och Azure Cosmos DB JDBC-drivrutiner för SQL Server.  

Du kan komma åt hello [Azure-portalen](https://portal.azure.com) från hello förinstallerade Firefox webbläsare. På hello Azure-portalen, kan du skapa, hantera och övervaka Azure-resurser.

### <a name="azure-machine-learning"></a>Azure Machine Learning
Azure Machine Learning är en helt hanterad molntjänst som du kan använda toobuild, distribuera och dela förutsägelseanalyslösningar. Du kan skapa dina experiment och modeller från Azure Machine Learning Studio. Den kan nås från en webbläsare på hello datavetenskap virtuella datorn genom att besöka [Microsoft Azure Machine Learning](https://studio.azureml.net).

När du loggar in tooAzure Machine Learning Studio har åtkomst tooan experiment arbetsyta där du kan skapa en logisk flöde för hello maskininlärningsalgoritmer. Du har åtkomst tooa Jupyter-anteckningsbok finns på Azure Machine Learning och fungerar sömlöst med hello experiment i Machine Learning Studio. Operationalisera hello maskininlärning modeller som du har skapat genom att omsluta dem i ett webbgränssnitt för tjänsten. Detta gör att klienter som skrivits i alla språk tooinvoke förutsägelser från maskininlärning för hello modeller. Mer information finns i hello [Machine Learning dokumentationen](https://azure.microsoft.com/documentation/services/machine-learning/).

Du kan också skapa modeller i R eller Python på hello VM och distribuerar den i produktion i Azure Machine Learning. Vi har installerat bibliotek i R (**AzureML**) och Python (**azureml**) tooenable den här funktionen.

Mer information om hur toodeploy modeller i R och Python i Azure Machine Learning finns [tio saker du kan göra på hello datavetenskap virtuella](machine-learning-data-science-vm-do-ten-things.md) (särskilt hello avsnittet ”bygga modeller med R eller Python och Operationalisera dem. med Azure Machine Learning ”).

> [!NOTE]
> Dessa instruktioner skrevs för hello Windows-versionen av hello datavetenskap VM. Men hello uppgifterna det om att distribuera modeller tooAzure Machine Learning är tillämpliga toohello Linux VM.
> 
> 

### <a name="machine-learning-tools"></a>Machine learning-verktyg
hello VM innehåller några maskininlärning verktyg och algoritmer som före kompileras och förinstallerat lokalt. Exempel på dessa är:

* **CNTK** (beräkningar nätverket Toolkit från Microsoft Research): en djup learning toolkit.
* **Vowpal Wabbit**: en snabb online inlärningsalgoritm.
* **xgboost**: ett verktyg som ger en optimerad, ökat trädet algoritmer.
* **Python**: Anaconda Python medföljer maskininlärningsalgoritmer med bibliotek som lär du dig Scikit. Du kan installera andra bibliotek med hello `pip install` kommando.
* **R**: ett omfattande Meddelandebibliotek machine learning-funktioner är tillgänglig för R. Vissa av hello bibliotek som är förinstallerat är lm, glm, randomForest, rpart. Du kan installera andra bibliotek genom att köra:
  
        install.packages(<lib name>)

Här är ytterligare information om hello först tre machine learning verktyg i hello-listan.

#### <a name="cntk"></a>CNTK
Det här är en öppen källkod djup learning toolkit. Det är ett kommandoradsverktyg (cntk) och hello sökväg används redan.

toorun ett grundläggande exempel köra följande kommandon i hello shell hello:

    cd /home/[USERNAME]/notebooks/CNTK/HelloWorld-LogisticRegression
    cntk configFile=lr_bs.cntk makeMode=false command=Train

Mer information finns i hello CNTK avsnitt i [GitHub](https://github.com/Microsoft/CNTK), och hello [CNTK wiki](https://github.com/Microsoft/CNTK/wiki).

#### <a name="vowpal-wabbit"></a>Vowpal Wabbit
Vowpal Wabbit är machine learning-system som använder tekniker, till exempel online, hash, allreduce, minskning, learning2search, aktiv, och interaktiva learning.

toorun hello verktyget på en grundläggande exempel hello följande:

    cp -r /dsvm/tools/VowpalWabbit/demo vwdemo
    cd vwdemo
    vw house_dataset

Det finns andra, större demonstrationer i katalogen. Mer information om VW finns [det här avsnittet i GitHub](https://github.com/JohnLangford/vowpal_wabbit), och hello [Vowpal Wabbit wiki](https://github.com/JohnLangford/vowpal_wabbit/wiki).

#### <a name="xgboost"></a>xgboost
Det här är ett bibliotek som har utformats och optimerats för ökat (trädet) algoritmer. hello syftet med det här biblioteket är toopush hello beräkning gränserna för datorer toohello yttersta behövs tooprovide storskaliga trädet förstärkning som är skalbart, bärbar och korrekt.

Det har angetts som en kommandorad som ett R-bibliotek.

toouse det här biblioteket i R, du kan starta en interaktiv R-session (genom att skriva **R** hello Shell), och Läs in hello bibliotek.

Här är ett enkelt exempel som du kan köra R-fråga:

    library(xgboost)

    data(agaricus.train, package='xgboost')
    data(agaricus.test, package='xgboost')
    train <- agaricus.train
    test <- agaricus.test
    bst <- xgboost(data = train$data, label = train$label, max.depth = 2,
                    eta = 1, nthread = 2, nround = 2, objective = "binary:logistic")
    pred <- predict(bst, test$data)

toorun hello xgboost kommandoraden följer hello kommandon tooexecute hello Shell:

    cp -r /dsvm/tools/xgboost/demo/binary_classification/ xgboostdemo
    cd xgboostdemo
    xgboost mushroom.conf


En .model-fil skrivs toohello katalogen som angetts. Information om det här exemplet demo finns [på GitHub](https://github.com/dmlc/xgboost/tree/master/demo/binary_classification).

Mer information om xgboost finns hello [xgboost dokumentationssidan](https://xgboost.readthedocs.org/en/latest/), och dess [GitHub-lagringsplatsen](https://github.com/dmlc/xgboost).

#### <a name="rattle"></a>Spännen
Rattle (hello **R** **A**nalytical **T**yg **T**o **L**tjäna **E** GUI-baserade datagranskning och modellering används asily). Den visar statistisk visual sammanfattningar av data, transformeringar data som kan modelleras lätt, skapar både oövervakade och övervakat modeller från hello data, anger hello grafiskt prestanda av modeller och anger poäng nya data. Den genererar också R-kod, replikerar hello åtgärder i hello användargränssnitt som kan köras direkt i R eller användas som utgångspunkt för ytterligare analys.

toorun spännen, du måste toobe i en grafisk inloggning skrivbordssession. Skriv på hello terminal ```R``` tooenter hello R-miljö. Ange hello följande kommandon i Kommandotolken hello R:

    library(rattle)
    rattle()

Nu ett grafiskt gränssnitt som öppnas med en uppsättning flikar. Här följer hello Snabbstart stegen i spännen behövs toouse exempeldata väder data och skapa en modell. I vissa hello stegen nedan är begärd tooautomatically installera och läsa in vissa nödvändiga R-paket som inte redan finns på hello system.

> [!NOTE]
> Om du inte har åtkomst tooinstall hello paketet i systemkatalogen för hello (hello standard), kan du se Kommandotolken på ditt R konsolen fönstret tooinstall paket tooyour personliga bibliotek. Svaret *y* om du ser dessa frågor.
> 
> 

1. Klicka på **Kör**.
2. En dialogruta visas, frågar om du vill toouse hello exempel väder datauppsättning. Klicka på **Ja** tooload hello exempel.
3. Klicka på hello **modellen** fliken.
4. Klicka på **kör** toobuild en beslutsträdet.
5. Klicka på **Rita** toodisplay hello beslutsträdet.
6. Klicka på hello **skog** alternativknappen och klickar på **kör** toobuild en slumpmässig skog.
7. Klicka på hello **utvärdera** fliken.
8. Klicka på hello **Risk** alternativknappen och klickar på **kör** toodisplay två Risk (kumulativ) prestanda områden.
9. Klicka på hello **loggen** fliken tooshow hello generera R-kod för hello föregående åtgärder.
   (På grund av tooa programfel i hello aktuella versionen av spännen, behöver du tooinsert en  *#*  tecknet framför *exportera den här loggen...*  i hello texten hello loggen.)
10. Klicka på hello **exportera** knappen toosave hello R-skriptfil med namnet *weather_script. R* toohello arbetsmapp.

Du kan avsluta spännen och R. Nu kan du ändra hello genereras R-skript eller använda den som den är toorun den när som helst toorepeat allt som gjordes i hello Rattle Användargränssnittet. Detta är ett enkelt sätt tooquickly göra analyser och maskininlärning i ett enkelt grafiskt gränssnitt vid automatisk generering av kod i R toomodify och/eller Läs särskilt för nybörjare i R.

## <a name="next-steps"></a>Nästa steg
Här visas hur du kan fortsätta att din inlärning och undersökning:

* Hej [datavetenskap på hello Linux datavetenskap virtuella](machine-learning-data-science-linux-dsvm-walkthrough.md) genomgången visar hur tooperform flera vanliga datavetenskap aktiviteter med hello Linux datavetenskap VM etablerats här. 
* Utforska hello olika datavetenskap verktyg på hello datavetenskap VM av provat hello verktyg som beskrivs i den här artikeln. Du kan också köra *dsvm mer information* på hello shell inom hello virtuell dator för en grundläggande introduktion och pekare toomore information om hello verktyg som installerats på hello VM.  
* Lär dig hur toobuild slutpunkt till slutpunkt Analyslösningar systematiskt med hello [Team datavetenskap Process](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).
* Besök hello [Cortana Analytics Gallery](http://gallery.cortanaanalytics.com) för machine learning och analytics exempel för att använda hello Cortana Analytics Suite.

