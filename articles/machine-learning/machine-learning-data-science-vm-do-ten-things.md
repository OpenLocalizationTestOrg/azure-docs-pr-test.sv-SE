---
title: "aaaTen saker du kan göra på hello datavetenskap virtuella | Microsoft Docs"
description: "Utför olika datagranskning och modellering aktivitet på hello datavetenskap virtuella datorn."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 145dfe3e-2bd2-478f-9b6e-99d97d789c62
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: gokuma;weig;bradsev
ms.openlocfilehash: 4dfe22f14f00208c63e26ce44b05123c9ac4b850
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="ten-things-you-can-do-on-hello-data-science-virtual-machine"></a>Tio saker du kan göra på hello datavetenskap virtuell dator
hello Microsoft Data vetenskap virtuell dator (DSVM) är en kraftfull datavetenskap utvecklingsmiljö som gör att du tooperform olika data från kartläggning av naturresurser och modellering uppgifter. hello miljön kommer redan inbyggda och anpassade med flera populära data analytics verktyg som gör det enkelt tooget snabbt igång med din analys för lokalt, moln eller hybrid distributioner. Hej DSVM användas tillsammans med många Azure-tjänster och är kan tooread och bearbetning av data som redan finns på Azure, Azure SQL Data Warehouse, Azure Data Lake, Azure Storage eller Azure Cosmos DB. Det kan också använda andra verktyg för analys, till exempel Azure Machine Learning och Azure Data Factory.

I den här artikeln vi dig igenom hur toouse din DSVM tooperform olika datavetenskap uppgifter och interagera med andra Azure-tjänster. Här följer några av hello saker du kan göra på hello DSVM:

1. Utforska data och utveckla modeller lokalt på hello DSVM som använder Microsoft-Server för R, Python
2. Använda en Jupyter-anteckningsbok tooexperiment med dina data i en webbläsare med hjälp av Python 2, 3 Python Microsoft R redo enterprise-version av R som utformats för skalbarhet och prestanda
3. Operationalisera modeller som skapats med R och Python i Azure Machine Learning så att klientprogram kan komma åt modeller med hjälp av ett enkelt Webbgränssnitt för tjänster
4. Administrera Azure-resurser med Azure-portalen eller Powershell
5. Utöka ditt lagringsutrymme och dela stora datauppsättningar / kod över hela teamet genom att skapa ett Azure File storage som en enhet som kan monteras på din DSVM
6. Dela kod med din grupp med GitHub och åtkomst till din databas med hjälp av hello förinstallerade Git klienter – Git Bash Git GUI.
7. Komma åt olika Azure data och analyser tjänster som Azure blob storage, Azure Data Lake, Azure HDInsight (Hadoop), Azure Cosmos DB Azure SQL Data Warehouse och databaser
8. Skapa rapporter och instrumentpanel med hjälp av hello Power BI Desktop förinstallerat på hello DSVM och distribuera dem på hello moln
9. Dynamiskt skala din DSVM toomeet måste ditt projekt
10. Installera ytterligare verktyg på den virtuella datorn   

