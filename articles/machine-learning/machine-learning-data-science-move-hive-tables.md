---
title: "aaaCreate Hive tabeller och Läs in data från Azure Blob Storage | Microsoft Docs"
description: "Skapa Hive-tabeller och läsa in data i blob toohive tabeller"
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: cff9280d-18ce-4b66-a54f-19f358d1ad90
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: 09622972bcac31c2971858393a8340f24e4b7390
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-hive-tables-and-load-data-from-azure-blob-storage"></a>Skapa Hive-tabeller och Läs in data från Azure Blob Storage
Det här avsnittet innehåller allmänna Hive-frågor som skapar Hive-tabeller och läsa in data från Azure blob storage. Vägledning finns även på partitionering Hive-tabeller och om hur du använder hello optimerade raden kolumner (ORC) formatering tooimprove frågeprestanda.

Detta **menyn** länkar tootopics som beskriver hur tooingest data till mål-miljöer där hello data kan lagras och behandlas under hello Team Data vetenskap processen (TDSP).

[!INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

## <a name="prerequisites"></a>Krav
Den här artikeln förutsätter att du har:

* Skapa ett Azure storage-konto. Om du behöver mer information, se [om Azure storage-konton](../storage/common/storage-create-storage-account.md).
* Etablera ett anpassat Hadoop-kluster med hello HDInsight-tjänst.  Om du behöver mer information, se [anpassa Azure HDInsight Hadoop-kluster för avancerade analyser](machine-learning-data-science-customize-hadoop-cluster.md).
* Aktiverade fjärråtkomst toohello klustret inloggad och öppnas hello Hadoop kommandoradskonsol. Om du behöver mer information, se [åtkomst hello Head-nod för Hadoop-kluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).

## <a name="upload-data-tooazure-blob-storage"></a>Ladda upp data tooAzure blob-lagring
Om du har skapat en virtuell Azure-dator genom att följa instruktionerna i hello [ställa in en virtuell Azure-dator för avancerade analyser](machine-learning-data-science-setup-virtual-machine.md), den här skriptfilen skulle ha varit hämtade toohello *C:\\ Användare\\\<användarnamn\>\\dokument\\datavetenskap skript* på hello virtuell dator. Dessa Hive-frågor kräver bara att du ansluter dina egna dataschemat och Azure blob storage-konfiguration i hello relevanta fält toobe klar för överföring.

Vi förutsätter att hello data för Hive-tabeller är i ett **okomprimerad** tabellformat och att hello data har överförts toohello standard (eller tooan ytterligare) behållare för hello storage-konto som används av hello Hadoop-kluster.

Om du vill toopractice på hello **NYC Taxi resa Data**, måste du:

* **Hämta** hello 24 [NYC Taxi resa Data](http://www.andresmh.com/nyctaxitrips) filer (12 resa filer och 12 avgiften-filer)
* **Packa upp** alla filer i CSV-filer, och sedan
* **Överför** dem toohello standard (eller lämplig behållare) av hello Azure storage-konto som har skapats av hello proceduren som beskrivs i hello [anpassa Azure HDInsight Hadoop-kluster för Advanced Analytics processen och teknik ](machine-learning-data-science-customize-hadoop-cluster.md) avsnittet. hello processen tooupload hello CSV-filer toohello standardbehållaren för hello storage-konto kan hittas på den här [sidan](machine-learning-data-science-process-hive-walkthrough.md#upload).

## <a name="submit"></a>Hur toosubmit Hive-frågor
Du kan skicka hive-frågor med hjälp av:

1. [Skicka Hive-frågor via Hadoop kommandoraden i headnode av Hadoop-kluster](#headnode)
2. [Skicka Hive-frågor med hello Hive-redigeraren](#hive-editor)
3. [Skicka Hive-frågor med Azure PowerShell-kommandon](#ps)

Hive-frågor är SQL-liknande. Om du är bekant med SQL hello kan vara [Hive för SQL användare Cheat blad](http://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf) användbart.

När en Hive-fråga, kan du också styra hello målet hello utdata från Hive-frågor om den finnas på hello skärmen eller tooa lokal fil på hello huvudnod eller tooan Azure blob.

### <a name="headnode"></a> 1. Skicka Hive-frågor via Hadoop kommandoraden i headnode av Hadoop-kluster
Om hello Hive är-frågan komplex eller skicka den direkt i hello huvudnod hello Hadoop-kluster normalt leder toofaster Stäng runt än att skicka den med en Hive-redigerare eller Azure PowerShell-skript.

Logga in toohello huvudnod hello Hadoop-kluster, öppna hello Hadoop kommandoraden på hello skrivbord hello huvudnod och anger kommandot `cd %hive_home%\bin`.

Det finns tre sätt toosubmit Hive-frågor i hello Hadoop-kommandoraden:

* direkt
* med hjälp av .hql filer
* med hello Hive kommandokonsolen

#### <a name="submit-hive-queries-directly-in-hadoop-command-line"></a>Skicka Hive-frågor direkt i Hadoop kommandoraden.
Du kan köra kommandot som `hive -e "<your hive query>;` toosubmit enkel Hive-frågor direkt i Hadoop kommandoraden. Här är ett exempel där hello röda rutan beskrivs hello kommando som skickar hello Hive-fråga och hello gröna rutan beskrivs hello utdata från hello Hive-fråga.

![Skapa arbetsyta](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-1.png)

#### <a name="submit-hive-queries-in-hql-files"></a>Skicka Hive-frågor i .hql filer
När hello Hive-frågan är mer komplicerad och har flera rader, är redigerar frågor i kommandoraden eller Hive kommandokonsolen inte praktiskt. Ett alternativ är toouse en textredigerare i hello huvudnod hello Hadoop-kluster toosave hello Hive-frågor i en .hql-fil i en lokal katalog på hello huvudnod. Sedan kan skicka hello Hive-fråga i hello .hql filen med hjälp av hello `-f` argumentet på följande sätt:

    hive -f "<path toohello .hql file>"

![Skapa arbetsyta](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-3.png)

**Ignorera förlopp status skärmen utskrift av Hive-frågor**

Som standard när Hive-frågan har skickats i Hadoop kommandoraden skrivs hello fortskrider hello kartan/minska jobb ut på skärmen. toosuppress hello skärmen utskrift av hello kartan/minska jobb pågår, kan du använda ett argument `-S` (”S” i versaler) i hello kommandoraden på följande sätt:

    hive -S -f "<path toohello .hql file>"
.    hive -S -e ”<Hive queries>”

#### <a name="submit-hive-queries-in-hive-command-console"></a>Skicka Hive-frågor i Hive-Kommandotolken.
Du kan också ange hello Hive kommandokonsolen genom att köra kommandot `hive` i Hadoop kommandoraden och skicka Hive-frågor i Hive-Kommandotolken. Här är ett exempel. I det här exemplet hello två röda rutor markeringen hello kommandon som används för tooenter hello Hive kommandokonsolen och hello skickade i Hive kommandokonsolen respektive Hive-fråga. hello gröna rutan visar hello utdata från hello Hive-fråga.

![Skapa arbetsyta](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-2.png)

hello föregående exempel utdata direkt hello Hive frågeresultat på skärmen. Du kan också skriva hello tooa lokala utdatafilen på hello huvudnod eller tooan Azure blob. Du kan sedan använda andra verktyg toofurther analysera hello utdata för Hive-frågor.

**Hive-fråga resultat tooa lokala utdatafilen.**
toooutput Hive-fråga resultat tooa lokal katalog på hello huvudnod, har du toosubmit hello Hive-fråga i hello Hadoop kommandoraden på följande sätt:

    hive -e "<hive query>" > <local path in hello head node>

I följande exempel hello, hello utdata för Hive-fråga skrivs till en fil `hivequeryoutput.txt` i katalogen `C:\apps\temp`.

![Skapa arbetsyta](./media/machine-learning-data-science-move-hive-tables/output-hive-results-1.png)

**Utdata Hive-fråga resultat tooan Azure blob**

Du kan också spara hello Hive-fråga resultat tooan Azure blob inom hello standardbehållaren för hello Hadoop-kluster. hello Hive-fråga för det här är följande:

    insert overwrite directory wasb:///<directory within hello default container> <select clause from ...>

I följande exempel hello, hello utdata för Hive-fråga skrivs tooa blob directory `queryoutputdir` inom hello standardbehållaren för hello Hadoop-kluster. Här, behöver du bara tooprovide hello katalognamn utan hello blob namn. Ett fel returneras om du anger både katalog och blob namn, exempelvis `wasb:///queryoutputdir/queryoutput.txt`.

![Skapa arbetsyta](./media/machine-learning-data-science-move-hive-tables/output-hive-results-2.png)

Om du öppnar hello standardbehållaren för hello Hadoop-kluster med Azure Lagringsutforskaren ser hello utdata från hello Hive-fråga som visas i följande bild hello. Du kan använda hello filter (visas med röd ruta) tooonly hämta hello blob med angivna bokstäverna i namn.

![Skapa arbetsyta](./media/machine-learning-data-science-move-hive-tables/output-hive-results-3.png)

### <a name="hive-editor"></a> 2. Skicka Hive-frågor med hello Hive-redigeraren
Du kan också använda hello frågan konsolen (Hive-redigeraren) genom att ange en URL i formatet hello *https://&#60; Hadoop-klusternamn >.azurehdinsight.net/Home/HiveEditor* i en webbläsare. Du måste vara inloggad hello finns den här konsolen och du måste ha autentiseringsuppgifter här din Hadoop-kluster.

### <a name="ps"></a> 3. Skicka Hive-frågor med Azure PowerShell-kommandon
Du kan också använda PowerShell toosubmit Hive-frågor. Instruktioner finns i [skicka Hive-jobb med hjälp av PowerShell](../hdinsight/hdinsight-hadoop-use-hive-powershell.md).

## <a name="create-tables"></a>Skapa Hive-databasen och tabeller
hello Hive-frågor delas i hello [GitHub-lagringsplatsen](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_db_tbls_load_data_generic.hql) och kan hämtas därifrån.

Här är hello Hive-fråga som skapar en Hive-tabell.

    create database if not exists <database name>;
    CREATE EXTERNAL TABLE if not exists <database name>.<table name>
    (
        field1 string,
        field2 int,
        field3 float,
        field4 double,
        ...,
        fieldN string
    )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' lines terminated by '<line separator>'
    STORED AS TEXTFILE LOCATION '<storage location>' TBLPROPERTIES("skip.header.line.count"="1");

Här följer hello beskrivningar av hello fält som du behöver tooplug i och andra konfigurationer:

* **&#60; databasnamn >**: hello namnet på hello-databasen som du vill toocreate. Om du bara vill toouse hello standarddatabasen hello frågan *Skapa databas...*  kan utelämnas.
* **&#60; tabellnamn >**: hello namnet på hello-tabell som du vill toocreate inom hello angivna databasen. Om du vill toouse hello standarddatabasen hello tabellen kan refereras direkt av *&#60; tabellnamn >* utan &#60; databasnamn >.
* **&#60; fältavgränsaren >**: hello avgränsare som avgränsar fälten i hello data filen toobe överförs toohello Hive-tabell.
* **&#60; radbrytningstecken >**: hello avgränsare som avgränsar rader i datafilen hello.
* **&#60; lagringsplats >**: hello Azure storage toosave hello lokaliseringsuppgifter för Hive-tabeller. Om du inte anger *plats &#60; lagringsplats >*hello databasen och hello tabeller lagras i *hive/datalager/* katalogen i hello standardbehållaren hello Hive-klustret som standard. Om du vill toospecify hello lagringsplats har hello lagringsplats toobe inom hello standardbehållaren för hello-databasen och tabeller. Den här platsen har toobe kallas plats relativa toohello standardbehållaren hello klustret i hello-format för *' wasb: / / / &#60; katalogen 1 > / ”* eller *' wasb: / / / &#60; katalogen 1 > / &#60; katalogen 2 > / ”*osv. När hello-frågan körs skapas hello relativa kataloger inom hello standardbehållaren.
* **TBLPROPERTIES("Skip.Header.Line.Count"="1")**: om hello datafilen har en rubrikrad, har du tooadd den här egenskapen **hello slutet** av hello *Skapa tabell* frågan. Annars hello rubrikraden har lästs in som en tabell med poster toohello. Om hello datafilen saknar en huvudrad kan den här konfigurationen utelämnas i hello-frågan.

## <a name="load-data"></a>Läsa in data tooHive tabeller
Här är hello Hive-frågan som läser in data i en Hive-tabell.

    LOAD DATA INPATH '<path tooblob data>' INTO TABLE <database name>.<table name>;

* **&#60; sökvägen tooblob data >**: om hello blob filen har överförts toobe toohello Hive tabellen i hello standardbehållaren för hello HDInsight Hadoop-kluster hello *&#60; sökvägen tooblob data >* måste vara i formatet hello *' wasb: / / / &#60; katalogen i den här behållaren > / &#60; blob filnamn >'*. hello blob-fil kan också vara ytterligare en behållare för hello HDInsight Hadoop-kluster. I det här fallet *&#60; sökvägen tooblob data >* måste vara i formatet hello *' wasb: / / &#60; behållarens namn > @&#60; lagringskontonamnet >.blob.core.windows.net/ &#60; blob filnamn >'*.

  > [!NOTE]
  > hello har blob data överförs toobe tooHive tabellen toobe i hello standard eller ytterligare en behållare för hello lagringskonto för hello Hadoop-kluster. Annars hello *Läs in DATA* frågan inte kunde köras klagande att det går inte att komma åt hello data.
  >
  >

## <a name="partition-orc"></a>Avancerade alternativ: partitionerad tabell och lagra Hive-data i ORC-format
Om hello data är stor, är partitionering hello tabell bra för frågor som bara behöver tooscan några partitioner för hello tabell. Det är exempelvis rimliga toopartition hello loggdata för en webbplats med datum.

I tillägg toopartitioning Hive tabeller, det är också bra toostore hello Hive data i hello optimerade raden kolumner (ORC)-format. Mer information om ORC formatering finns <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC#LanguageManualORC-ORCFiles" target="_blank">med ORC-filer som förbättrar prestanda när Hive läsning, skrivning och bearbetning av data</a>.

### <a name="partitioned-table"></a>Partitionerad tabell
Här är hello Hive-fråga som skapar en partitionerad tabell och läser in data i den.

    CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<table name>
    (field1 string,
    ...
    fieldN string
    )
    PARTITIONED BY (<partitionfieldname> vartype) ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
         lines terminated by '<line separator>' TBLPROPERTIES("skip.header.line.count"="1");
    LOAD DATA INPATH '<path toohello source file>' INTO TABLE <database name>.<partitioned table name>
        PARTITION (<partitionfieldname>=<partitionfieldvalue>);

När du frågar partitionerade tabeller bör tooadd hello partition villkor i hello **början** av hello `where` satsen som detta förbättrar hello effekt söker avsevärt.

    select
        field1, field2, ..., fieldN
    from <database name>.<partitioned table name>
    where <partitionfieldname>=<partitionfieldvalue> and ...;

### <a name="orc"></a>Hive data lagras i ORC-format
Du kan inte direkt läsa in data från blob storage i Hive-tabeller som är lagrad i hello ORC-format. Här är hello stegen som hello som du behöver tootake tooload data från Azure BLOB-objekt tooHive tabeller som lagras i ORC-format.

Skapa en extern tabell **LAGRAS AS TEXTFILE** och läsa in data från blob storage toohello tabell.

        CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<external textfile table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
            lines terminated by '<line separator>' STORED AS TEXTFILE
            LOCATION 'wasb:///<directory in Azure blob>' TBLPROPERTIES("skip.header.line.count"="1");

        LOAD DATA INPATH '<path toohello source file>' INTO TABLE <database name>.<table name>;

Skapa en intern tabell med hello samma schema som hello extern tabell i steg 1, med hello samma fältavgränsaren och lagra hello Hive-data i hello ORC format.

        CREATE TABLE IF NOT EXISTS <database name>.<ORC table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' STORED AS ORC;

Välj data från hello extern tabell i steg 1 och infoga i hello ORC tabell

        INSERT OVERWRITE TABLE <database name>.<ORC table name>
            SELECT * FROM <database name>.<external textfile table name>;

> [!NOTE]
> Om hello TEXTFILE tabell *&#60; databasnamn >. &#60; externa textfile tabellnamn >* har partitioner i steg3 hello `SELECT * FROM <database name>.<external textfile table name>` kommandot väljer hello partition variabeln som ett fält i hello returneras datauppsättning. Infoga den i hello *&#60; databasnamn >. &#60; ORC tabellnamn >* misslyckas eftersom *&#60; databasnamn >. &#60; ORC tabellnamn >* hello partition variabel som inte har en fält i hello tabellens schema. I det här fallet behöver du toospecifically väljer hello fält toobe infogas för*&#60; databasnamn >. &#60; ORC-tabellnamn >* på följande sätt:
>
>

        INSERT OVERWRITE TABLE <database name>.<ORC table name> PARTITION (<partition variable>=<partition value>)
           SELECT field1, field2, ..., fieldN
           FROM <database name>.<external textfile table name>
           WHERE <partition variable>=<partition value>;

Det är säkert toodrop hello *&#60; externa textfile tabellnamn >* när med hello följande fråga efter alla data som har infogats i *&#60; databasnamn >. &#60; ORC-tabellnamn >*:

        DROP TABLE IF EXISTS <database name>.<external textfile table name>;

När den här proceduren bör du ha en tabell med data i hello ORC format redo toouse.  
