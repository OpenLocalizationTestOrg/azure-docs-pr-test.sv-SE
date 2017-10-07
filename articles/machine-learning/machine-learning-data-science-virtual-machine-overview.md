---
title: "aaaWhat är en datavetenskap virtuell dator? | Microsoft Docs"
description: "Hur igång tooget göra viktiga analytics scenarier med datavetenskap virtuella datorer."
keywords: "datavetenskap verktyg, datavetenskap virtuell dator, verktyg för datavetenskap, datavetenskap för linux"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: d4f91270-dbd2-4290-ab2b-b7bfad0b2703
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: gokuma;bradsev
ms.openlocfilehash: 18c7a75208671c663f3b6be6ee8d0bf666772e01
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toohello-cloud-based-data-science-virtual-machine-for-linux-and-windows"></a>Introduktion toohello molnbaserade datavetenskap virtuell dator för Linux och Windows
hello Data vetenskap virtuell dator (DSVM) är en anpassad VM-avbildning på Microsoft Azure-molnet som skapats specifikt för att göra datavetenskap. Det har många populära datavetenskap och andra verktyg förinstallerat och förkonfigurerade toojump-börja utveckla intelligent program för avancerade analyser. Den är tillgänglig på Windows Server och Linux. Vi erbjuder Windows-versionen av DSVM på Server 2016 och Server 2012. Vi erbjuder Linux-versionen av hello DSVM på Ubuntu 16.04 LTS och OpenLogic 7.2 CentOS-baserade Linux-distributioner. 

Det här avsnittet beskrivs vad du kan göra med hello datavetenskap VM, en översikt över hello viktiga scenarier för att använda hello VM, specificerar hello viktiga funktioner som är tillgängliga på hello Windows och Linux-versioner och innehåller instruktioner om hur tooget igång med dem.

## <a name="what-can-i-do-with-hello-data-science-virtual-machine"></a>Vad kan jag göra med hello datavetenskap virtuella datorn?
hello målet med hello datavetenskap virtuella datorn är tooprovide data tekniker hos alla funktioner och roller med en kostnadsfri friktion datavetenskap miljö. Den här virtuella datorn sparar mycket tid som du tog henne om du har distribuerat en jämförbar miljö på egen hand. Starta projektet datavetenskap i stället, direkt i en nyligen skapade VM-instans. 

hello datavetenskap VM är utformad och konfigurerad för att arbeta med en bred Användningsscenarier. Du kan skala din miljö uppåt eller nedåt när projektet behöver ändras. Du är kan toouse önskat språk tooprogram datavetenskap aktiviteterna. Du kan installera andra verktyg och anpassa hello system för dina specifika behov.

## <a name="key-scenarios"></a>Viktiga scenarier
Det här avsnittet beskrivs några viktiga scenarier för vilka hello datavetenskap VM kan distribueras.

### <a name="preconfigured-analytics-desktop-in-hello-cloud"></a>Förkonfigurerade analytics skrivbordet i hello moln
hello datavetenskap VM innehåller en grundläggande konfiguration för datavetenskap team söker tooreplace sina lokala datorer med en hanterad dator. Den här baslinjen säkerställer att alla hello datavetare i ett team har en konsekvent konfiguration med vilka tooverify experiment och främja samarbete. Minskar även kostnaderna genom att minska hello sysadmin belastningen och sparar på hello tid behövs tooevaluate, installera och underhålla hello olika programvarupaket behövs toodo avancerade analyser.  

### <a name="data-science-training-and-education"></a>Kurser och utbildning om datavetenskap
Ange en virtuell dator avbildningen tooensure att deras studenter har en konsekvent konfiguration och att hello exempel fungerar förutsägbart Enterprise lärare och lärare som vanligtvis utbilda vetenskap dataklasser. hello datavetenskap VM skapar en på-begäran-miljö med en konsekvent konfiguration som underlättar hello support och inkompatibilitet utmaningar. Fall där dessa miljöer måste toobe inbyggda ofta, särskilt för kortare utbildning klasser, dra avsevärt.

### <a name="on-demand-elastic-capacity-for-large-scale-projects"></a>På begäran elastisk kapacitet för storskaliga projekt
Datavetenskap hackathons/tävlingar eller storskaliga datamodellering och utforskning kräver utskalad maskinvarukapacitet, vanligtvis under kort tid. hello datavetenskap VM kan hjälpa replikera hello datavetenskap miljö snabbt på begäran, skaländras ut servrar som tillåter experiment som kräver kraftfulla databehandling resurser toobe körs.