> [!NOTE]
> Extra kostnader gäller för många hello ytterligare lagring och analyser datatjänster i den här artikeln. Se toohello [priser för Azure](https://azure.microsoft.com/pricing/) information på sidan.
> 
> 

**Förutsättningar**

* Du behöver en Azure-prenumeration. Du kan registrera dig för en kostnadsfri utvärderingsversion [här](https://azure.microsoft.com/free/).
* Instruktioner för att etablera en datavetenskap virtuell dator på hello Azure-portalen finns på [skapar en virtuell dator](https://portal.azure.com/#create/microsoft-ads.standard-data-science-vmstandard-data-science-vm).

## <a name="1-explore-data-and-develop-models-using-microsoft-r-server-or-python"></a>1. Utforska data och utveckla modeller med hjälp av Microsoft Server för R eller Python
Du kan använda språk som R och Python toodo din dataanalys åt höger på hello DSVM.

Du kan använda en IDE kallas ”Revolution R Enterprise 8.0” som finns på hello start-menyn eller hello skrivbord för R. Microsoft tillhandahåller ytterligare bibliotek ovanpå hello öppna R-käll-CRAN tooenable skalbara analytics hello möjlighet tooanalyze data och större än hello minnesstorleken tillåts genom att göra parallella chunked analys. Du kan också installera IDE R av din choice liknande [RStudio](https://www.rstudio.com/products/rstudio-desktop/).

För Python, kan du använda ett IDE som Visual Studio Community Edition som har hello Python Tools för Visual Studio (PTVS)-tillägg förinstallerat. Som standard konfigureras endast en grundläggande Python 2.7 på PTVS (utan något analytics bibliotek som SciKit Pandas). I ordning tooenable Anaconda Python 2.7 och 3.5 behöver du toodo hello följande:

* Skapa anpassade miljöer för varje version genom att navigera för**verktyg** -> **Python Tools** -> **Python-miljöer** och sedan klicka på ” **+ Anpassad**”i hello Visual Studio 2015 Community Edition
* Ge en beskrivning och ange hello miljö prefix sökvägar som *c:\anaconda* för Anaconda Python 2.7 eller *c:\anaconda\envs\py35* för Anaconda Python 3.5
* Klicka på **automatisk identifiering** och sedan **tillämpa** toosave hello miljö.

Här är vad hello anpassad miljö ser ut som i Visual Studio.

![PTVS installationen](./media/machine-learning-data-science-vm-do-ten-things/PTVSSetup.png)

Se hello [dokumentationen till PTVS](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) för ytterligare information om hur toocreate Python-miljöer.

Nu har du konfigurerat toocreate ett nytt Python-projekt. Navigera för**filen** -> **ny** -> **projekt** -> **Python** och ange hello Python-program som du skapar. Du kan ange hello Python-miljö för hello aktuella projektet toohello önskade versionen (Anaconda 2.7 eller 3.5): Högerklicka på hello **Python-miljö**väljer **Lägg till/ta bort Python-miljöer**, och sedan väljer hello önskade miljö tooassociate med hello-projekt. Du hittar mer information om hur du arbetar med PTVS hello produkten [dokumentationen](https://github.com/Microsoft/PTVS/wiki) sidan.

## <a name="2-using-a-jupyter-notebook-tooexplore-and-model-your-data-with-python-or-r"></a>2. Med en Jupyter-anteckningsbok tooexplore och modellen dina data med Python eller R
Hej Jupyter-anteckningsbok är en kraftfull miljö som tillhandahåller en webbaserat ”IDE” för modellering och undersökning av data. Du kan använda Python 2, Python 3 eller R (öppen källkod och hello Microsoft R Server) i en Jupyter-anteckningsbok.

toolaunch hello Jupyter-anteckningsbok klickar du på ikonen för hello start-menyn / skrivbordsikon med titeln **Jupyter-anteckningsbok**. På hello DSVM du kan även bläddra för ”https://localhost:9999 /” tooaccess hello Jupiter bärbar dator. Om ombeds du ange ett lösenord, använder du instruktionerna i hello ***hur toocreate ett starkt lösenord på hello Jupyter-anteckningsbok server*** avsnitt i hello [etablera hello Microsoft datavetenskap Virtual Machine](machine-learning-data-science-provision-vm.md)avsnittet toocreate ett starkt lösenord tooaccess hello Jupyter-anteckningsbok. 

När du har öppnat hello bärbar dator, bör du se en katalog som innehåller några exempel bärbara datorer är förhand packade i hello DSVM. Nu kan du:

* Klicka på hello anteckningsboken toosee hello kod.
* Köra varje cell genom att trycka på **SKIFT-ange**.
* Kör hello anteckningsboken genom att klicka på **Cell** -> **kör**
* Skapa en ny anteckningsbok genom att klicka på hello Jupyter-ikonen (övre vänstra hörnet) och klicka på **ny** knappen hello högra och sedan välja hello anteckningsboken språk (även kallat kernel).   

> [!NOTE]
> För närvarande stöder vi Python 2.7, Python 3.5 och R. hello R kernel stöder programmering i både öppen källkod R samt hello enterprise skalbara Microsoft R Server.   
> 
> 

När du är i hello anteckningsbok du utforska dina data, skapa hello modell, testa hello modellen med ditt val av bibliotek.

## <a name="3-build-models-using-r-or-python-and-operationalize-them-using-azure-machine-learning"></a>3. Bygga modeller med R eller Python och Operationalize dem med hjälp av Azure Machine Learning
När du har skapat och verifiera modellen hello nästa steg är vanligtvis toodeploy den i produktion. Detta gör din klienten program tooinvoke hello modellen förutsägelser på en realtid eller på grundval av batch-läge. Azure Machine Learning tillhandahåller en mekanism toooperationalize en modell som skapats i R eller Python.

När du driftsätta modellen i Azure Machine Learning exponeras en webbtjänst som tillåter att klienter toomake REST-anrop som skickar i indataparametrar och ta emot förutsägelser från hello modellen som utdata.   

> [!NOTE]
> Om du inte har har registrerat dig för Azure Machine Learning, kan du hämta en kostnadsfri arbetsyta eller en standard arbetsyta genom att besöka hello [Azure Machine Learning Studio](https://studio.azureml.net/) startsidan och klicka på ”Kom igång”.   
> 
> 

### <a name="build-and-operationalize-python-models"></a>Bygg- och Operationalisera Python modeller
Här är en kodavsnittet utvecklats i en Python Jupyter-anteckningsbok som skapar en enkel modell med hello SciKit Läs-biblioteket.

    #IRIS classification
    from sklearn import datasets
    from sklearn import svm
    clf = svm.SVC()
    iris = datasets.load_iris()
    X, y = iris.data, iris.target
    clf.fit(X, y)

hello-metoden används toodeploy din python modeller tooAzure Machine Learning radbryter hello förutsägelser av hello modell i en funktion och decorates med attribut som tillhandahålls av hello förinstallerad Azure Machine Learning python-bibliotek som anger din Azure-dator Learning arbetsyte-ID, API-nyckel och hello indata och returnerar parametrar.  

    from azureml import services
    @services.publish(workspaceid, auth_token)
    @services.types(sep_l = float, sep_w = float, pet_l=float, pet_w=float)
    @services.returns(int) #0, or 1, or 2
    def predictIris(sep_l, sep_w, pet_l, pet_w):
     inputArray = [sep_l, sep_w, pet_l, pet_w]
    return clf.predict(inputArray)

En klient kan nu göra anrop toohello-webbtjänsten. Det finns bekvämlighet omslutningar som skapar hello REST API-begäranden. Här är en webbtjänst till exempel kod tooconsume hello.

    # Consume through web service URL and keys
    from azureml import services
    @services.service(url, api_key)
    @services.types(sep_l = float, sep_w = float, pet_l=float, pet_w=float)
    @services.returns(float)
    def IrisPredictor(sep_l, sep_w, pet_l, pet_w):
    pass

    IrisPredictor(3,2,3,4)


> [!NOTE]
> hello Azure Machine Learning-biblioteket stöds bara på Python 2.7 för närvarande.   
> 
> 

### <a name="build-and-operationalize-r-models"></a>Bygg- och Operationalisera R modeller
Du kan distribuera R modeller som bygger på hello datavetenskap virtuella datorn eller någon annanstans på Azure Machine Learning på ett sätt som är liknande toohow är klar för Python. Hennes hello steg:

* Skapa en settings.json filen tooprovide ditt arbetsyte-ID och auth token som visas i följande kodexempel hello.
* skriva en wrapper för hello modellen förutsäga funktion.
* anropa ```publishWebService``` i hello Azure Machine Learning-biblioteket toopass i hello funktionen omslutning.  

Här är hello proceduren och koden kodavsnitt som kan använda tooset upp, skapa, publicera och använda en modell som en webbtjänst i Azure Machine Learning.

#### <a name="setup"></a>Konfiguration
1. Installera hello Machine Learning R-paket genom att skriva ```install.packages("AzureML")``` i Revolution R Enterprise 8.0 IDE eller -R-IDE.
2. Hämta RTools från [här](https://cran.r-project.org/bin/windows/Rtools/). Du måste hello zip-verktyget i hello sökväg (och namngivna zip.exe) toooperationalize R-paket i Machine Learning.
3. Skapa en settings.json fil under en katalog med namnet ```.azureml``` under arbetskatalogen och ange hello parametrar från Azure Machine Learning-arbetsytan:

Settings.JSON filstruktur:

    {"workspace":{
    "id"                  : "ENTER YOUR AZUREML WORKSPACE ID",
    "authorization_token" : "ENTER YOUR AZUREML AUTH TOKEN"
    }}


#### <a name="build-a-model-in-r-and-publish-it-in-azure-machine-learning"></a>Skapa en modell i R och publicera den i Azure Machine Learning
    library(AzureML)
    ws <- workspace(config="~/.azureml/settings.json")

    if(!require("lme4")) install.packages("lme4")
    library(lme4)
    set.seed(1)
    train <- sleepstudy[sample(nrow(sleepstudy), 120),]
    m <- lm(Reaction ~ Days + Subject, data = train)

    # Define a prediction function toopublish based on hello model:
    sleepyPredict <- function(newdata){
          predict(m, newdata=newdata)
    }

    ep <- publishWebService(ws, fun = sleepyPredict, name="sleepy lm", inputSchema = sleepstudy, data.frame=TRUE)

#### <a name="consume-hello-model-deployed-in-azure-machine-learning"></a>Använda hello modellen har distribuerats i Azure Machine Learning
tooconsume hello modellen från ett klientprogram, använder vi hello Azure Machine Learning-biblioteket toolook in hello webbtjänst har publicerats av namn med hjälp av hello `services` API-anrop toodetermine hello-slutpunkt. Du bara anropa hello `consume` fungerar och skicka in hello data ram toobe förutsade.
hello följande kod är används tooconsume hello modell publiceras som en Azure Machine Learning-webbtjänst.

    library(AzureML)
    library(lme4)
    ws <- workspace(config="~/.azureml/settings.json")

    s <-  services(ws, name = "sleepy lm")
    s <- tail(s, 1) # use hello last published function, in case of duplicate function names

    ep <- endpoints(ws, s)

    # OK, try this out, and compare with raw data
    ans = consume(ep, sleepstudy)$ans

Mer information om hello Azure Machine Learning R-biblioteket finns [här](https://cran.r-project.org/web/packages/AzureML/AzureML.pdf).

## <a name="4-administer-your-azure-resources-using-azure-portal-or-powershell"></a>4. Administrera Azure-resurser med Azure-portalen eller Powershell
Hej DSVM kan du inte bara toobuild analytics-lösningen lokalt på hello virtuell dator, men du kan också tooaccess tjänster på Microsoft Azure-molnet. Azure tillhandahåller flera bearbetning, lagring, data Analystjänster och andra tjänster som du kan administrera och åtkomst till från din DSVM.

tooadminister Azure-prenumeration och moln-resurser kan du använda din webbläsare och punkt toothe [Azure-portalen](https://portal.azure.com). Du kan också använda Azure Powershell tooadminister dina Azure-prenumeration och resurser via ett skript.
Du kan köra Azure Powershell från en genväg på skrivbordet hello eller från hello start-menyn med rubriken ”Microsoft Azure Powershell”. Referera till [dokumentation för Microsoft Azure Powershell](../powershell-azure-resource-manager.md) mer information om hur du kan administrera din Azure-prenumeration och resurser med hjälp av Windows Powershell-skript.

## <a name="5-extend-your-storage-space-with-a-shared-file-system"></a>5. Utöka ditt lagringsutrymme med ett delat filsystem
Datavetare kan dela stora datamängder, kod eller andra resurser i hello team. Hej DSVM själva har 70GB diskutrymme. tooextend ditt lagringsutrymme som du kan använda hello Azure File Service och montera på hello DSVM eller åtkomst till den via ett REST-API.   

> [!NOTE]
> hello Maximalt utrymme för hello Azure File Service resursen är 5TB storleksgränsen för enskilda filer är 1TB.   
> 
> 

Du kan använda Azure Powershell toocreate en Azures Filtjänst resurs. Här är hello skriptet toorun under Azure PowerShell toocreate tjänsten en Azure-filresurs.

    # Authenticate tooAzure.
    Login-AzureRmAccount
    # Select your subscription
    Get-AzureRmSubscription –SubscriptionName "<your subscription name>" | Select-AzureRmSubscription
    # Create a new resource group.
    New-AzureRmResourceGroup -Name <dsvmdatarg>
    # Create a new storage account. You can reuse existing storage account if you wish.
    New-AzureRmStorageAccount -Name <mydatadisk> -ResourceGroupName <dsvmdatarg> -Location "<Azure Data Center Name For eg. South Central US>" -Type "Standard_LRS"
    # Set your current working storage account
    Set-AzureRmCurrentStorageAccount –ResourceGroupName "<dsvmdatarg>" –StorageAccountName <mydatadisk>

    # Create a Azure File Service Share
    $s = New-AzureStorageShare <<teamsharename>>
    # Create a directory under hello FIle share. You can give it any name
    New-AzureStorageDirectory -Share $s -Path <directory name>
    # List hello share tooconfirm that everything worked
    Get-AzureStorageFile -Share $s


Nu när du har skapat en Azure-filresurs kan montera du den i en virtuell dator i Azure. Du rekommenderas att hello VM är i samma Azure-datacenter som hello lagringssvarstiden konto tooavoid och data transfer avgifter. Här följer hello kommandon toomount hello enhet på hello DSVM som du kan köra på Azure Powershell.

    # Get storage key of hello storage account that has hello Azure file share from Azure portal. Store it securely on hello VM tooavoid prompted in next command.
    cmdkey /add:<<mydatadisk>>.file.core.windows.net /user:<<mydatadisk>> /pass:<storage key>

    # Mount hello Azure file share as Z: drive on hello VM. You can chose another drive letter if you wish
    net use z:  \\<mydatadisk>.file.core.windows.net\<<teamsharename>>


Du kan nu komma åt enheten precis som med alla vanliga enheter på hello VM.

## <a name="6-share-code-with-your-team-using-github"></a>6. Dela kod med din grupp med GitHub
GitHub är en kod lagringsplats där du kan hitta mycket exempelkod och datakällor för olika verktyg med olika teknologier som delas av hello developer community. Den använder Git hello teknik tootrack och lagra versioner av hello kodfiler. GitHub är också en plattform där du kan skapa egna databasen toostore gruppens delade koden och dokumentation, implementera versionskontroll och även kontroll som har åtkomst till tooview och bidra med kod. Besök hello [GitHub hjälpsidor](https://help.github.com/) för mer information om hur du använder Git. Du kan använda GitHub som ett hello sätt toocollaborate med din grupp, använda kod som har utvecklats av hello community och bidra kod tillbaka toohello community.

Hej DSVM levereras inlästa med klientverktyg på både kommandoradsverktyg som korrekt GUI tooaccess GitHub-lagringsplatsen. hello kommandoradsverktyget toowork med Git och GitHub kallas Git Bash. Visual Studio installerat på hello DSVM har hello Git tillägg. Du kan hitta uppstart ikoner för de här verktygen på hello start-menyn och skrivbordet hello.

toodownload kod från en GitHub-databas som du använder hello ```git clone``` kommando. Till exempel toodownload datavetenskap databasen som publicerats av Microsoft i hello aktuell katalog du kan köra hello följande kommando när du är i ```git-bash```.

    git clone https://github.com/Azure/Azure-MachineLearning-DataScience.git

I Visual Studio kan du göra hello samma åtgärd för kloning. hello följande skärmbild som visar hur tooaccess Git och GitHub verktyg i Visual Studio.

![Git i Visual Studio](./media/machine-learning-data-science-vm-do-ten-things/VSGit.PNG)

Du hittar mer information om hur du använder Git toowork med GitHub-lagringsplats från flera resurser som är tillgängliga på github.com. Hej [fusklapp](https://training.github.com/kit/downloads/github-git-cheat-sheet.pdf) är en bra referens.

## <a name="7-access-various-azure-data-and-analytics-services"></a>7. Komma åt olika Azure data och analytics-tjänster
### <a name="azure-blob"></a>Azure-blobb
Azure blob är en tillförlitlig, ekonomiska molnlagring för data stora och små. Låt oss titta på hur du kan flytta data tooAzure Blob och åtkomst till data som lagras i Azure-Blob.

**Krav**

* **Skapa Azure Blob storage-konto från [Azure-portalen](https://portal.azure.com).**

![Create_Azure_Blob](./media/machine-learning-data-science-vm-do-ten-things/Create_Azure_Blob.PNG)

* Bekräfta att hello förinstallerade AzCopy kommandoradsverktyget påträffades vid ```C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy.exe```. Du kan lägga till hello som innehåller hello azcopy.exe tooyour sökväg miljö variabeln tooavoid skriva hello fullständiga kommandot katalogsökväg när du kör det här verktyget. Mer information om verktyget AzCopy finns för[AzCopy-dokumentationen](../storage/common/storage-use-azcopy.md)
* Starta hello Azure Lagringsutforskaren-verktyget. Du kan hämta från [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com/). 

![AzureStorageExplorer_v4](./media/machine-learning-data-science-vm-do-ten-things/AzureStorageExplorer_v4.png)

**Flytta data från VM tooAzure Blob: AzCopy**

toomove data mellan din lokala filer och blob-lagring, kan du använda AzCopy i kommandoraden eller PowerShell:

    AzCopy /Source:C:\myfolder /Dest:https://<mystorageaccount>.blob.core.windows.net/<mycontainer> /DestKey:<storage account key> /Pattern:abc.txt

Ersätt **C:\myfolder** toohello sökväg där filen lagras **mittlagringskonto** tooyour blob storage kontonamn **minbehållare** toohello behållarnamn **lagringskontonyckel** tooyour blob lagringsåtkomstnyckel. Du kan hitta autentiseringsuppgifterna för ditt lagringskonto i [Azure-portalen](https://portal.azure.com).

![StorageAccountCredential_v2](./media/machine-learning-data-science-vm-do-ten-things/StorageAccountCredential_v2.png)

Kör AzCopy-kommandot i PowerShell eller från en kommandotolk. Här är några exempel på användning av AzCopy-kommandot:

    # Copy *.sql from local machine tooa Azure Blob
    "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:"c:\Aaqs\Data Science Scripts" /Dest:https://[ENTER STORAGE ACCOUNT].blob.core.windows.net/[ENTER CONTAINER] /DestKey:[ENTER STORAGE KEY] /S /Pattern:*.sql

    # Copy back all files from Azure Blob container tooLocal machine

    "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Dest:"c:\Aaqs\Data Science Scripts\temp" /Source:https://[ENTER STORAGE ACCOUNT].blob.core.windows.net/[ENTER CONTAINER] /SourceKey:[ENTER STORAGE KEY] /S



När du kör visas din AzCopy-kommandot toocopy tooan Azure blob som finns i filen i Azure Lagringsutforskaren inom kort.

![AzCopy_run_finshed_Storage_Explorer_v3](./media/machine-learning-data-science-vm-do-ten-things/AzCopy_run_finshed_Storage_Explorer_v3.png)

**Flytta data från VM tooAzure Blob: Azure Lagringsutforskaren**

Du kan också ladda upp data från en lokal hello-fil i den virtuella datorn med Azure Lagringsutforskaren:

* tooupload data tooa behållare, Välj hello mål-behållaren och klicka på hello **överför** knappen.![ Ladda upp i Lagringsutforskaren](./media/machine-learning-data-science-vm-do-ten-things/storage-accounts.png)
* Klicka på hello **...**  toohello höger i hello **filer** , Välj en eller flera filer tooupload hello filsystemet och klicka **överför** toobegin Överför filer hello.![ Ladda upp filer tooblob](./media/machine-learning-data-science-vm-do-ten-things/upload-files-to-blob.png)

**Läsa data från Azure Blob: modul för dataläsare för Machine Learning**

I Azure Machine Learning Studio kan du använda en **importera Data modulen** tooread data från din blob.

![AML_ReaderBlob_Module_v3](./media/machine-learning-data-science-vm-do-ten-things/AML_ReaderBlob_Module_v3.png)

**Läsa data från Azure Blob: Python ODBC**

Du kan använda **BlobService** biblioteket tooread data direkt från blob i ett Jupyter-anteckningsbok eller Python.

Först importera nödvändiga paket:

    import pandas as pd
    from pandas import Series, DataFrame
    import numpy as np
    import matplotlib.pyplot as plt
    from time import time
    import pyodbc
    import os
    from azure.storage.blob import BlobService
    import tables
    import time
    import zipfile
    import random

Sedan ansluter dina Azure Blob-autentiseringsuppgifter och läsa data från Blob:

    CONTAINERNAME = 'xxx'
    STORAGEACCOUNTNAME = 'xxxx'
    STORAGEACCOUNTKEY = 'xxxxxxxxxxxxxxxx'
    BLOBNAME = 'nyctaxidataset/nyctaxitrip/trip_data_1.csv'
    localfilename = 'trip_data_1.csv'
    LOCALDIRECTORY = os.getcwd()
    LOCALFILE =  os.path.join(LOCALDIRECTORY, localfilename)

    #download from blob
    t1 = time.time()
    blob_service = BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
    blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILE)
    t2 = time.time()
    print(("It takes %s seconds toodownload "+BLOBNAME) % (t2 - t1))

    #unzipping downloaded files if needed
    #with zipfile.ZipFile(ZIPPEDLOCALFILE, "r") as z:
    #    z.extractall(LOCALDIRECTORY)

    df1 = pd.read_csv(LOCALFILE, header=0)
    df1.columns = ['medallion','hack_license','vendor_id','rate_code','store_and_fwd_flag','pickup_datetime','dropoff_datetime','passenger_count','trip_time_in_secs','trip_distance','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude']
    print 'hello size of hello data is: %d rows and  %d columns' % df1.shape

hello data läses i som en data-ram:

![IPNB_data_readin](./media/machine-learning-data-science-vm-do-ten-things/IPNB_data_readin.PNG)

### <a name="azure-data-lake"></a>Azure Data Lake
Azure Data Lake Storage är en storskalig lagringsplats för analytics stordataarbetsbelastningar och kompatibla med Hadoop Distributed File System (HDFS). Den fungerar med både hello Hadoop-ekosystemet och hello Azure Data Lake Analytics. Visar vi hur du kan flytta data till hello Azure Data Lake Store och kör analytics med hjälp av Azure Data Lake Analytics.

**Krav**

* Skapa Azure Data Lake Analytics i [Azure-portalen](https://portal.azure.com).

![Azure_Data_Lake_Create_v2](./media/machine-learning-data-science-vm-do-ten-things/Azure_Data_Lake_Create_v2.png)

* Hej **Azure Data Lake-verktyg** i **Visual Studio** hittades på den här [länk](https://www.microsoft.com/download/details.aspx?id=49504) redan är installerad på hello Visual Studio Community Edition som finns på hello virtuell dator. När du startar Visual Studio och loggning i din Azure-prenumeration kan bör du se din Azure Data Analytics-konto och lagringsutrymme i hello vänstra panelen av Visual Studio.

![Azure_Data_Lake_PlugIn_v2](./media/machine-learning-data-science-vm-do-ten-things/Azure_Data_Lake_PlugIn_v2.PNG)

**Flytta data från VM tooData Lake: Azure Data Lake Explorer**

Du kan använda **Azure Data Lake Explorer** tooupload data från hello lokala filer i virtuella tooData Lake lagring.

![Azure_Data_Lake_UploadData](./media/machine-learning-data-science-vm-do-ten-things/Azure_Data_Lake_UploadData.PNG)

Du kan också skapa ett data pipeline tooproductionize din tooor för flytt av data från Azure Data Lake med hello [Azure Data Factory(ADF)](https://azure.microsoft.com/services/data-factory/). Vi refererar toothis [artikel](https://azure.microsoft.com/blog/creating-big-data-pipelines-using-azure-data-lake-and-azure-data-factory/) tooguide du via hello steg toobuild hello data rörledningar.

**Läsa data från Azure Blob tooData Lake: U-SQL**

Om dina data finns i Azure Blob storage, kan du direkt läsa data från Azure storage blob i U-SQL-frågan. Kontrollera att blob storage-konto är länkade tooyour Azure Data Lake innan du skriver U-SQL-fråga. Gå för**Azure-portalen**, hitta din Azure Data Lake Analytics-instrumentpanelen, klicka på **Lägg till datakälla**, Välj lagringstyp för**Azure Storage** och Anslut i Azure Storage Kontonamnet och nyckeln. Är du kan tooreference hello data som lagras i hello storage-konto.

![Ange storage-konto och nyckel](./media/machine-learning-data-science-vm-do-ten-things/Link_Blob_to_ADLA_v2.PNG)

I Visual Studio kan du läsa data från blob storage, göra vissa datamanipulering, funktionen tekniker och utdata hello resulterande data tooeither Azure Data Lake eller Azure Blob Storage. När du refererar hello data i blob storage använda **wasb: / /**; när du refererar till hello data i Azure Data Lake, Använd **swbhdfs: / /**

![Data-ram](./media/machine-learning-data-science-vm-do-ten-things/USQL_Read_Blob_v2.PNG)

Du kan använda följande U-SQL-frågor i Visual Studio hello:

    @a =
        EXTRACT medallion string,
                hack_license string,
                vendor_id string,
                rate_code string,
                store_and_fwd_flag string,
                pickup_datetime string,
                dropoff_datetime string,
                passenger_count int,
                trip_time_in_secs double,
                trip_distance double,
                pickup_longitude string,
                pickup_latitude string,
                dropoff_longitude string,
                dropoff_latitude string

        FROM "wasb://<Container name>@<Azure Blob Storage Account Name>.blob.core.windows.net/<Input Data File Name>"
        USING Extractors.Csv();

    @b =
        SELECT vendor_id,
        COUNT(medallion) AS cnt_medallion,
        SUM(passenger_count) AS cnt_passenger,
        AVG(trip_distance) AS avg_trip_dist,
        MIN(trip_distance) AS min_trip_dist,
        MAX(trip_distance) AS max_trip_dist,
        AVG(trip_time_in_secs) AS avg_trip_time
        FROM @a
        GROUP BY vendor_id;

    OUTPUT @b   
    too"swebhdfs://<Azure Data Lake Storage Account Name>.azuredatalakestore.net/<Folder Name>/<Output Data File Name>"
    USING Outputters.Csv();

    OUTPUT @b   
    too"wasb://<Container name>@<Azure Blob Storage Account Name>.blob.core.windows.net/<Output Data File Name>"
    USING Outputters.Csv();



När frågan har skickats toohello server, visas ett diagram som visar hello status för jobbet.

![Jobbet status diagram](./media/machine-learning-data-science-vm-do-ten-things/USQL_Job_Status.PNG)

**Fråga efter data i Data Lake: U-SQL**

När hello dataset inhämtas i Azure Data Lake, kan du använda [U-SQL-språket](../data-lake-analytics/data-lake-analytics-u-sql-get-started.md) tooquery och utforska hello data. U-SQL-språket är liknande Tool-SQL, men kombinerar vissa funktioner från C# så att användarna kan skriva anpassade moduler, användardefinierade funktioner och osv. Du kan använda hello skript i hello föregående steg.

Efter hello frågan har skickats tooserver, tripdata_summary. CSV kan hittas inom kort i **Azure Data Lake Explorer**, du kan förhandsgranska hello data genom att högerklicka på hello-filen.

![Filen i Azure Data Lake Explorer](./media/machine-learning-data-science-vm-do-ten-things/USQL_create_summary.png)

information om toosee hello:

![Filen sammanfattning](./media/machine-learning-data-science-vm-do-ten-things/USQL_tripdata_summary.png)

### <a name="hdinsight-hadoop-clusters"></a>HDInsight Hadoop-kluster
Azure HDInsight är en hanterad tjänst för Apache Hadoop, Spark, HBase eller Storm på hello moln. Du kan arbeta enkelt med Azure HDInsight-kluster från hello datavetenskap virtuell dator.

**Krav**

* Skapa Azure Blob storage-konto från [Azure-portalen](https://portal.azure.com). Det här lagringskontot är används toostore data för HDInsight-kluster.

![Skapa Azure Blob storage-konto](./media/machine-learning-data-science-vm-do-ten-things/Create_Azure_Blob.PNG)

* Anpassa Azure HDInsight Hadoop-kluster från [Azure-portalen](machine-learning-data-science-customize-hadoop-cluster.md)
  
  * Du måste länka hello-lagringskonto som skapats med ditt HDInsight-kluster när den skapas. Det här lagringskontot används för att komma åt data som kan bearbetas i hello kluster.

![Länken toostorage konto som har skapats med HDInsight-kluster](./media/machine-learning-data-science-vm-do-ten-things/Create_HDI_v4.PNG)

* Du måste aktivera **fjärråtkomst** toohello huvudnod hello klustret när den har skapats. Kom ihåg hello fjärråtkomst autentiseringsuppgifter du anger här (skiljer sig från de som anges för hello klustret när skapades): du behöver dem i hello efterföljande proceduren.

![Aktivera fjärråtkomst](./media/machine-learning-data-science-vm-do-ten-things/Create_HDI_dashboard_v3.PNG)

* Skapa en Azure Machine Learning-arbetsytan. Din Maskininlärningsexperiment lagras i Machine Learning-arbetsytan. Välj hello markerat alternativ i portalen som visas i följande skärmbild hello:

![Skapa en Azure Machine Learning-arbetsyta](./media/machine-learning-data-science-vm-do-ten-things/Create_ML_Space.PNG)

* Ange hello parametrar för din arbetsyta

![Ange parametrar för Machine Learning-arbetsytan](./media/machine-learning-data-science-vm-do-ten-things/Create_ML_Space_step2_v2.PNG)

* Överföra data med hjälp av IPython bärbar dator. Först importera nödvändiga paket, plugin-autentiseringsuppgifter, skapa en db i ditt lagringskonto och läsa in data tooHDI kluster.

        #Import required Packages
        import pyodbc
        import time as time
        import json
        import os
        import urllib
        import urllib2
        import warnings
        import re
        import pandas as pd
        import matplotlib.pyplot as plt
        from azure.storage.blob import BlobService
        warnings.filterwarnings("ignore", category=UserWarning, module='urllib2')


        #Create hello connection tooHive using ODBC
        SERVER_NAME='xxx.azurehdinsight.net'
        DATABASE_NAME='nyctaxidb'
        USERID='xxx'
        PASSWORD='xxxx'
        DB_DRIVER='Microsoft Hive ODBC Driver'
        driver = 'DRIVER={' + DB_DRIVER + '}'
        server = 'Host=' + SERVER_NAME + ';Port=443'
        database = 'Schema=' + DATABASE_NAME
        hiveserv = 'HiveServerType=2'
        auth = 'AuthMech=6'
        uid = 'UID=' + USERID
        pwd = 'PWD=' + PASSWORD
        CONNECTION_STRING = ';'.join([driver,server,database,hiveserv,auth,uid,pwd])
        connection = pyodbc.connect(CONNECTION_STRING, autocommit=True)
        cursor=connection.cursor()


        #Create Hive database and tables
        queryString = "create database if not exists nyctaxidb;"
        cursor.execute(queryString)

        queryString = """
                        create external table if not exists nyctaxidb.trip
                        (
                            medallion string,
                            hack_license string,
                            vendor_id string,
                            rate_code string,
                            store_and_fwd_flag string,
                            pickup_datetime string,
                            dropoff_datetime string,
                            passenger_count int,
                            trip_time_in_secs double,
                            trip_distance double,
                            pickup_longitude double,
                            pickup_latitude double,
                            dropoff_longitude double,
                            dropoff_latitude double)  
                        PARTITIONED BY (month int)
                        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\\n'
                        STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/trip' TBLPROPERTIES('skip.header.line.count'='1');
                    """
        cursor.execute(queryString)

        queryString = """
                        create external table if not exists nyctaxidb.fare
                        (
                            medallion string,
                            hack_license string,
                            vendor_id string,
                            pickup_datetime string,
                            payment_type string,
                            fare_amount double,
                            surcharge double,
                            mta_tax double,
                            tip_amount double,
                            tolls_amount double,
                            total_amount double)
                        PARTITIONED BY (month int)
                        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\\n'
                        STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/fare' TBLPROPERTIES('skip.header.line.count'='1');
                    """
        cursor.execute(queryString)


        #Upload data from blob storage tooHDI cluster
        for i in range(1,13):
            queryString = "LOAD DATA INPATH 'wasb:///nyctaxitripraw2/trip_data_%d.csv' INTO TABLE nyctaxidb2.trip PARTITION (month=%d);"%(i,i)
            cursor.execute(queryString)
            queryString = "LOAD DATA INPATH 'wasb:///nyctaxifareraw2/trip_fare_%d.csv' INTO TABLE nyctaxidb2.fare PARTITION (month=%d);"%(i,i)  
            cursor.execute(queryString)


* Alternativt kan du följa detta [genomgången](machine-learning-data-science-process-hive-walkthrough.md) tooupload NYC Taxi data tooHDI klustret. Större stegen innefattar:
  
  * AzCopy: hämta komprimerade CSV från offentliga blob tooyour lokal mapp
  * AzCopy: Överför uppackade CSV från lokal mapp tooHDI kluster
  * Logga in på hello huvudnod i Hadoop-kluster och förbereda för undersökande dataanalys

När hello data har lästs in tooHDI klustret måste kontrollera du dina data i Azure Lagringsutforskaren. Och du har en databas nyctaxidb som skapats i HDI-klustret.

**Datagranskning: Hive-frågor i Python**

Eftersom hello data i Hadoop-kluster kan du använda hello pyodbc paketet tooconnect tooHadoop kluster och fråga databas med hjälp av Hive toodo undersökning och funktionen tekniker. Du kan visa befintliga hello tabeller som vi skapade i hello nödvändiga steg.

    queryString = """
        show tables in nyctaxidb2;
        """
    pd.read_sql(queryString,connection)


![Visa befintliga tabeller](./media/machine-learning-data-science-vm-do-ten-things/Python_View_Existing_Tables_Hive_v3.PNG)

Nu ska vi titta på hello antalet poster i varje månad och hello frekvenser på lutad eller inte i hello resa tabell:

    queryString = """
        select month, count(*) from nyctaxidb.trip group by month;
        """
    results = pd.read_sql(queryString,connection)

    %matplotlib inline

    results.columns = ['month', 'trip_count']
    df = results.copy()
    df.index = df['month']
    df['trip_count'].plot(kind='bar')


![Område för antalet poster i varje månad](./media/machine-learning-data-science-vm-do-ten-things/Exploration_Number_Records_by_Month_v3.PNG)

    queryString = """
        SELECT tipped, COUNT(*) AS tip_freq
        FROM
        (
            SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
            FROM nyctaxidb.fare
        )tc
        GROUP BY tipped;
        """
    results = pd.read_sql(queryString,connection)

    results.columns = ['tipped', 'trip_count']
    df = results.copy()
    df.index = df['tipped']
    df['trip_count'].plot(kind='bar')


![Ritytans tips frekvenser](./media/machine-learning-data-science-vm-do-ten-things/Exploration_Frequency_tip_or_not_v3.PNG)

Vi kan även beräkna hello avståndet mellan där den ska hämtas och dropoff plats och sedan jämföra toohello utlösas avstånd.

    queryString = """
                    select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, trip_distance, trip_time_in_secs,
                        3959*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
                        *radians(180)/180/2),2)-cos(pickup_latitude*radians(180)/180)
                        *cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2)))
                        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*radians(180)/180/2),2)
                        +cos(pickup_latitude*radians(180)/180)*cos(dropoff_latitude*radians(180)/180)*
                        pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2))) as direct_distance
                        from nyctaxidb.trip
                        where month=1
                            and pickup_longitude between -90 and -30
                            and pickup_latitude between 30 and 90
                            and dropoff_longitude between -90 and -30
                            and dropoff_latitude between 30 and 90;
                """
    results = pd.read_sql(queryString,connection)
    results.head(5)


![Hämtning och dropoff tabell](./media/machine-learning-data-science-vm-do-ten-things/Exploration_compute_pickup_dropoff_distance_v2.PNG)

    results.columns = ['pickup_longitude', 'pickup_latitude', 'dropoff_longitude',
                       'dropoff_latitude', 'trip_distance', 'trip_time_in_secs', 'direct_distance']
    df = results.loc[results['trip_distance']<=100] #remove outliers
    df = df.loc[df['direct_distance']<=100] #remove outliers
    plt.scatter(df['direct_distance'], df['trip_distance'])


![Område för hämtning/dropoff avstånd tootrip avstånd](./media/machine-learning-data-science-vm-do-ten-things/Exploration_direct_distance_trip_distance_v2.PNG)

Nu ska vi förbereder en ned provtagning (% 1) uppsättning data för modellering. Vi kan använda dessa data i modul för dataläsare för Machine Learning.

        queryString = """
        create  table if not exists nyctaxi_downsampled_dataset_testNEW (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        pickup_hour string,
        pickup_week string,
        weekday string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double,
        direct_distance double,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double,
        tipped string,
        tip_class string
        )
        row format delimited fields terminated by ','
        lines terminated by '\\n'
        stored as textfile;
        """
        cursor.execute(queryString)

        --- now insert contents of hello join into hello preceding internal table

        queryString = """
        insert overwrite table nyctaxi_downsampled_dataset_testNEW
        select
        t.medallion,
        t.hack_license,
        t.vendor_id,
        t.rate_code,
        t.store_and_fwd_flag,
        t.pickup_datetime,
        t.dropoff_datetime,
        hour(t.pickup_datetime) as pickup_hour,
        weekofyear(t.pickup_datetime) as pickup_week,
        from_unixtime(unix_timestamp(t.pickup_datetime, 'yyyy-MM-dd HH:mm:ss'),'u') as weekday,
        t.passenger_count,
        t.trip_time_in_secs,
        t.trip_distance,
        t.pickup_longitude,
        t.pickup_latitude,
        t.dropoff_longitude,
        t.dropoff_latitude,
        t.direct_distance,
        f.payment_type,
        f.fare_amount,
        f.surcharge,
        f.mta_tax,
        f.tip_amount,
        f.tolls_amount,
        f.total_amount,
        if(tip_amount>0,1,0) as tipped,
        if(tip_amount=0,0,
        if(tip_amount>0 and tip_amount<=5,1,
        if(tip_amount>5 and tip_amount<=10,2,
        if(tip_amount>10 and tip_amount<=20,3,4)))) as tip_class
        from
        (
        select
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        pickup_datetime,
        dropoff_datetime,
        passenger_count,
        trip_time_in_secs,
        trip_distance,
        pickup_longitude,
        pickup_latitude,
        dropoff_longitude,
        dropoff_latitude,
        3959*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
        radians(180)/180/2),2)-cos(pickup_latitude*radians(180)/180)
        *cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2)))
        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*radians(180)/180/2),2)
        +cos(pickup_latitude*radians(180)/180)*cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2))) as direct_distance,
        rand() as sample_key

        from trip
        where pickup_latitude between 30 and 90
            and pickup_longitude between -90 and -30
            and dropoff_latitude between 30 and 90
            and dropoff_longitude between -90 and -30
        )t
        join
        (
        select
        medallion,
        hack_license,
        vendor_id,
        pickup_datetime,
        payment_type,
        fare_amount,
        surcharge,
        mta_tax,
        tip_amount,
        tolls_amount,
        total_amount
        from fare
        )f
        on t.medallion=f.medallion and t.hack_license=f.hack_license and t.pickup_datetime=f.pickup_datetime
        where t.sample_key<=0.01
        """
        cursor.execute(queryString)

Du kan se hello data har lästs in i Hadoop-kluster efter ett tag:

    queryString = """
        select * from nyctaxi_downsampled_dataset limit 10;
        """
    cursor.execute(queryString)
    pd.read_sql(queryString,connection)


![Tabell med data](./media/machine-learning-data-science-vm-do-ten-things/DownSample_Data_For_Modeling_v2.PNG)

**Läsa data från HDI med Machine Learning: modul för dataläsare**

Du kan också använda hello **reader** modul i Machine Learning Studio tooaccess hello databasen i Hadoop-kluster. Anslut hello autentiseringsuppgifter HDI-kluster och Azure Storage-konto tooenable skapa ing maskininlärning modeller som använder databasen i HDI-kluster.

![Läsaren modulen egenskaper](./media/machine-learning-data-science-vm-do-ten-things/AML_Reader_Hive.PNG)

Hej kan poängsatta dataset sedan visas:

![Visa bedömas dataset](./media/machine-learning-data-science-vm-do-ten-things/AML_Model_Results.PNG)

### <a name="azure-sql-data-warehouse--databases"></a>Azure SQL Data Warehouse och databaser
Azure SQL Data Warehouse är ett datalager för elastiska som en tjänst med företagsklass SQL Server-miljön.

Du kan etablera din Azure SQL Data Warehouse genom att följa hello anvisningarna i det här [artikel](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md). När du distribuera din Azure SQL Data Warehouse, kan du använda den [genomgången](machine-learning-data-science-process-sqldw-walkthrough.md) toodo data överför undersökning och modellering med data i hello SQL Data Warehouse.

#### <a name="azure-cosmos-db"></a>Azure Cosmos DB
Azure Cosmos-DB är en NoSQL-databas i hello molnet. Det kan du toowork med som JSON-dokument och toostore och fråga hello-dokument.

Du måste toodo hello följande per kraven steg tooaccess Azure Cosmos DB från hello DSVM.

1. Installera DocumentDB Python SDK (kör ```pip install pydocumentdb``` från kommandoraden)
2. Skapa ett Azure DB som Cosmos-konto och en databas från [Azure-portalen](https://portal.azure.com)
3. Ladda ned ”Azure Cosmos DB Migreringsverktyget” från [här](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d) och extrahera tooa directory önskat
4. Importera JSON-data (vulkanen data) lagras på en [offentlig blob](https://cahandson.blob.core.windows.net/samples/volcano.json) i Cosmos DB med följande kommando parametrar toohello Migreringsverktyget (dtui.exe från hello katalog där du installerade hello Cosmos DB Migration Tool). Ange hello käll- och plats med följande parametrar:
   
    /s:JsonFile /s.Files:https://cahandson.blob.core.windows.net/samples/volcano.json /t:DocumentDBBulk /t.ConnectionString:AccountEndpoint=https://[DocDBAccountName].documents.azure.com:443/; AccountKey = [[nyckel]; Database = vulkanen /t.Collection:volcano1

När du importerar hello data går du tooJupyter och öppna hello anteckningsboken med titeln *DocumentDBSample* som innehåller python code tooaccess DocumentDB och utföra vissa grundläggande frågor. Du kan läsa mer om Cosmos DB genom att besöka hello service [dokumentationssidan](https://docs.microsoft.com/azure/cosmos-db/).

## <a name="8-build-reports-and-dashboard-using-hello-power-bi-desktop"></a>8. Skapa rapporter och instrumentpanel med hjälp av hello Power BI Desktop
Låt oss visualisera hello vulkanen JSON-fil som vi såg i hello före Cosmos-DB-exempel i Power BI toogain visual insikter om hello data. Detaljerade anvisningar finns i hello [Power BI-artikel](../cosmos-db/powerbi-visualize.md). Här följer hello anvisningar:

1. Öppna Power BI Desktop och gör ”hämta Data”. Ange hello URL som: https://cahandson.blob.core.windows.net/samples/volcano.json
2. Du bör se hello JSON-poster som importeras som en lista
3. Konvertera hello listan tooa tabell så att Power BI kan arbeta med hello samma
4. Expandera hello kolumner genom att klicka på hello Expandera ikonen (hello något med hello ”vänsterpilen och en högerpil” ikon på hello höger i hello kolumn)
5. Observera att platsen är ett ”post”-fält. Visa hello-post och välj endast hello koordinater. Koordinaten är en kolumn
6. Lägg till en ny kolumn tooconvert hello koordinaten kolumn i en kommatecken separat LatLong kolumn sammanbinda hello två element i hello koordinaten listfält med hello formeln ```Text.From([coordinates]{1})&","&Text.From([coordinates]{0})```.
7. Slutligen konvertera hello ```Elevation``` kolumnen tooDecimal och välj hello **Stäng** och **tillämpa**.

I stället för föregående steg, kan du klistra in hello följande kod som skript ut hello steg som används i hello avancerade redigeraren i Power BI som tillåter Datatransformationer för toowrite hello i ett frågespråk.

    let
        Source = Json.Document(Web.Contents("https://cahandson.blob.core.windows.net/samples/volcano.json")),
        #"Converted tooTable" = Table.FromList(Source, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
        #"Expanded Column1" = Table.ExpandRecordColumn(#"Converted tooTable", "Column1", {"Volcano Name", "Country", "Region", "Location", "Elevation", "Type", "Status", "Last Known Eruption", "id"}, {"Volcano Name", "Country", "Region", "Location", "Elevation", "Type", "Status", "Last Known Eruption", "id"}),
        #"Expanded Location" = Table.ExpandRecordColumn(#"Expanded Column1", "Location", {"coordinates"}, {"coordinates"}),
        #"Added Custom" = Table.AddColumn(#"Expanded Location", "LatLong", each Text.From([coordinates]{1})&","&Text.From([coordinates]{0})),
        #"Changed Type" = Table.TransformColumnTypes(#"Added Custom",{{"Elevation", type number}})
    in
        #"Changed Type"



Nu har du hello data i Power BI datamodellen. Power BI desktop ska visas på följande sätt:

![Power BI desktop](./media/machine-learning-data-science-vm-do-ten-things/PowerBIVolcanoData.png)

Du kan börja skapa rapporter och visualiseringar med hello datamodellen. Du kan följa hello stegen i den här [Power BI-artikel](../cosmos-db/powerbi-visualize.md#build-the-reports) toobuild en rapport. hello slutresultatet är en rapport som ser ut som följande hello.

![Power BI Desktop rapportvyn - anslutningsprogrammet för Powerbi](./media/machine-learning-data-science-vm-do-ten-things/power_bi_connector_pbireportview2.png)

## <a name="9-dynamically-scale-your-dsvm-toomeet-your-project-needs"></a>9. Dynamiskt skala din DSVM toomeet måste ditt projekt
Du kan skala uppåt och nedåt hello DSVM toomeet projektet måste. Om du inte behöver toouse hello VM i hello kväll eller helger, kan du bara stänga av hello VM från hello [Azure-portalen](https://portal.azure.com).

> [!NOTE]
> Du betalar avgifter för beräkning om du använder bara hello operativsystemet Stäng på hello VM.  
> 
> 

Om du behöver toohandle vissa storskaliga analys och behöver mer CPU eller minne eller disk kapacitet hittar du ett stort urval av VM-storlekar vad gäller CPU-kärnor, minneskapacitet och disktyper (inklusive solid-state-hårddiskar) som uppfyller dina beräknings- och budgeten behov. hello fullständig lista över virtuella datorer tillsammans med deras timvis beräkning prisnivå är tillgängligt på hello [priser för Azure virtuella datorer](https://azure.microsoft.com/pricing/details/virtual-machines/) sidan.

På samma sätt om du minskar behovet av VM-bearbetningskapacitet (till exempel: du flyttat en större arbetsbelastning tooa Hadoop eller ett Spark-kluster), du kan skala ned hello kluster från hello [Azure-portalen](https://portal.azure.com) och toohello inställningarna för den virtuella datorn instans. Här är en skärmbild.

![Inställningar för VM-instans](./media/machine-learning-data-science-vm-do-ten-things/VMScaling.PNG)

## <a name="10-install-additional-tools-on-your-virtual-machine"></a>10. Installera ytterligare verktyg på den virtuella datorn
Vi har paketerat flera verktyg som vi tror är kan tooaddress många hello vanliga analytics databehov och som ska spara tid genom att undvika att ha tooinstall och konfigurera dina miljöer i taget och spara pengar genom att betala endast för resurser som du Använd.

Du kan använda andra Azure-data och Analystjänster profileras startas om den här artikeln tooenhance analytics-miljö. Vi förstår att dina behov i vissa fall kan kräva ytterligare verktyg, bland annat vissa egna verktyg från tredje part. Du har fullständig administrativ åtkomst på hello virtuella tooinstall nya verktyg du behöver. Du kan också installera ytterligare paket i Python och R som inte redan är installerad. För Python kan du använda antingen ```conda``` eller ```pip```. Du kan använda hello för R ```install.packages()``` i hello R-konsolen eller använda hello IDE och välj ”**paket** -> **paket installeras...** ".

## <a name="summary"></a>Sammanfattning
Det är några av hello saker du kan göra på hello Microsoft datavetenskap Virtual Machine. Det finns många saker du kan göra toomake den en effektiv analytics-miljö.

