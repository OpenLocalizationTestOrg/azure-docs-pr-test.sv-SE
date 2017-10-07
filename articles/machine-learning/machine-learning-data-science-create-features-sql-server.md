---
title: "aaaCreate funktioner för data i SQL Server med SQL och Python | Microsoft Docs"
description: "Bearbetning av Data från SQL Azure"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: 
ms.assetid: bf1f4a6c-7711-4456-beb7-35fdccd46a44
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev;fashah;garye
ms.openlocfilehash: 223352edb0377a159333e7528ad03c43902e6f45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-features-for-data-in-sql-server-using-sql-and-python"></a>Skapa funktioner för data i SQL Server med SQL och Python
Det här dokumentet beskrivs hur toogenerate funktioner för data som lagras i en SQL Server-VM på Azure som hjälper algoritmer lära sig mer effektivt från hello data. Detta kan göras med hjälp av SQL eller genom att använda ett programmeringsspråk som Python, som visas här.

[!INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

Detta **menyn** länkar tootopics som beskriver hur toocreate funktioner för data i olika miljöer. Den här uppgiften är ett steg i hello [Team Data vetenskap processen (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

> [!NOTE]
> Ett praktiskt exempel, finns hello [NYC Taxi dataset](http://www.andresmh.com/nyctaxitrips/) och finns toohello IPNB med titeln [NYC Data wrangling IPython anteckningsboken och SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) för en slutpunkt till slutpunkt-hanteringspaketen.
> 
> 

## <a name="prerequisites"></a>Krav
Den här artikeln förutsätter att du har:

* Skapa ett Azure storage-konto. Om du behöver mer information, se [skapa ett Azure Storage-konto](../storage/common/storage-create-storage-account.md#create-a-storage-account)
* Lagrade data i SQL Server. Om du inte har Se [flytta data tooan Azure SQL Database för Azure Machine Learning](machine-learning-data-science-move-sql-azure.md) anvisningar för hur toomove hello data där.

## <a name="sql-featuregen"></a>Funktionen Generation med SQL
I det här avsnittet beskrivs olika sätt att skapa funktioner med hjälp av SQL:  

1. [Antal baserat funktionen Generation](#sql-countfeature)
2. [Diskretisering funktionen Generation](#sql-binningfeature)
3. [Lansera hello funktioner från en enda kolumn](#sql-featurerollout)

> [!NOTE]
> När du skapar ytterligare funktioner, kan du lägga till dem som kolumner toohello befintlig tabell eller skapa en ny tabell med hello ytterligare funktioner och primärnyckel som kan förenas med hello ursprungliga tabellen.
> 
> 

### <a name="sql-countfeature"></a>Antal baserat funktionen Generation
Det här dokumentet visar två sätt att generera antal funktioner. hello första metoden använder Villkorlig summering och hello andra metoden använder hello uttrycket 'where'. De kan sedan kopplas med hello ursprungliga tabellen (med primärnyckelkolumnerna) toohave antal funktioner tillsammans med hello ursprungliga data.

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3>

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename>
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2>

### <a name="sql-binningfeature"></a>Diskretisering funktionen Generation
hello följande exempel visas hur toogenerate binned funktioner av diskretisering (med 5 lagerplatser) en numerisk kolumn som kan användas som en funktion i stället:

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


### <a name="sql-featurerollout"></a>Lansera hello funktioner från en enda kolumn
I det här avsnittet visar vi hur tooroll ut en kolumn i en tabell toogenerate ytterligare funktioner. hello exemplet förutsätter att det finns en latitud och longitud kolumn i hello tabell från vilken du försöker toogenerate funktioner.

Här är en kort introduktion om latitud/longitud platsdata (resurstilldelas från stackoverflow `http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude`). Detta är användbart toounderstand innan featurizing hello Platsfältet:

* hello logga talar om för oss om vi är norr eller Syd, Öst eller Väst på hello jordglob.
* Ett annat värde än noll hundratals siffra visar vi använder longitud inte latitud!
* hello flera siffror ger en position tooabout 1 000 kilometer. Det ger oss användbar information om vilka kontinent eller oceanen vi finns på.
* hello enheter siffra (en decimal grad) ger en position upp too111 kilometer (60 sjömil, om 69 miles). Det kan berätta ungefär vad eller det stora land vi finns i.
* hello första decimaltecknet är värda too11.1 km: den kan skilja hello positionen för en stor ort från en närliggande stora stad.
* hello andra decimaltecknet är värda too1.1 km: den kan skilja en village från hello nästa.
* hello tredje decimaltecknet är värda upp too110 m: den kan identifiera en stor jordbruket institutionella plats.
* hello fjärde decimaltecknet är värda upp too11 m: mark kan identifiera. Det är jämförbar toohello vanliga korrektheten i en okorrigerade GPS-enhet utan störningar.
* hello femte decimaltecknet är värda upp too1.1 m: den skilja träd från varandra. Precision toothis nivå med kommersiella GPS-enheter kan endast uppnås med differentiell korrigering.
* hello sjätte decimaltecknet är värda upp too0.11 m: kan du använda den för att utforma strukturer i detalj för att utforma landskap, skapa vägar. Det bör vara mer än tillräckligt bra för spårning av glaciers och floder. Detta kan uppnås genom att göra painstaking åtgärder med GPS, till exempel differentially korrigerade GPS.

hello platsinformation kan kan vara featurized på följande sätt att skilja ut region, plats och information om ort. Observera att en gång kan även anropa en REST-slutpunkt, till exempel Bing Maps API finns på `https://msdn.microsoft.com/library/ff701710.aspx` tooget hello region/distrikt information.

    select
        <location_columnname>
        ,round(<location_columnname>,0) as l1        
        ,l2=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 1 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),1,1) else '0' end     
        ,l3=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 2 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),2,1) else '0' end     
        ,l4=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 3 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),3,1) else '0' end     
        ,l5=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 4 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),4,1) else '0' end     
        ,l6=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 5 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),5,1) else '0' end     
        ,l7=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 6 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),6,1) else '0' end     
    from <tablename>

