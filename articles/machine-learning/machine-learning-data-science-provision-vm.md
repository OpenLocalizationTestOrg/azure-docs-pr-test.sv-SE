---
title: aaaProvision hello Microsoft datavetenskap Virtual Machine | Microsoft Docs
description: "Konfigurera och skapa en datavetenskap virtuell dator i Azure för analys och maskininlärning."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: e1467c0f-497b-48f7-96a0-7f806a7bec0b
ms.service: machine-learning
ms.workload: data-services
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: bradsev
ms.openlocfilehash: 907a3bdc7e480d05e8e245f5e50d632900fcf471
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="provision-hello-microsoft-data-science-virtual-machine"></a>Etablera hello Microsoft datavetenskap virtuell dator
hello Microsoft datavetenskap Virtual Machine är en avbildning på virtuell dator (VM) för Windows Azure före installeras och konfigureras med flera populära verktyg som används för dataanalys och maskininlärning. hello-verktygen är:

* Microsoft R Server Developer Edition
* Anaconda Python-distribution
* Jupyter-anteckningsbok (med R, Python kärnor)
* Visual Studio Community Edition
* Power BI desktop
* SQL Server 2016 Developer Edition
* Machine learning och dataanalys verktyg
  * [Beräkningar nätverket Toolkit (CNTK)](https://github.com/Microsoft/CNTK): en djup learning toolkit för programvara från Microsoft Research.
  * [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): snabb machine learning-system som har stöd för tekniker som online, hash, allreduce, minskning, learning2search, aktiv, och interaktiva learning.
  * [XGBoost](https://xgboost.readthedocs.org/en/latest/): ett verktyg som ger snabb och exakt ökat trädet implementering.
  * [Rattle](http://rattle.togaware.com/) (hello R analytiska verktyget tooLearn enkelt): ett verktyg som gör att komma igång med dataanalys och machine learning i R enkelt med GUI-baserade datagranskning och modellering med automatisk kodgenerering för R.
  * [mxnet](https://github.com/dmlc/mxnet): ett ramverk för djup learning som utformats för både effektivitet och flexibilitet
  * [Weka](http://www.cs.waikato.ac.nz/ml/weka/) : ett visual datautvinning och maskininlärning programvara i Java.
  * [Apache ökad](https://drill.apache.org/): en schemafria SQL frågemotor för Hadoop, NoSQL och lagringsutrymmet i molnet.  Stöder ODBC och JDBC gränssnitt tooenable frågar NoSQL och filer från standard BI-verktyg som PowerBI, Excel, Tableau.
* Bibliotek i R och Python för använder i Azure Machine Learning och andra Azure-tjänster
* Git inklusive Git Bash toowork med källkodslager inklusive GitHub, Visual Studio Team Services
* Windows-portar för flera populära Linux kommandoradsverktyg (inklusive awk, sed, perl, grep, hitta, wget, curl osv) tillgängligt via Kommandotolken. 

Gör datavetenskap omfattar iteration av en serie uppgifter:

1. Hitta, läsa in och förbearbetning av data
2. Skapa och testa modeller
3. Distribuera hello modeller för användning i intelligent program

Datavetare använda olika verktyg toocomplete dessa uppgifter. Det kan vara ganska tidskrävande toofind hello lämpliga programversioner hello, och sedan ladda ned och installera dem. hello Microsoft datavetenskap virtuell dator kan minska det här problemet genom att ge en klar att använda avbildning som kan etableras på Azure alla flera populära verktyg före installeras och konfigureras. 

hello Microsoft datavetenskap Virtual Machine jump-starts projektet analytics. Den låter dig toowork på aktiviteter i olika språk, inklusive R, Python, SQL och C#. Visual Studio innehåller en IDE-toodevelop och testa din kod som är lätt toouse. hello Azure SDK ingår i hello VM kan du toobuild dina program med olika tjänster på Microsofts molnplattform. 

Det finns inga avgifter för programvara för den här datavetenskap VM-avbildning. Du betalar bara för hello Azure användning avgifter vilka beroende hello storleken på hello virtuella datorn du etablera. Mer information om hello compute avgifter kan hittas i hello priser informationsavsnittet på hello [datavetenskap virtuella](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) sidan. 

## <a name="other-versions-of-hello-data-science-virtual-machine"></a>Andra versioner av hello datavetenskap virtuell dator
En [CentOS](machine-learning-data-science-linux-dsvm-intro.md) bilden är också tillgänglig, med många av hello samma verktyg som hello Windows-avbildning. En [Ubuntu](machine-learning-data-science-dsvm-ubuntu-intro.md) bilden är tillgänglig, med många liknande verktyg plus djup learning ramverk.

## <a name="prerequisites"></a>Krav
Innan du kan skapa en Microsoft datavetenskap virtuell dator, måste du ha hello följande:

* **En Azure-prenumeration**: tooobtain en, se [hämta kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Ett Azure storage-konto**: toocreate en, se [skapa ett Azure storage-konto](../storage/common/storage-create-storage-account.md#create-a-storage-account). Du kan också kan hello storage-konto skapas som en del av hello processen att skapa hello VM om du inte vill att toouse ett befintligt konto.

## <a name="create-your-microsoft-data-science-virtual-machine"></a>Skapa din Microsoft Data vetenskap virtuell dator
Här följer hello steg toocreate en instans av hello Microsoft datavetenskap virtuell dator:

1. Navigera toohello virtuella lista på [Azure-portalen](https://portal.azure.com/#create/microsoft-ads.standard-data-science-vmstandard-data-science-vm).
2. Välj hello **skapa** längst hello nedre toobe tas med i en guide.![ Konfigurera-data-vetenskap-vm](./media/machine-learning-data-science-provision-vm/configure-data-science-virtual-machine.png)
3. hello guiden används toocreate hello kräver att Microsoft datavetenskap Virtual Machine **indata** för varje hello **fem steg** räknas upp på hello höger i den här bilden. Här följer hello indata behövs tooconfigure varje av de här stegen:
   
   1. **Grundläggande inställningar**
      
      1. **Namnet**: namnet på din server för vetenskap av data som du skapar.
      2. **Användarnamnet**: Admin inloggnings-id för kontot.
      3. **Lösenordet**: Admin kontolösenord.
      4. **Prenumerationen**: Om du har mer än en prenumeration väljer hello en på vilka hello datorn är toobe skapas och debiteras.
      5. **Resursgruppen**: du kan skapa en ny eller Använd en befintlig grupp.
      6. **Plats**: Välj hello datacenter som är mest lämplig. Det är vanligtvis hello datacenter som har de flesta av dina data eller som är närmast tooyour fysiska platsen för snabbaste nätverksåtkomst.
   2. **Storlek**: Välj något av hello servertyper som uppfyller dina krav på funktionsnivå och kostnaden begränsningar. Du får fler alternativ för VM-storlekar genom att välja ”Visa alla”.
   3. **Inställningar för**:
      
      1. **Disktyp**: väljer Premium om du föredrar en SSD-enhet (SSD), annars väljer du ”Standard”.
      2. **Lagringskontot**: du kan skapa ett nytt Azure storage-konto i din prenumeration eller använda en befintlig i hello samma *plats* som har valts på hello **grunderna** steg hello guiden.
      3. **Andra parametrar**: vanligtvis du bara använda hello standardvärden. Hovra över hello informationsmeddelande länken Hjälp om hello specifika fält om du vill ha tooconsider hello användning av icke-standardvärden.
   4. **Sammanfattning**: Kontrollera att all information du angett är korrekt.
   5. **Köpa**: Klicka på **köpa** toostart hello etablering. En länk som toohello användningsvillkor hello transaktion. hello VM har inte några ytterligare kostnader utöver hello beräkning för hello server storlek som du valde i hello **storlek** steg. 

> [!NOTE]
> hello etablering bör ta ungefär 10 20 minuter. hello status för etablering av hello visas på hello Azure-portalen.
> 
> 

## <a name="how-tooaccess-hello-microsoft-data-science-virtual-machine"></a>Hur tooaccess hello Microsoft datavetenskap virtuell dator
En gång hello virtuell dator skapas, kan du fjärrskrivbord till den med hjälp av hello Admin-kontouppgifter som du konfigurerade i föregående hello **grunderna** avsnitt. 

När den virtuella datorn skapas och etableras, är du redo toostart med hello verktygen som installeras och konfigureras på den. Det finns start-menyn paneler och ikoner på skrivbordet för många hello verktyg. 

## <a name="how-toocreate-a-strong-password-for-jupyter-and-start-hello-notebook-server"></a>Hur toocreate ett starkt lösenord för Jupyter och starta hello anteckningsboken server
Som standard är hello Jupyter-anteckningsbok server förkonfigurerade men inaktiverad på hello VM förrän du har angett ett Jupyter-lösenord. toocreate ett starkt lösenord för hello Jupyter notebook server installerad på datorn hello kör hello följande kommando från en kommandotolk på hello datavetenskap virtuell dator eller dubbelklicka hello genväg på skrivbordet vi kallas  **Ange lösenord för Jupyter & Start** från ett lokalt administratörskonto för virtuell dator.

    C:\dsvm\tools\setup\JupyterSetPasswordAndStart.cmd

Följ hälsningsmeddelande och välja ett starkt lösenord när du uppmanas.

hello föregående skriptet skapar en lösenords-hash och lagra den i hello Jupyter-konfigurationsfilen finns på: **C:\ProgramData\jupyter\jupyter_notebook_config.py** under hello parameternamnet ***c. NotebookApp.password***.

hello skript kan även och kör hello Jupyter server hello bakgrund. Jupyter server skapas som en windows-aktivitet i hello Schemaläggaren i WIndows **Start_IPython_Notebook**.  Du kan ha toowait för några sekunder efter att ange hello lösenord innan du öppnar hello anteckningsboken i webbläsaren. Finns hello avsnittet nedan **Jupyter-anteckningsbok** på hur tooaccess hello Jupyter-anteckningsbok server. 


## <a name="tools-installed-on-hello-microsoft-data-science-virtual-machine"></a>Verktygen som installeras på hello Microsoft datavetenskap virtuell dator

### <a name="microsoft-r-server-developer-edition"></a>Microsoft R Server Developer Edition
Om du vill toouse R för din analytics har hello VM Microsoft R Server Developer edition är installerat. Microsoft R Server är en brett distribuerbara företagsklass analytics plattform baserat på R som stöds, skalbara och säkra. R Server stöder stöder en mängd olika stordata statistik, förutsägelsemodellering och maskininlärning funktioner hello fullständig uppsättning analytics – undersökning, analys, visualisering och modellering. Genom att använda och utöka öppen källkod R är Microsoft R Server helt kompatibel med R-skript, funktioner och CRAN-paket, tooanalyze data på företagsnivå. Hello InMemory-begränsningar för öppen källa R åtgärdas även genom att lägga till parallella och chunked bearbetning av data. Detta gör att du toorun analyser på data som är mycket större än vad som passar i huvudminnet.  Visual Studio Community Edition på hello VM innehåller hello R verktyg för Visual Studio-tillägg som ger en fullständig IDE för att arbeta med R. Du kan också hämta och använda andra IDEs samt som [RStudio](http://www.rstudio.com). 

### <a name="python"></a>Python
För utveckling med hjälp av Python, har distribution av Anaconda Python 2.7 och 3.5 installerats. Den här distributionen innehåller hello basera Python tillsammans med ungefär 300 hello populäraste matematiska, ingenjörer och data analytics paket. Du kan använda Python Tools för Visual Studio (PTVS) som är installerade på hello Visual Studio 2015 Community edition eller någon av hello IDEs tillsammans med Anaconda som INAKTIV eller Spyder. Du kan starta en av dessa genom att söka i sökfältet hello (**Win** + **S** nyckel).

> [!NOTE]
> toopoint hello Python Tools för Visual Studio på Anaconda Python 2.7 och 3.5, behöver du toocreate anpassade miljöer för varje version. tooset sökvägarna miljö i hello Visual Studio 2015 Community Edition, navigera för**verktyg** -> **Python Tools** -> **Python-miljöer** och klicka sedan på **+ anpassad**. 
> 
> 

Anaconda Python 2.7 installeras under C:\Anaconda och Anaconda Python 3.5 är installerat under c:\Anaconda\envs\py35. Se [dokumentationen till PTVS](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) detaljerade anvisningar. 

### <a name="jupyter-notebook"></a>Jupyter Notebook
Anaconda distribution innehåller också en Jupyter-anteckningsbok, en miljö tooshare koden och analys. En Jupyter-anteckningsbok server har konfigurerats med Python 2.7, Python 3.4, Python 3.5 och R kärnor före. Det finns en skrivbordsikon med namnet ”Jupyter-anteckningsbok toolaunch hello webbläsare tooaccess hello anteckningsboken server. Om du är på hello VM via fjärrskrivbord kan du kan också besöka [https://localhost:9999 /](https://localhost:9999/) tooaccess hello Jupyter-anteckningsbok server när inloggad toohello VM.

> [!NOTE]
> Fortsätt om du får några certifikatvarningar. 
> 
> 

Vi har paketerat flera exempel bärbara datorer i Python och i R. hello Jupyter-anteckningsböcker visa hur toowork med Microsoft R Server, SQL Server 2016 R Services (i databasen analytics), Python, Microsoft kognitiva ToolKit (CNTK) för djup och andra Azure tekniker när du loggar in tooJupyter. Du kan se hello länken toohello exempel på startsidan för hello bärbar dator när du autentiserar toohello Jupyter-anteckningsbok med hello lösenordet du skapade tidigare. 

### <a name="visual-studio-2015-community-edition"></a>Visual Studio 2015 Community edition
Visual Studio Community edition är installerade på hello VM. Det är en kostnadsfri version av hello populära IDE från Microsoft som du kan använda i utvärderingssyfte och för små team. Du kan checka ut hello licensvillkoren [här](https://www.visualstudio.com/support/legal/mt171547).  Öppna Visual Studio genom att dubbelklicka på hello skrivbordsikon eller hello **starta** menyn. Du kan också söka efter program med **Win** + **S** och ange ”Visual Studio”. När det kan du skapa projekt i språk som C#, Python, R, node.js. Plugin-program installeras även som gör det enkelt toowork med Azure-tjänster som Azure Data Catalog, Azure HDInsight (Hadoop, Spark) och Azure Data Lake. 

> [!NOTE]
> Du får ett meddelande om att utvärderingsperioden har upphört att gälla. Ange autentiseringsuppgifterna för ditt Microsoft-konto eller skapa en ny gratiskonto tooget åtkomst toohello Visual Studio Community Edition. 
> 
> 

### <a name="sql-server-2016-developer-edition"></a>SQL Server 2016 Developer edition
En utvecklare version av SQL Server 2016 med R Services toorun i databasen analytics finns på hello VM. R tillhandahåller en plattform för att utveckla och distribuera intelligent program. Du kan använda hello omfattande och kraftfulla R-språk och hello många paket från hello community toocreate modeller och generera förutsägelser för SQL Server-data. Du kan behålla Stäng toohello analysdata eftersom R-tjänster (i databasen) integrerade hello R språk med SQL Server. Detta eliminerar hello kostnader och säkerhetsriskerna med dataflyttning.

> [!NOTE]
> hello SQL Server 2016 developer edition kan bara användas för utveckling och testsyfte. Du behöver en licens toorun den i produktion. 
> 
> 

Du kan komma åt hello SQLServer genom att starta **SQL Server Management Studio**. VM-namn fylls som hello servernamn. Använd Windows-autentisering när du loggade in som administratör på Windows hello. När du är i SQL Server Management Studio kan du skapa andra användare, skapa databaser, importera data och köra SQL-frågor. 

tooenable i databasen analytics med hjälp av Microsoft-R, kör följande kommando som en en hello tid åtgärden i SQL Server management studio när du loggar in som administratör för hello-server. 

        CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS 

        (Please replace hello %COMPUTERNAME% with your VM name)


### <a name="azure"></a>Azure
Flera Azure tools är installerat på hello VM:

* Det finns en genväg på skrivbordet tooaccess hello Azure SDK-dokumentationen. 
* **AzCopy**: används toomove data till och från Microsoft Azure Storage-konto. toosee användning av typen **Azcopy** vid en kommandotolk toosee hello användning. 
* **Microsoft Azure Lagringsutforskaren**: används toobrowse via hello-objekt som du har lagrat i ditt Azure Storage-konto och överför data tooand från Azure storage. Du kan skriva **Lagringsutforskaren** i Sök eller hitta programmet på hello Windows Start-menyn tooaccess det här verktyget. 
* **Adlcopy**: används toomove data tooAzure Data Lake. toosee användning av typen **adlcopy** i en kommandotolk. 
* **dtui**: används toomove data tooand från Azure Cosmos DB, en NoSQL-databas på hello moln. Typen **dtui** i Kommandotolken. 
* **Microsoft Data Management Gateway**: gör det möjligt för flytt av data mellan lokala datakällor och molnet. Den används i verktyg som Azure Data Factory. 
* **Microsoft Azure Powershell**: ett verktyg som används tooadminister Azure-resurser i hello Powershell skriptspråk som också är installerad på den virtuella datorn. 

### <a name="power-bi"></a>Power BI
toohelp du skapar instrumentpaneler och bra visualiseringar hello **Power BI Desktop** har installerats. Använd det här verktyget toopull data från olika källor, tooauthor dina instrumentpaneler och rapporter och toopublish dem toohello moln. Mer information finns i hello [Power BI](http://powerbi.microsoft.com) plats. Du hittar Power BI desktop på hello Start-menyn. 

> [!NOTE]
> Du behöver en Office 365-konto tooaccess Power BI. 
> 
> 

## <a name="additional-microsoft-development-tools"></a>Ytterligare Microsoft-utvecklingsverktyg
Hej [ **Microsoft Web Platform Installer** ](https://www.microsoft.com/web/downloads/platform.aspx) kan använda toodiscover och ladda ned andra Microsoft-utvecklingsverktyg. Det finns också ett genvägen toohello verktyg som tillhandahålls på hello Microsoft datavetenskap virtuella skrivbord.  

## <a name="important-directories-on-hello-vm"></a>Viktiga kataloger på hello VM
| Objekt | Katalog |
| --- | --- |
| Serverkonfigurationer för Jupyter-anteckningsbok |C:\ProgramData\jupyter |
| Arbetskatalog för Jupyter Notebook-exempel |c:\dsvm\notebooks |
| Andra exempel |c:\dsvm\samples |
| Anaconda (standard: Python 2.7) |c:\Anaconda |
| Anaconda Python 3.5 miljö |c:\Anaconda\envs\py35 |
| R Server fristående instans directory (standard R-instans utifrån R3.2.2) |C:\Program Files\Microsoft SQL Server\130\R_SERVER |
| R Server i databasen instans directory (R3.2.2) |C:\Program Files\Microsoft SQL Server\MSSQL13. MSSQLSERVER\R_SERVICES |
| Microsoft R öppna (R3.3.1) instans directory |C:\Program Files\Microsoft\MRO-3.3.1 |
| Diverse verktyg |c:\dsvm\tools |

> [!NOTE]
> Instanser av hello Microsoft datavetenskap virtuell dator som skapats före 1.5.0 (före 3 September 2016) används en något annorlunda katalogstruktur än i föregående tabell hello. 
> 
> 

## <a name="next-steps"></a>Nästa steg
Här följer några nästa steg toocontinue din inlärning och undersökning. 

* Utforska hello olika datavetenskap verktyg på hello datavetenskap VM genom att klicka på hello start-menyn och checka ut hello verktyg på hello-menyn.
* Navigera för**C:\Program Files\Microsoft SQL Server\130\R_SERVER\library\RevoScaleR\demoScripts** för exempel som använder hello RevoScaleR bibliotek i R som har stöd för dataanalys på företagsnivå.  
* Läs hello artikel: [10 saker du kan göra på hello datavetenskap virtuell dator](http://aka.ms/dsvmtenthings)
* Lär dig hur toobuild end tooend Analyslösningar systematiskt med hello [Team datavetenskap Process](https://azure.microsoft.com/documentation/learning-paths/data-science-process/).
* Besök hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com) för machine learning och analytics exempel för att använda hello Cortana Intelligence Suite. Vi har också tillhandahålls en ikon på hello **starta** menyn och på hello skrivbord hello virtuella toothis gallery.

