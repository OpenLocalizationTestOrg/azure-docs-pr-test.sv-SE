---
title: aaaExplore data i en SQL Server-dator i Azure | Microsoft Docs
description: "Utforska data och skapa funktioner i en virtuell dator i SQL Server på Azure"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: 
ms.assetid: 3949fb2c-ffab-49fb-908d-27d5e42f743b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: fashah;garye;bradsev
ms.openlocfilehash: 67f4b058b0f6557ee15fd42795c918d68f1a9871
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="heading"></a>Bearbeta Data i SQL Server-dator på Azure
Det här dokumentet beskriver hur tooexplore data och generera data som lagras i en SQL Server-VM på Azure-funktioner. Detta kan göras med data wrangling med hjälp av SQL eller med ett programmeringsspråk som Python.

> [!NOTE]
> hello exempel SQL-instruktioner i det här dokumentet förutsätter att data är i SQL Server. Om det inte finns toohello moln datavetenskap processen kartan toolearn hur toomove dina data tooSQL Server.
> 
> 

## <a name="SQL"></a>Med SQL
Vi beskriver hello följande data wrangling uppgifter i det här avsnittet med hjälp av SQL:

1. [Datagranskning](#sql-dataexploration)
2. [Funktionen Generation](#sql-featuregen)

### <a name="sql-dataexploration"></a>Datagranskning
Här följer några exempel SQL-skript som kan använda tooexplore datalager i SQL Server.

> [!NOTE]
> En praktisk t.ex, du kan använda hello [NYC Taxi dataset](http://www.andresmh.com/nyctaxitrips/) och finns toohello IPNB med titeln [NYC Data wrangling IPython anteckningsboken och SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) för en slutpunkt till slutpunkt-hanteringspaketen.
> 
> 

1. Hämta hello antal observationer per dag
   
    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 
2. Hämta hello nivåer i en kategoriska kolumn
   
    `select  distinct <column_name> from <databasename>`
3. Hämta hello antalet nivåer i kombination med två kategoriska kolumner 
   
    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`
4. Hämta hello distribution för numeriska kolumner
   
    `select <column_name>, count(*) from <tablename> group by <column_name>`

### <a name="sql-featuregen"></a>Funktionen Generation
I det här avsnittet beskrivs olika sätt att skapa funktioner med hjälp av SQL:  

1. [Antal baserat funktionen Generation](#sql-countfeature)
2. [Diskretisering funktionen Generation](#sql-binningfeature)
3. [Lansera hello funktioner från en enda kolumn](#sql-featurerollout)

> [!NOTE]
> När du skapar ytterligare funktioner, kan du lägga till dem som kolumner toohello befintlig tabell eller skapa en ny tabell med hello ytterligare funktioner och primärnyckel som kan förenas med hello ursprungliga tabellen. 
> 
> 

### <a name="sql-countfeature"></a>Antal baserat funktionen Generation
hello visar följande exempel två sätt att generera antal funktioner. hello första metoden använder Villkorlig summering och hello andra metoden använder hello uttrycket 'where'. De kan sedan kopplas med hello ursprungliga tabellen (med primärnyckelkolumnerna) toohave antal funktioner tillsammans med hello ursprungliga data.

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3> 

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename> 
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2> 

### <a name="sql-binningfeature"></a>Diskretisering funktionen Generation
hello följande exempel visas hur toogenerate binned funktioner av diskretisering (med fem lagerplatser) en numerisk kolumn som kan användas som en funktion i stället:

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


### <a name="sql-featurerollout"></a>Lansera hello funktioner från en enda kolumn
I det här avsnittet visar vi hur tooroll i en kolumn i en tabell toogenerate ytterligare funktioner. hello exemplet förutsätter att det finns en latitud och longitud kolumn i hello tabell från vilken du försöker toogenerate funktioner.

Här är en kort introduktion om latitud/longitud platsdata (resurstilldelas från stackoverflow [hur toomeasure hello riktighet latitud och longitud?](http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude)). Detta är användbart toounderstand innan featurizing hello Platsfältet:

* hello logga talar om för oss om vi är norr eller Syd, Öst eller Väst på hello jordglob.
* Ett annat värde än noll hundratals siffra talar om för oss att vi använder longitud inte latitud!
* hello flera siffror ger en position tooabout 1 000 kilometer. Det ger oss användbar information om vilka kontinent eller oceanen vi finns på.
* hello enheter siffra (en decimal grad) ger en position upp too111 kilometer (60 sjömil, om 69 miles). Det kan berätta ungefär vad eller det stora land vi finns i.
* hello första decimaltecknet är värda too11.1 km: den kan skilja hello positionen för en stor ort från en närliggande stora stad.
* hello andra decimaltecknet är värda too1.1 km: den kan skilja en village från hello nästa.
* hello tredje decimaltecknet är värda upp too110 m: den kan identifiera en stor jordbruket institutionella plats.
* hello fjärde decimaltecknet är värda upp too11 m: mark kan identifiera. Det är jämförbar toohello vanliga korrektheten i en okorrigerade GPS-enhet utan störningar.
* hello femte decimaltecknet är värda upp too1.1 m: den särskiljer träd från varandra. Precision toothis nivå med kommersiella GPS-enheter kan endast uppnås med differentiell korrigering.
* hello sjätte decimaltecknet är värda upp too0.11 m: kan du använda den för att utforma strukturer i detalj för att utforma landskap, skapa vägar. Det bör vara mer än tillräckligt bra för spårning av glaciers och floder. Detta kan uppnås genom att göra painstaking åtgärder med GPS, till exempel differentially korrigerade GPS.

hello platsinformation kan vara featurized på följande sätt att skilja ut region, plats och information om ort. Observera att du kan också anropa en REST-slutpunkt, till exempel Bing Maps API finns på [hitta en plats med en punkt](https://msdn.microsoft.com/library/ff701710.aspx) tooget hello region/distrikt information.

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

Dessa funktioner för plats-baserad kan vara mer används toogenerate ytterligare antal funktioner som tidigare beskrivits. 

> [!TIP]
> Du kan via programmering Infoga hello poster med hjälp av ditt språk. Du kanske måste tooinsert hello data i segment tooimprove skrivåtgärder effektivitet (ett exempel på hur toodo denna med pyodbc finns [A HelloWorld exempel tooaccess SQLServer med python](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python)). Ett annat alternativ är tooinsert data i hello-databas med hjälp av hello [BCP-verktyget](https://msdn.microsoft.com/library/ms162802.aspx).
> 
> 

### <a name="sql-aml"></a>Ansluta tooAzure Machine Learning
hello nya funktionen kan läggas till som en befintlig tabell tooan för kolumn eller lagras i en ny tabell och kopplas till hello ursprungliga tabellen för machine learning. Funktioner kan genereras eller komma åt om redan har skapats med hjälp av hello [importera Data] [ import-data] modul i Azure Machine Learning enligt nedan:

![azureml-läsare][1] 

## <a name="python"></a>Med hjälp av programmeringsspråk som Python
Med hjälp av Python tooexplore data och generera funktioner när hello data finns i SQL Server är liknande tooprocessing data i Azure blob använder Python enligt beskrivningen i [processen Azure Blob-data i datavetenskap miljön](machine-learning-data-science-process-data-blob.md). hello data måste toobe läses in från hello databasen i ett pandas data och sedan behandlas vidare. Vi dokumentera hello process för att ansluta databasen toohello och hello data läses in i hello data ram i det här avsnittet.

hello följande Anslutningssträngens format kan vara används tooconnect tooa SQL Server-databas från Python med pyodbc (Ersätt servername, dbname, användarnamn och lösenord med dina specifika värden):

    #Set up hello SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

Hej [Pandas biblioteket](http://pandas.pydata.org/) i Python ger en omfattande uppsättning datastrukturer och verktyg för analys av data till datamanipulering för Python-programmering. hello koden nedan läser hello resultatet som returneras från en SQL Server-databas till en Pandas data ram:

    # Query database and load hello returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Nu kan du arbeta med hello Pandas data ram som beskrivs i artikeln hello [processen Azure Blob-data i datavetenskap miljön](machine-learning-data-science-process-data-blob.md).

## <a name="azure-data-science-in-action-example"></a>Azure datavetenskap i åtgärden exempel
En slutpunkt till slutpunkt genomgången exempel på hello Azure vetenskap av data med hjälp av en offentlig dataset finns [Azure datavetenskap hur fungerar](machine-learning-data-science-process-sql-walkthrough.md).

[1]: ./media/machine-learning-data-science-process-sql-server-virtual-machine/reader_db_featurizedinput.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

