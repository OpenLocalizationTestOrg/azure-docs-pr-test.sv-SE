---
title: "aaaIdentify avancerade analyser scenarier för Azure Machine Learning | Microsoft Docs"
description: "Välj hello lämpliga scenarier för att göra avancerade förutsägelseanalys med hello Team datavetenskap Process."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 53aecc1e-5089-42cf-8d44-77678653f92d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: 52c6bb10d6df4f640a4f66cf17cf4993cc1067b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scenarios-for-advanced-analytics-in-azure-machine-learning"></a>Scenarier för avancerade analyser i Azure Machine Learning
Den här artikeln beskrivs hello olika exempel datakällor och målscenarier som kan hanteras av hello [Team Data vetenskap processen (TDSP)](data-science-process-overview.md). Hej TDSP innehåller systematiskt för team toocollaborate om hur du skapar intelligent program. hello-scenarier som presenteras här visar alternativen i hello databearbetning arbetsflöde som beror på hello dataegenskaper källplatser och mål-databaser i Azure.

Hej **beslutsträdet** för att välja hello exempelscenarier som passar för dina data och målet visas i hello sista avsnittet.

> [!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]
> 
> 

Var och en av hello följande avsnitt innehåller ett exempelscenario. För varje scenario ett möjligt datavetenskap eller avancerade analyser flödet och ge support för Azure-resurser visas.

> [!NOTE]
> **För alla hello följande scenarier, måste du:**
> <br/>
> 
> * [Skapa ett lagringskonto](../storage/common/storage-create-storage-account.md)
>   <br/>
> * [Skapa en arbetsyta för Azure Machine Learning](machine-learning-create-workspace.md)
> 
> 

## <a name="smalllocal"></a>Scenariot \#1: liten toomedium tabular datauppsättning i lokala filer
![Lokala filer i små toomedium][1]