hello ovanstående plats baserat funktioner kan vara ytterligare används toogenerate ytterligare antal funktioner som tidigare beskrivits.

> [!TIP]
> Du kan via programmering Infoga hello poster med hjälp av ditt språk. Du kanske måste tooinsert hello data i segment tooimprove skrivåtgärder effektivitet [kolla hello exempel på hur toodo denna med pyodbc här](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python).
> Ett annat alternativ är tooinsert data i hello databas med hjälp av [BCP-verktyget](https://msdn.microsoft.com/library/ms162802.aspx)
> 
> 

### <a name="sql-aml"></a>Ansluta tooAzure Machine Learning
hello nya funktionen kan läggas till som en befintlig tabell tooan för kolumn eller lagras i en ny tabell och kopplas till hello ursprungliga tabellen för machine learning. Funktioner kan genereras eller komma åt om redan har skapats med hjälp av hello [importera Data](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) modul i Azure ML enligt nedan:

![azureml-läsare](./media/machine-learning-data-science-process-sql-server-virtual-machine/reader_db_featurizedinput.png)

## <a name="python"></a>Med hjälp av programmeringsspråk som Python
Använda Python toogenerate funktioner när hello data i SQL Server är liknande tooprocessing data i Azure blob använder Python enligt beskrivningen i [processen Azure Blob-data i du datavetenskap miljö](machine-learning-data-science-process-data-blob.md). hello data måste toobe läses in från hello databasen i ett pandas data och sedan behandlas vidare. Vi dokumentera hello process för att ansluta databasen toohello och hello data läses in i hello data ram i det här avsnittet.

hello följande Anslutningssträngens format kan vara används tooconnect tooa SQL Server-databas från Python med pyodbc (Ersätt servername, dbname, användarnamn och lösenord med dina specifika värden):

    #Set up hello SQL Azure connection
    import pyodbc
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

Hej [Pandas biblioteket](http://pandas.pydata.org/) i Python ger en omfattande uppsättning datastrukturer och verktyg för analys av data till datamanipulering för Python-programmering. hello koden nedan läser hello resultatet som returneras från en SQL Server-databas till en Pandas data ram:

    # Query database and load hello returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Nu kan du arbeta med hello Pandas data ram som beskrivs i avsnitt [skapa funktioner för Azure blob storage-data med hjälp av Panda](machine-learning-data-science-create-features-blob.md).

