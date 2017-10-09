---
title: aaaExplore data i SQL Server-dator i Azure | Microsoft Docs
description: "Hur tooexplore data som lagras i en SQL Server-VM på Azure."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: ccbb3085-af9e-4ec2-9df2-15dcab261d05
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: fcc449fc0d0e49be9b673cfb2de347cf44804017
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="explore-data-in-sql-server-virtual-machine-on-azure"></a>Utforska data i en virtuell dator med SQL Server på Azure
Det här dokumentet beskriver hur tooexplore data som lagras i en SQL Server-VM på Azure. Detta kan göras med data wrangling med hjälp av SQL eller med ett programmeringsspråk som Python.

hello följande **menyn** länkar tootopics som beskriver hur toouse verktyg tooexplore data från olika miljöer för lagring. Det här är ett steg i hello Cortana Analytics processen (CAP).

[!INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

> [!NOTE]
> hello exempel SQL-instruktioner i det här dokumentet förutsätter att data är i SQL Server. Om det inte finns toohello moln datavetenskap processen kartan toolearn hur toomove dina data tooSQL Server.
> 
> 

## <a name="sql-dataexploration"></a>Utforska SQL-data med SQL-skript
Här följer några exempel SQL-skript som kan använda tooexplore datalager i SQL Server.

1. Hämta hello antal observationer per dag
   
    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 
2. Hämta hello nivåer i en kategoriska kolumn
   
    `select  distinct <column_name> from <databasename>`
3. Hämta hello antalet nivåer i kombination med två kategoriska kolumner 
   
    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`
4. Hämta hello distribution för numeriska kolumner
   
    `select <column_name>, count(*) from <tablename> group by <column_name>`

> [!NOTE]
> En praktisk t.ex, du kan använda hello [NYC Taxi dataset](http://www.andresmh.com/nyctaxitrips/) och finns toohello IPNB med titeln [NYC Data wrangling IPython anteckningsboken och SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) för en slutpunkt till slutpunkt-hanteringspaketen.
> 
> 

## <a name="python"></a>Utforska SQL-data med Python
Med hjälp av Python tooexplore data och generera funktioner när hello data finns i SQL Server är liknande tooprocessing data i Azure blob med Python, enligt beskrivningen i [processen Azure Blob-data i datavetenskap miljön](machine-learning-data-science-process-data-blob.md). hello data måste toobe läses in från hello databas i en pandas DataFrame och sedan behandlas vidare. Vi dokumentera hello process för att ansluta toohello databasen och läsa in hello data i hello DataFrame i det här avsnittet.

hello följande Anslutningssträngens format kan vara används tooconnect tooa SQL Server-databas från Python med pyodbc (Ersätt servername, dbname, användarnamn och lösenord med dina specifika värden):

    #Set up hello SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

Hej [Pandas biblioteket](http://pandas.pydata.org/) i Python ger en omfattande uppsättning datastrukturer och verktyg för analys av data till datamanipulering för Python-programmering. hello läser följande kod hello resultatet som returneras från en SQL Server-databas till en Pandas data ram:

    # Query database and load hello returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Nu kan du arbeta med hello Pandas DataFrame som beskrivs i avsnittet hello [processen Azure Blob-data i datavetenskap miljön](machine-learning-data-science-process-data-blob.md).

## <a name="cortana-analytics-process-in-action-example"></a>Cortana Analytics processen i åtgärden exempel
En slutpunkt till slutpunkt genomgången exempel på hello Cortana Analytics Process som använder en offentlig dataset finns [hello Team av vetenskapliga data i praktiken: använder SQL Server](machine-learning-data-science-process-sql-walkthrough.md).