### <a name="short-term-experimentation-and-evaluation"></a>Kortsiktig undersökningar och utvärdering
hello datavetenskap VM kan använda tooevaluate eller Läs verktyg som Microsoft R Server, SQL Server, Visual Studio tools, Jupyter, djupa learning / ML verktyg och nya verktyg som är vanliga i hello community med minimal installation ansträngning. Eftersom hello datavetenskap VM kan ställas in snabbt, kan den användas i andra kortsiktig användarscenarier som replikerar publicerade experiment, köra demonstrationer följande genomgång i online sessioner eller konferens självstudier.

### <a name="deep-learning"></a>Djupgående learning
Hej datavetenskap VM kan användas för utbildning-modell med djup learning algoritmer på GPU (Graphics bearbetningsenheter) baserad maskinvara. Använda VM skalning maskinvarufunktioner för Azure-molnet kan DSVM du använda GPU-baserad maskinvara på hello molnet enligt behov. En kan växla tooa GPU baserat VM när utbildning stora modeller eller behöver hög hastighet beräkningar medan hello samma OS-disken.  hello Windows Server 2016 version av DSVM finns förinstallerat med GPU drivrutiner, ramverk och GPU-versionen av hello djup learning algoritmer. Hello Linux djup learning på GPU är aktiverat på hello [datavetenskap virtuell dator för Linux (Ubuntu) edition](http://aka.ms/dsvm/ubuntu). Du kan distribuera hello 2016-Ubuntu-Windows-versionen av datavetenskap VM toonon GPU-baserad virtuell dator i Azure i så fall kommer alla hello djup learning ramverk återställningsplats toohello CPU-läge. Tidigare, för Windows Server 2012 vi publicerade en [djup learning toolkit](http://aka.ms/dsvm/deeplearning) men nu bör du använda Windows Server 2016 för Windows-baserade djup learning arbetsbelastningar. hello baserat på CentOS Linux-versionen av hello DSVM innehåller endast hello CPU bygger på några av hello djup lära (CNTK, Tensorflow, MXNet) men kommer inte förinstallerat hello GPU drivrutiner och ramverk. 

## <a name="whats-included-in-hello-data-science-vm"></a>Vad ingår i hello datavetenskap VM?
hello datavetenskap virtuell dator har många populära datavetenskap och djup lära redan har installerats och konfigurerats. Den omfattar också verktyg som gör det enkelt toowork olika Azure-data och analyser produkter. Du kan utforska och skapa förutsägelsemodeller i stora datauppsättningar genom att använda hello Microsoft R Server eller SQL Server 2016. En mängd andra verktyg från hello öppen källkod och från Microsoft finns också inkluderade som exempel koden och bärbara datorer. hello följande tabell specificerar och jämför hello huvudkomponenterna i hello Windows och Linux-versionerna av hello datavetenskap virtuell dator.


| **Verktyget**                                                           | **Windows-version** | **Linux-version** |
| :------------------------------------------------------------------ |:-------------------:|:------------------:|
| [Microsoft R öppna](https://mran.microsoft.com/open/) med populära paket förinstallerat   |Y                      | Y             |
| [Microsoft R Server](https://msdn.microsoft.com/microsoft-r/) Developer Edition innehåller <br />  &nbsp;&nbsp;&nbsp;&nbsp;* [ScaleR](https://msdn.microsoft.com/microsoft-r/scaler-getting-started) parallella och distribuerade högpresterande R framework<br />  &nbsp;&nbsp;&nbsp;&nbsp;* [MicrosoftML](https://msdn.microsoft.com/microsoft-r/microsoftml-introduction) -nya den senaste ML algoritmer från Microsoft <br />  &nbsp;&nbsp;&nbsp;&nbsp;* [R Operationalization](https://msdn.microsoft.com/microsoft-r/operationalize/about)                                            |Y                      | Y <br/> (MicrosoftML ännu inte tillgänglig)|
| [Microsoft Office](https://products.office.com/en-us/business/office-365-proplus-business-software) Pro-Plus med delade aktivering - Excel, Word och PowerPoint   |Y                      |N              |
| [Anaconda Python](https://www.continuum.io/) 2.7, 3.5 med populära paket förinstallerat    |Y                      |Y              |
| [JuliaPro](https://juliacomputing.com/products/juliapro.html) med populära paket för Julia språket förinstallerat                         |Y                      |Y              |
| Relationsdatabaser                                                            | [SQL Server 2016 SP1](https://www.microsoft.com/sql-server/sql-server-2016) <br/> Developer Edition| [PostgreSQL](https://www.postgresql.org/) |
| Databasverktyg                                                       | * SQL Server Management Studio <br/>* SQL Server Integration Services<br/>* [BCP sqlcmd](https://docs.microsoft.com/sql/tools/command-prompt-utility-reference-database-engine)<br /> * ODBC/JDBC-drivrutiner| * [SQuirreL SQL](http://squirrel-sql.sourceforge.net/) (frågar verktyget), <br /> * bcp sqlcmd <br /> * ODBC/JDBC-drivrutiner|
| Skalbar i databasen analytics med SQL Server-R-tjänster | Y     |N              |
| **[Jupyter-anteckningsbok Server](http://jupyter.org/) med följande kärnor**                                  | Y     | Y |
|     &nbsp;&nbsp;&nbsp;&nbsp;* R | Y | Y |
|     &nbsp;&nbsp;&nbsp;&nbsp;* Python 2.7 & 3.5 | Y | Y |
|     &nbsp;&nbsp;&nbsp;&nbsp;* Julia | Y | Y |
|     &nbsp;&nbsp;&nbsp;&nbsp;* PySpark | N | Y |
|     &nbsp;&nbsp;&nbsp;&nbsp;* [Sparkmagic](https://github.com/jupyter-incubator/sparkmagic) | N | Y (Ubuntu) |
|     &nbsp;&nbsp;&nbsp;&nbsp;* SparkR     | N | Y |
| JupyterHub (flera användares anteckningsböcker server)| N | Y |
| **Utvecklingsverktyg, IDEs och kod redigerare**| | |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Visual Studio 2017 (Community Edition)](https://www.visualstudio.com/community/) > med Git Plugin, Azure HDInsight (Hadoop), Data Lake, SQL Server Data tools [Node.js](https://github.com/Microsoft/nodejstools), [Python](http://aka.ms/ptvs), och [R Verktyg för Visual Studio (RTVS)](http://microsoft.github.io/RTVS-docs/) | Y | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Visual Studio Code](https://code.visualstudio.com/) | Y | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;* [RStudio Desktop](https://www.rstudio.com/products/rstudio/#Desktop) | Y | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;* [RStudio Server](https://www.rstudio.com/products/rstudio/#Server) | N | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;* [PyCharm](https://www.jetbrains.com/pycharm/) | N | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Atom](https://atom.io/) | N | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Juno (Julia IDE)](http://junolab.org/)| Y | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;* Vim och Emacs | Y | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;* Git och GitBash | Y | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;* OpenJDK | Y | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;* .net framework | Y | N |
| PowerBI Desktop | Y | N |
| SDK: er tooaccess Azure och Cortana Intelligence Suite av tjänster | Y | Y |
| **Dataförflyttning och hanteringsverktyg** | | |
| &nbsp;&nbsp;&nbsp;&nbsp;* Azure Lagringsutforskaren | Y | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Azure CLI](https://docs.microsoft.com/cli/azure/overview) | Y | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;* Azure Powershell | Y | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Azcopy](https://docs.microsoft.com/azure/storage/storage-use-azcopy) | Y | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Adlcopy (Azure Data Lake lagring)](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob) | Y | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [DocDB Datamigreringsverktyg](https://docs.microsoft.com/azure/documentdb/documentdb-import-data) | Y | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Microsoft Data Management Gateway](https://msdn.microsoft.com/library/dn879362.aspx) : flytta data mellan OnPrem och moln | Y | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* Unix/Linux-kommandoradsverktyg | Y | Y |
| [Apache ökad](http://drill.apache.org) för datagranskning | Y | Y |
| **Machine Learning-verktyg** |||
| &nbsp;&nbsp;&nbsp;&nbsp;* Integrering med [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) (R, Python) | Y | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Xgboost](https://github.com/dmlc/xgboost) | Y | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit) | Y | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Weka](http://www.cs.waikato.ac.nz/ml/weka/) | Y | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Spännen](http://rattle.togaware.com/) | Y | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;* [LightGBM](https://github.com/Microsoft/LightGBM) | N | Y (Ubuntu) |
| &nbsp;&nbsp;&nbsp;&nbsp;* [H2O](https://www.h2o.ai/h2o/) | N | Y (Ubuntu) |
| **GPU-baserad djup Learning-verktyg** |Windows Server 2016 version  |Ubuntu edition |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Microsoft kognitiva Toolkit (CNTK)](http://cntk.ai) | Y | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Tensorflow](https://www.tensorflow.org/) | Y | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;* [MXNet](http://mxnet.io/) | Y | Y|
| &nbsp;&nbsp;&nbsp;&nbsp;* [Caffe & Caffe2](https://github.com/caffe2/caffe2) | N | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Torch](http://torch.ch/) | N | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Theano](https://github.com/Theano/Theano) | N | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Keras](https://keras.io/)| N | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;* [NVidia siffror](https://github.com/NVIDIA/DIGITS) | N | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;* [CUDA CUDNN, Nvidia drivrutin](https://developer.nvidia.com/cuda-toolkit) | Y | Y |
| **Stordataplattform (Devtest)**|||
| &nbsp;&nbsp;&nbsp;&nbsp;* Lokala [Spark](http://spark.apache.org/) fristående | N | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;* Lokala [Hadoop](http://hadoop.apache.org/) (HDFS, YARN) | N | Y |



## <a name="how-tooget-started-with-hello-windows-data-science-vm"></a>Hur tooget igång med hello Windows datavetenskap VM
* Skapa en instans av hello önskad Windows DSVM edition genom att gå till
  * [Windows Server 2016 baserat DSVM](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.windows-data-science-vm)
  
  eller 
  * [Windows Server 2012 baserad DSVM](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) 
* Klicka på hello **hämta IT nu** knappen.
* Logga in toohello VM från fjärrskrivbordet med hello autentiseringsuppgifter du angav när du skapade hello VM.
* toodiscover och starta hello verktygen, klicka på hello **starta** menyn.

## <a name="get-started-with-hello-linux-data-science-vm"></a>Kom igång med hello Linux datavetenskap VM
* Skapa en instans av hello önskad Linux DSVM edition genom att navigera för
  * [Ubuntu baserat DSVM](http://aka.ms/dsvm/ubuntu)

  eller

  * [OpenLogic CentOS baserat DSVM](http://aka.ms/dsvm/centos)

  
* Klicka på hello **blir det nu** knappen.
* Logga in toohello virtuell dator från en SSH-klienten, till exempel Putty eller SSH-kommando, med hello autentiseringsuppgifter du angav när du skapade hello VM.
* Ange dsvm mer information i Kommandotolken för hello-gränssnittet.
* Ett grafiskt skrivbord hämta hello X2Go klienten för din klientplattform [här](http://wiki.x2go.org/doku.php/doc:installation:x2goclient) och följer instruktionerna för hello i hello Linux datavetenskap VM dokumentet [etablera hello Linux datavetenskap virtuella](machine-learning-data-science-linux-dsvm-intro.md#installing-and-configuring-x2go-client).

## <a name="next-steps"></a>Nästa steg
### <a name="for-hello-windows-data-science-vm"></a>För hello Windows datavetenskap VM
* Mer information om hur toorun specifika verktyg på hello Windows-versionen finns [etablera hello Microsoft datavetenskap Virtual Machine](machine-learning-data-science-provision-vm.md) och
* Mer information om hur tooperform olika uppgifter som behövs för projektet datavetenskap på hello Windows VM Se [tio saker du kan göra på hello datavetenskap virtuella](machine-learning-data-science-vm-do-ten-things.md).

### <a name="for-hello-linux-data-science-vm"></a>För hello Linux datavetenskap VM
* Mer information om hur toorun specifika verktyg på hello Linux-version finns [etablera hello Linux datavetenskap virtuella](machine-learning-data-science-linux-dsvm-intro.md).
* En genomgång som visar hur tooperform flera vanliga datavetenskap aktiviteter med hello Linux VM finns [datavetenskap på hello Linux datavetenskap virtuella](machine-learning-data-science-linux-dsvm-walkthrough.md).