#### <a name="additional-azure-resources-none"></a>Ytterligare Azure-resurser: ingen
1. Logga in toohello [Azure Machine Learning Studio](https://studio.azureml.net/).
2. Överför en datamängd.
3. Skapa ett flöde för Azure Machine Learning-experiment som börjar med överförda datauppsättning/ar.

## <a name="smalllocalprocess"></a>Scenariot \#2: liten toomedium dataset för lokala filer som krävs
![Liten toomedium lokala filer med bearbetning][2]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Ytterligare Azure-resurser: Azure-dator (IPython anteckningsboken server)
1. Skapa en Azure-dator som kör IPython bärbar dator.
2. Ladda upp data tooan Azure storage-behållare.
3. Förbearbeta och rensa data i IPython anteckningsbok, kommer åt data från Azure storage-behållare.
4. Transformera data toocleaned, tabellform.
5. Spara transformerade data i Azure BLOB.
6. Logga in toohello [Azure Machine Learning Studio](https://studio.azureml.net/).
7. Läsa hello data från Azure BLOB med hello [importera Data] [ import-data] modul.
8. Skapa ett flöde för Azure Machine Learning-experiment som börjar med infogade datauppsättning/ar.

## <a name="largelocal"></a>Scenariot \#3: stora dataset av lokala filer, riktad på Azure-BLOB
![Stora lokala filer][3]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Ytterligare Azure-resurser: Azure-dator (IPython anteckningsboken server)
1. Skapa en Azure-dator som kör IPython bärbar dator.
2. Ladda upp data tooan Azure storage-behållare.
3. Förbearbeta och rensa data i IPython anteckningsbok, kommer åt data från Azure BLOB.
4. Transformera data toocleaned, tabellform, om det behövs.
5. Utforska data och skapa funktioner efter behov.
6. Extrahera ett datasampel för små till medelstora.
7. Spara hello exempeldata i Azure BLOB.
8. Logga in toohello [Azure Machine Learning Studio](https://studio.azureml.net/).
9. Läsa hello data från Azure BLOB med hello [importera Data] [ import-data] modul.
10. Skapa flödet för Azure Machine Learning-experiment från och med infogade datauppsättning/ar.

## <a name="smalllocaltodb"></a>Scenariot \#4: liten toomedium dataset av lokala filer, SQL Server i en virtuell dator i Azure som mål
![Liten toomedium lokala filer tooSQL DB i Azure][4]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Ytterligare Azure-resurser: Azure-dator (SQL Server / IPython anteckningsboken server)
1. Skapa en Azure-dator som kör SQL Server + IPython bärbar dator.
2. Ladda upp data tooan Azure storage-behållare.
3. Förbearbeta och rensa data i Azure storage-behållare med IPython bärbar dator.
4. Transformera data toocleaned, tabellform, om det behövs.
5. Spara tooVM-lokala datafiler (IPython bärbar dator körs på den virtuella datorn finns i lokala enheter tooVM enheter).
6. Läsa in data tooSQL Server-databas som körs på en virtuell dator i Azure.
   
   Alternativet \#1: använder SQL Server Management Studio.
   
   * Inloggningen tooSQL Server VM
   * Kör SQL Server Management Studio.
   * Skapa tabeller i databasen och målet.
   * Använd en av hello bulk importera metoder tooload hello data från VM-lokala filer.
   
   Alternativet \#2: med IPython anteckningsboken – inte lämpligt för medelstora och större datauppsättningar
   
   <!-- -->    
   * Använd ODBC-anslutning sträng tooaccess SQL Server på den virtuella datorn.
   * Skapa tabeller i databasen och målet.
   * Använd en av hello bulk importera metoder tooload hello data från VM-lokala filer.
7. Utforska data, skapa funktioner efter behov. Observera att hello funktioner inte behöver toobe materialiserad i hello databastabeller. Endast Observera hello nödvändiga frågan toocreate dem.
8. Besluta om exempel datastorleken, om det behövs och/eller önskas.
9. Logga in toohello [Azure Machine Learning Studio](https://studio.azureml.net/).
10. Läs hello data direkt från hello SQL Server med hello [importera Data] [ import-data] modul. Klistra in hello nödvändiga frågan som extraherar fält, skapar funktioner och exempel data om det behövs direkt i hello [importera Data] [ import-data] frågan.
11. Skapa flödet för Azure Machine Learning-experiment från och med infogade datauppsättning/ar.

## <a name="largelocaltodb"></a>Scenariot \#5: stora datauppsättning i lokala filer, mål SQL Server i Azure VM
![Stora lokala filer tooSQL DB i Azure][5]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Ytterligare Azure-resurser: Azure-dator (SQL Server / IPython anteckningsboken server)
1. Skapa en Azure-dator som kör SQL Server och IPython anteckningsboken server.
2. Ladda upp data tooan Azure storage-behållare.
3. (Valfritt) Förbearbeta och rensa data.
   
   a.  Förbearbeta och rensa data i IPython anteckningsbok, kommer åt data från Azure
   
       blobs.
   
   b.  Transformera data toocleaned, tabellform, om det behövs.
   
   c.  Spara tooVM-lokala datafiler (IPython bärbar dator körs på den virtuella datorn finns i lokala enheter tooVM enheter).
4. Läsa in data tooSQL Server-databas som körs på en virtuell dator i Azure.
   
   a.  Inloggningen tooSQL serverns virtuella dator.
   
   b.  Om data inte sparats redan hämta datafiler från Azure
   
       storage container toolocal-VM folder.
   
   c.  Kör SQL Server Management Studio.
   
   d.  Skapa tabeller i databasen och målet.
   
   e.  Använd en av hello bulk importera metoder tooload hello data.
   
   f.  Om tabellkopplingar krävs, skapa index tooexpedite kopplingar.
   
   > [!NOTE]
   > För snabbare överföring av stora datamängder rekommenderas det att du skapar partitionerade tabeller och massimportera hello data parallellt. Mer information finns i [Parallel Data importera tooSQL partitionerade tabeller](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).
   > 
   > 
5. Utforska data, skapa funktioner efter behov. Observera att hello funktioner inte behöver toobe materialiserad i hello databastabeller. Endast Observera hello nödvändiga frågan toocreate dem.
6. Besluta om exempel datastorleken, om det behövs och/eller önskas.
7. Logga in toohello [Azure Machine Learning Studio](https://studio.azureml.net/).
8. Läs hello data direkt från hello SQL Server med hello [importera Data] [ import-data] modul. Klistra in hello nödvändiga frågan som extraherar fält, skapar funktioner och exempel data om det behövs direkt i hello [importera Data] [ import-data] frågan.
9. Enkel Azure Machine Learning experiment flödet startar med överförda datamängd

## <a name="largedbtodb"></a>Scenariot \#6: stora dataset i en SQL Server-databas lokalt, SQL Server i en virtuell dator i Azure som mål
![Stora SQL DB lokal tooSQL DB i Azure][6]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Ytterligare Azure-resurser: Azure-dator (SQL Server / IPython anteckningsboken server)
1. Skapa en Azure-dator som kör SQL Server och IPython anteckningsboken server.
2. Använd en av hello exportera metoder tooexport hello data från SQL Server toodump filer.
   
   > [!NOTE]
   > Om du väljer toomove alla data från en alternativ (snabbare) metoden toomove hello fullständig toothe SQL Server-instansen i Azure-hello lokal databas. Hoppa över hello steg tooexport data, skapa och läsa in/import data toohello måldatabasen och följ hello alternativ metod.
   > 
   > 
3. Överför dump filer tooAzure lagringsbehållaren.
4. Läsa in hello data tooa SQL Server-databas som körs på en virtuell dator i Azure.
   
   a.  Inloggningen toohello SQL Server-VM.
   
   b.  Hämta data från en Azure storage-behållare toohello lokala VM-mapp.
   
   c.  Kör SQL Server Management Studio.
   
   d.  Skapa tabeller i databasen och målet.
   
   e.  Använd en av hello bulk importera metoder tooload hello data.
   
   f.  Om tabellkopplingar krävs, skapa index tooexpedite kopplingar.
   
   > [!NOTE]
   > Skapa partitionerade tabeller och toobulk importera hello data parallellt för snabbare överföring av stora datamängder. Mer information finns i [Parallel Data importera tooSQL partitionerade tabeller](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).
   > 
   > 
5. Utforska data, skapa funktioner efter behov. Observera att hello funktioner inte behöver toobe materialiserad i hello databastabeller. Endast Observera hello nödvändiga frågan toocreate dem.
6. Besluta om exempel datastorleken, om det behövs och/eller önskas.
7. Logga in toohello [Azure Machine Learning Studio](https://studio.azureml.net/).
8. Läs hello data direkt från hello SQL Server med hello [importera Data] [ import-data] modul. Klistra in hello nödvändiga frågan som extraherar fält, skapar funktioner och exempel data om det behövs direkt i hello [importera Data] [ import-data] frågan.
9. Enkel Azure Machine Learning experimentera flödet som börjar med överförda dataset.

### <a name="alternate-method-toocopy-a-full-database-from-an-on-premises--sql-server-tooazure-sql-database"></a>Alternativ metod toocopy en fullständig databas från en lokal SQL Server-tooAzure SQL-databas
![Koppla från lokala DB och bifoga tooSQL DB i Azure][7]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Ytterligare Azure-resurser: Azure-dator (SQL Server / IPython anteckningsboken server)
tooreplicate hello hela SQL Server-databas i SQL Server-VM, bör du kopiera en databas från en plats/server tooanother, förutsatt att hello-databasen kan tas offline. Du kan göra detta i hello SQL Server Management Studio Object Explorer, eller via hello motsvarande Transact-SQL-kommandon.

1. Koppla bort hello databasen på hello källplats. Mer information finns i [koppla från en databas](https://technet.microsoft.com/library/ms191491\(v=sql.110\).aspx).
2. I Utforskaren i Windows eller Windows kommandotolk-fönster frånkoppla kopiera hello databasfilen eller filer och loggfilen eller filer toohello målplats på hello SQL Server-VM i Azure.
3. Koppla hello kopieras filerna toohello mål SQL Server-instansen. Mer information finns i [ansluta en databas](https://technet.microsoft.com/library/ms190209\(v=sql.110\).aspx).

[Flytta en databas med hjälp av koppla från och ansluta (Transact-SQL)](https://technet.microsoft.com/library/ms187858\(v=sql.110\).aspx)

## <a name="largedbtohive"></a>Scenariot \#7: stordata i lokala filer mål Hive-databas i Azure HDInsight Hadoop-kluster
![Stordata i lokala mål Hive][9]

#### <a name="additional-azure-resources-azure-hdinsight-hadoop-cluster-and-azure-virtual-machine-ipython-notebook-server"></a>Ytterligare Azure-resurser: Azure HDInsight Hadoop-kluster och Azure-dator (IPython anteckningsboken server)
1. Skapa en Azure-dator som kör IPython anteckningsboken server.
2. Skapa ett Azure HDInsight Hadoop-kluster.
3. (Valfritt) Förbearbeta och rensa data.
   
   a.  Förbearbeta och rensa data i IPython anteckningsbok, kommer åt data från Azure
   
       blobs.
   
   b.  Transformera data toocleaned, tabellform, om det behövs.
   
   c.  Spara tooVM-lokala datafiler (IPython bärbar dator körs på den virtuella datorn finns i lokala enheter tooVM enheter).
4. Ladda upp data toohello standardbehållaren för hello Hadoop-kluster som valts i hello steg 2.
5. Läs in data tooHive databas i Azure HDInsight Hadoop-kluster.
   
   a.  Logga in toohello huvudnod hello Hadoop-kluster
   
   b.  Öppna hello Hadoop-kommandoraden.
   
   c.  Ange hello Hive rotkatalog med kommandot `cd %hive_home%\bin` i Hadoop-kommandoraden.
   
   d.  Kör hello Hive-frågor toocreate databasen och tabeller och Läs in data från blob storage-tooHive tabeller.
   
   > [!NOTE]
   > Om hello data är stort, kan användare skapa hello Hive-tabell med partitioner. Användare kan sedan använda en `for` loop i hello Hadoop kommandoraden på hello huvudnod tooload data till hello Hive-tabell partitioneras av partitionen.
   > 
   > 
6. Utforska data och skapa funktioner som behövs i Hadoop-kommandoraden. Observera att hello funktioner inte behöver toobe materialiserad i hello databastabeller. Endast Observera hello nödvändiga frågan toocreate dem.
   
   a.  Logga in toohello huvudnod hello Hadoop-kluster
   
   b.  Öppna hello Hadoop-kommandoraden.
   
   c.  Ange hello Hive rotkatalog med kommandot `cd %hive_home%\bin` i Hadoop-kommandoraden.
   
   d.  Kör hello Hive-frågor i Hadoop kommandoraden i hello huvudnod hello Hadoop-kluster tooexplore hello data och skapa funktioner efter behov.
7. Om det behövs och/eller önskas, exempel hello data toofit i Azure Machine Learning Studio.
8. Logga in toohello [Azure Machine Learning Studio](https://studio.azureml.net/).
9. Läsa hello data direkt från hello `Hive Queries` med hello [importera Data] [ import-data] modul. Klistra in hello nödvändiga frågan som extraherar fält, skapar funktioner och exempel data om det behövs direkt i hello [importera Data] [ import-data] frågan.
10. Enkel Azure Machine Learning experimentera flödet som börjar med överförda dataset.

## <a name="decisiontree"></a>Beslutsträd för val av scenariot
- - -
hello sammanfattar följande diagram hello-scenarier som beskrivs ovan och hello Advanced Analytics processen och tekniken val som görs ta tooeach hello specificerade scenarier. Observera att databearbetning, undersökning, funktionen tekniker och sampling kan ta i en eller flera metoden/miljö--vid hello källan, mellanliggande, och/eller mål-miljöer, och kan fortsätta upprepade gånger efter behov. hello diagrammet bara fungerar som en bild på några möjliga flöden och ger inte en uttömmande förteckning.

![Genomgång exempelscenarier DS process][8]

### <a name="advanced-analytics-in-action-examples"></a>Avancerade analyser i praktiken exempel
För slutpunkt till slutpunkt Azure Machine Learning genomgångar som använder Hej Advanced Analytics processen och tekniken med offentliga datauppsättningar, se:

* [Teamet vetenskap av data i praktiken: använder SQL Server](machine-learning-data-science-process-sql-walkthrough.md).
* [Teamet vetenskap av data i praktiken: med HDInsight Hadoop-kluster](machine-learning-data-science-process-hive-walkthrough.md).

[1]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-small-in-aml.png
[2]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-with-processing.png
[3]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-large.png
[4]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-to-db.png
[5]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-large-to-db.png
[6]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-db-to-db.png
[7]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-attach-db.png
[8]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-sample-scenarios.png
[9]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-to-hive.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
