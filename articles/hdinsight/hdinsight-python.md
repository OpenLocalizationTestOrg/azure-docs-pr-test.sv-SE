---
title: aaaPython UDF med Apache Hive och Pig - Azure HDInsight | Microsoft Docs
description: "Lär dig hur toouse Python användaren definierat funktioner (UDF) från Hive och Pig i HDInsight, hello Hadoop teknik stacken på Azure."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: c44d6606-28cd-429b-b535-235e8f34a664
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 07/17/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 26d8160cc6ed7fc22c3f06f7c1c9954c224b2366
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-python-user-defined-functions-udf-with-hive-and-pig-in-hdinsight"></a>Använd användardefinierade Python funktioner (UDF) med Hive och Pig i HDInsight

Lär dig hur toouse Python användardefinierade funktioner (UDF) med Apache Hive och Pig i Hadoop på Azure HDInsight.

## <a name="python"></a>Python på HDInsight

Python2.7 installeras som standard på HDInsight 3.0 och senare. Apache Hive kan användas med den här versionen av Python för bearbetning av dataströmmen. Bearbetning av dataströmmen använder STDOUT- och STDIN toopass data mellan Hive och hello UDF.

HDInsight innehåller också Jython, vilket är en Python-implementering skriven i Java. Jython körs direkt på hello Java Virtual Machine och använder inte strömning. Jython är hello rekommenderade Python-tolkning när du använder Python med Pig.

> [!WARNING]
> hello stegen i det här dokumentet att Hej följande förutsättningar: 
>
> * Skapar du hello Python-skript på din lokala utvecklingsmiljö.
> * Du överför hello skript tooHDInsight med hjälp av antingen hello `scp` från en lokal Bash-session eller hello tillhandahålls PowerShell-skript.
>
> Om du vill toouse hello [Azure Cloud Shell (bash)](https://docs.microsoft.com/azure/cloud-shell/overview) Förhandsgranska toowork med HDInsight, måste du:
>
> * Skapa hello skript i hello shell molnmiljö.
> * Använd `scp` tooupload hello filer från hello molnet shell tooHDInsight.
> * Använd `ssh` från hello molnet shell tooconnect tooHDInsight och kör hello exempel.

## <a name="hivepython"></a>Hive UDF

Python kan användas som en UDF från Hive via hello HiveQL `TRANSFORM` instruktionen. Till exempel hello följande HiveQL anropar hello `hiveudf.py` filen lagras i hello Azure standardkontot för lagring för hello-kluster.

**Linux-baserat HDInsight**

```hiveql
add file wasb:///hiveudf.py;

SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'python hiveudf.py' AS
    (clientid string, phoneLable string, phoneHash string)
FROM hivesampletable
ORDER BY clientid LIMIT 50;
```

**Windows-baserat HDInsight**

```hiveql
add file wasb:///hiveudf.py;

SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'D:\Python27\python.exe hiveudf.py' AS
    (clientid string, phoneLable string, phoneHash string)
FROM hivesampletable
ORDER BY clientid LIMIT 50;
```

> [!NOTE]
> På Windows-baserade HDInsight-kluster hello `USING` -satsen måste innehålla hello fullständig sökväg toopython.exe.

Här är det här exemplet har:

1. Hej `add file` hello början av hello fil läggs hello `hiveudf.py` filen toohello distribuerad cache, så är den tillgänglig för alla noder i klustret hello.
2. Hej `SELECT TRANSFORM ... USING` instruktionen väljer data från hello `hivesampletable`. Skickar också hello clientid, devicemake och devicemodel värden toohello `hiveudf.py` skript.
3. Hej `AS` satsen beskrivs hello fält som returneras från `hiveudf.py`.

<a name="streamingpy"></a>

### <a name="create-hello-hiveudfpy-file"></a>Skapa hello hiveudf.py fil


Skapa en textfil med namnet på din utvecklingsmiljö `hiveudf.py`. Använd hello efter koden som hello innehållet i hello-fil:

```python
#!/usr/bin/env python
import sys
import string
import hashlib

while True:
    line = sys.stdin.readline()
    if not line:
        break

    line = string.strip(line, "\n ")
    clientid, devicemake, devicemodel = string.split(line, "\t")
    phone_label = devicemake + ' ' + devicemodel
    print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])
```

Det här skriptet utför hello följande åtgärder:

1. Läsa en rad med data från STDIN.
2. hello avslutande radmatningstecken tas bort med `string.strip(line, "\n ")`.
3. När du gör dataströmmen bearbetning, innehåller en enda rad hello värden med ett tabbtecken mellan varje värde. Så `string.split(line, "\t")` kan vara används toosplit hello indata på varje flik, returnerar bara hello fält.
4. När bearbetningen är klar måste hello utdata skrivas tooSTDOUT som en enda rad med en flik mellan varje fält. Till exempel `print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])`.
5. Hej `while` loop upprepas tills inga `line` läses.

hello skriptets utdata är en sammansättning av hello indatavärden för `devicemake` och `devicemodel`, och en hash av hello sammanfogas värde.

Se [köra hello exempel](#running) för hur toorun det här exemplet på ditt HDInsight-kluster.

## <a name="pigpython"></a>Pig UDF

Python-skriptet kan användas som en UDF från Pig via hello `GENERATE` instruktionen. Du kan köra hello skript med Jython eller C Python.

* Jython körs på hello JVM och internt kan anropas från Pig.
* C Python är en extern process så hello data från Pig på hello JVM skickas ut toohello skript som körs i en Python-process. hello utdata från hello Python-skriptet skickas tillbaka till Pig.

toospecify hello Python-tolken Använd `register` när du refererar till hello Python-skriptet. hello följande exempel registrera skript med Pig som `myfuncs`:

* **toouse Jython**:`register '/path/to/pigudf.py' using jython as myfuncs;`
* **toouse C Python**:`register '/path/to/pigudf.py' using streaming_python as myfuncs;`

> [!IMPORTANT]
> När du använder Jython hello sökvägen toohello pig_jython filen kan vara en lokal sökväg eller en WASB: / / sökväg. När du använder C Python, måste du referera en fil på hello lokalt filsystem för hello nod att du använder toosubmit hello Pig-jobbet.

När efter registreringen hello Pig Latin för det här exemplet är hello samma för både:

```pig
LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
LOG = FILTER LOGS by LINE is not null;
DETAILS = FOREACH LOG GENERATE myfuncs.create_structure(LINE);
DUMP DETAILS;
```

Här är det här exemplet har:

1. hello första raden läser in hello exempeldatafil `sample.log` till `LOGS`. Varje post som definierar också en `chararray`.
2. hello nästa rad filtrerar ut eventuella null-värden, lagra hello resultatet av hello i `LOG`.
3. Därefter den itererar över hello poster i `LOG` och använder `GENERATE` tooinvoke hello `create_structure` metod i hello Python/Jython skript som läses in som `myfuncs`. `LINE`är används toopass hello aktuella poster toohello funktion.
4. Slutligen hello utdata är dumpade tooSTDOUT med hello `DUMP` kommando. Detta kommando visar hello resultaten efter hello har slutförts.

### <a name="create-hello-pigudfpy-file"></a>Skapa hello pigudf.py fil

Skapa en textfil med namnet på din utvecklingsmiljö `pigudf.py`. Använd hello efter koden som hello innehållet i hello-fil:

<a name="streamingpy"></a>

```python
# Uncomment hello following if using C Python
#from pig_util import outputSchema

@outputSchema("log: {(date:chararray, time:chararray, classname:chararray, level:chararray, detail:chararray)}")
def create_structure(input):
    if (input.startswith('java.lang.Exception')):
        input = input[21:len(input)] + ' - java.lang.Exception'
    date, time, classname, level, detail = input.split(' ', 4)
    return date, time, classname, level, detail
```

Vi har definierat hello i hello Pig Latin exempel `LINE` indata som en chararray eftersom det finns inget konsekvent schema för hello indata. hello Python-skriptet omvandlar hello data till ett konsekvent schema för utdata.

1. Hej `@outputSchema` uttrycket definierar hello data som returneras tooPig hello format. I det här fallet har en **data egenskapsuppsättning**, vilket är en Pig-datatyp. hello egenskapsuppsättning innehåller hello följande fält, alla är chararray (strängar):

   * Date - hello datum hello loggposten skapades
   * tid - hello tid hello loggpost skapades
   * Klassnamn - hello klassen namnet hello transaktionen skapades för
   * nivå - hello Loggnivå
   * detaljer - utförlig information för hello loggpost

2. Därefter hello `def create_structure(input)` definierar hello-funktion som Pig skickar objekt till.

3. Hej exempeldata `sample.log`, främst överensstämmer toohello datum, tid, classname, nivå, och detaljerat schema som vi vill tooreturn. Men den innehåller några rader som börjar med `*java.lang.Exception*`. Dessa rader måste vara ändrade toomatch hello schemat. Hej `if` instruktionen kontrollerar för dem och sedan massageinstitut hello indata toomove hello `*java.lang.Exception*` sträng toohello syfte att hello data i nivå med vår utdata som förväntas schemat.

4. Därefter hello `split` kommandot är används toosplit hello data på hello första fyra blanksteg. hello utdata har tilldelats till `date`, `time`, `classname`, `level`, och `detail`.

5. Slutligen returneras hello värden tooPig.

När hello data returneras tooPig har ett konsekvent schema som definierats i hello `@outputSchema` instruktionen.

## <a name="running"></a>Ladda upp och köra hello-exempel

> [!IMPORTANT]
> Hej **SSH** stegen fungerar endast med en Linux-baserade HDInsight-kluster. Hej **PowerShell** stegen fungerar med en Linux- eller Windows-baserade HDInsight-kluster, men kräver en Windows-klient.

### <a name="ssh"></a>SSH

Mer information om hur du använder SSH finns [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

1. Använd `scp` toocopy hello filer tooyour HDInsight-kluster. Hej exempelvis följande kommando kopior hello filer tooa kluster med namnet **mycluster**.

    ```bash
    scp hiveudf.py pigudf.py myuser@mycluster-ssh.azurehdinsight.net:
    ```

2. Använda SSH tooconnect toohello kluster.

    ```bash
    ssh myuser@mycluster-ssh.azurehdinsight.net
    ```

3. Lägg till hello python filer överförs tidigare toohello WASB lagringen för hello klustret från hello SSH-sessionen.

    ```bash
    hdfs dfs -put hiveudf.py /hiveudf.py
    hdfs dfs -put pigudf.py /pigudf.py
    ```

Använd hello följande steg toorun hello Hive och Pig-jobb efter överföring hello-filer.

#### <a name="use-hello-hive-udf"></a>Använd hello Hive UDF

1. Använd hello `hive` kommandogränssnittet toostart hello hive. Du bör se en `hive>` fråga när hello shell har lästs in.

2. Ange följande fråga på hello hello `hive>` prompten:

   ```hive
   add file wasb:///hiveudf.py;
   SELECT TRANSFORM (clientid, devicemake, devicemodel)
       USING 'python hiveudf.py' AS
       (clientid string, phoneLabel string, phoneHash string)
   FROM hivesampletable
   ORDER BY clientid LIMIT 50;
   ```

3. När du har angett hello sista raden ska hello jobbet starta. När hello jobbet är slutfört, returnerar utdata liknande toohello följande exempel:

        100041    RIM 9650    d476f3687700442549a83fac4560c51c
        100041    RIM 9650    d476f3687700442549a83fac4560c51c
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9

#### <a name="use-hello-pig-udf"></a>Använd hello Pig UDF

1. Använd hello `pig` toostart hello kommandogränssnittet. Du ser en `grunt>` fråga när hello shell har lästs in.

2. Ange hello följa instruktionerna på hello `grunt>` prompten:

   ```pig
   Register wasb:///pigudf.py using jython as myfuncs;
   LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
   LOG = FILTER LOGS by LINE is not null;
   DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
   DUMP DETAILS;
   ```

3. När du har angett följande rad hello ska hello jobbet starta. När hello jobbet är slutfört, returnerar utdata liknande toohello följande data:

        ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
        ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
        ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
        ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
        ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

4. Använd `quit` tooexit hello Grunt shell och Använd sedan följande tooedit hello pigudf.py i hello lokala filsystemet hello:

    ```bash
    nano pigudf.py
    ```

5. En gång i Redigeraren för hello, ta bort kommentarerna hello följande rad genom att ta bort hello `#` tecknet från hello början av hello rad:

    ```bash
    #from pig_util import outputSchema
    ```

    När hello ändring har gjorts, använder du Ctrl + X tooexit hello editor. Välj Y och ange sedan toosave hello ändringar.

6. Använd hello `pig` kommandogränssnittet toostart hello igen. När du är på hello `grunt>` fråga använder hello följande toorun hello Python-skriptet med hello C Python-tolken.

   ```pig
   Register 'pigudf.py' using streaming_python as myfuncs;
   LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
   LOG = FILTER LOGS by LINE is not null;
   DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
   DUMP DETAILS;
   ```

    När jobbet har slutförts, bör du se hello samma utdata som när du tidigare körde hello skript med Jython.

### <a name="powershell-upload-hello-files"></a>PowerShell: Hello filer

Du kan använda PowerShell tooupload hello filer toohello HDInsight-servern. Använd följande Python skriptfiler tooupload hello hello:

> [!IMPORTANT] 
> hello stegen i det här avsnittet använder Azure PowerShell. Mer information om hur du använder Azure PowerShell finns [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).

```powershell
# Login tooyour Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Add-AzureRmAccount
}

# Get cluster info
$clusterName = Read-Host -Prompt "Enter hello HDInsight cluster name"
# Change hello path toomatch hello file location on your system
$pathToStreamingFile = "C:\path\to\hiveudf.py"
$pathToJythonFile = "C:\path\to\pigudf.py"

$clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroup = $clusterInfo.ResourceGroup
$storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
$container=$clusterInfo.DefaultStorageContainer
$storageAccountKey=(Get-AzureRmStorageAccountKey `
    -Name $storageAccountName `
-ResourceGroupName $resourceGroup)[0].Value

#Create a storage content and upload hello file
$context = New-AzureStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey $storageAccountKey

Set-AzureStorageBlobContent `
    -File $pathToStreamingFile `
    -Blob "hiveudf.py" `
    -Container $container `
    -Context $context

Set-AzureStorageBlobContent `
    -File $pathToJythonFile `
    -Blob "pigudf.py" `
    -Container $container `
    -Context $context
```
> [!IMPORTANT]
> Ändra hello `C:\path\to` toohello sökväg toohello filer på din utvecklingsmiljö.

Det här skriptet hämtar information om ditt HDInsight-kluster och extraherar hello-konto och nyckel för hello standardkontot för lagring och överföringar hello filer toohello rot hello behållare.

> [!NOTE]
> Mer information om överföringen av filer finns hello [överföra data för Hadoop-jobb i HDInsight](hdinsight-upload-data.md) dokumentet.

#### <a name="powershell-use-hello-hive-udf"></a>PowerShell: Hello Hive UDF använder du

PowerShell kan också vara används tooremotely kör Hive-frågor. Använd hello följande PowerShell-skriptet toorun en Hive-fråga som använder **hiveudf.py** skript:

> [!IMPORTANT]
> Innan du kör efterfrågar hello skript hello HTTPs/Admin kontoinformationen för ditt HDInsight-kluster.

```powershell
# Login tooyour Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Add-AzureRmAccount
}

# Get cluster info
$clusterName = Read-Host -Prompt "Enter hello HDInsight cluster name"
$creds=Get-Credential -Message "Enter hello login for hello cluster"

# If using a Windows-based HDInsight cluster, change hello USING statement to:
# "USING 'D:\Python27\python.exe hiveudf.py' AS " +
$HiveQuery = "add file wasb:///hiveudf.py;" +
                "SELECT TRANSFORM (clientid, devicemake, devicemodel) " +
                "USING 'python hiveudf.py' AS " +
                "(clientid string, phoneLabel string, phoneHash string) " +
                "FROM hivesampletable " +
                "ORDER BY clientid LIMIT 50;"

$jobDefinition = New-AzureRmHDInsightHiveJobDefinition `
    -Query $HiveQuery

$job = Start-AzureRmHDInsightJob `
    -ClusterName $clusterName `
    -JobDefinition $jobDefinition `
    -HttpCredential $creds
Write-Host "Wait for hello Hive job toocomplete ..." -ForegroundColor Green
Wait-AzureRmHDInsightJob `
    -JobId $job.JobId `
    -ClusterName $clusterName `
    -HttpCredential $creds
# Uncomment hello following toosee stderr output
# Get-AzureRmHDInsightJobOutput `
#   -Clustername $clusterName `
#   -JobId $job.JobId `
#   -HttpCredential $creds `
#   -DisplayOutputType StandardError
Write-Host "Display hello standard output ..." -ForegroundColor Green
Get-AzureRmHDInsightJobOutput `
    -Clustername $clusterName `
    -JobId $job.JobId `
    -HttpCredential $creds
```

Hej utdata för hello **Hive** jobbet ska visas liknande toohello följande exempel:

    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9

#### <a name="pig-jython"></a>Pig (Jython)

PowerShell kan också vara används toorun Pig Latin jobb. toorun ett Pig Latin-jobb som använder hello **pigudf.py** skript använder hello följande PowerShell-skript:

> [!NOTE]
> När du skickar in ett jobb som använder PowerShell via fjärranslutning, är det inte möjligt toouse C Python som hello tolk.

```powershell
# Login tooyour Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Add-AzureRmAccount
}

# Get cluster info
$clusterName = Read-Host -Prompt "Enter hello HDInsight cluster name"
$creds=Get-Credential -Message "Enter hello login for hello cluster"

$PigQuery = "Register wasb:///pigudf.py using jython as myfuncs;" +
            "LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);" +
            "LOG = FILTER LOGS by LINE is not null;" +
            "DETAILS = foreach LOG generate myfuncs.create_structure(LINE);" +
            "DUMP DETAILS;"

$jobDefinition = New-AzureRmHDInsightPigJobDefinition -Query $PigQuery

$job = Start-AzureRmHDInsightJob `
    -ClusterName $clusterName `
    -JobDefinition $jobDefinition `
    -HttpCredential $creds

Write-Host "Wait for hello Pig job toocomplete ..." -ForegroundColor Green
Wait-AzureRmHDInsightJob `
    -Job $job.JobId `
    -ClusterName $clusterName `
    -HttpCredential $creds
# Uncomment hello following toosee stderr output
# Get-AzureRmHDInsightJobOutput `
#    -Clustername $clusterName `
#    -JobId $job.JobId `
#    -HttpCredential $creds `
#    -DisplayOutputType StandardError
Write-Host "Display hello standard output ..." -ForegroundColor Green
Get-AzureRmHDInsightJobOutput `
    -Clustername $clusterName `
    -JobId $job.JobId `
    -HttpCredential $creds
```

Hej utdata för hello **Pig** jobbet ska visas liknande toohello följande data:

    ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
    ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
    ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
    ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
    ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

## <a name="troubleshooting"></a>Felsökning

### <a name="errors-when-running-jobs"></a>Fel när du kör jobb

När du kör hello hive-jobb kan stöta du på ett felmeddelande liknande toohello följande text:

    Caused by: org.apache.hadoop.hive.ql.metadata.HiveException: [Error 20001]: An error occurred while reading or writing tooyour custom script. It may have crashed with an error.

Det här problemet kan orsakas av hello radbrytningar i hello Python-fil. Många Windows redigerare standard toousing CRLF som hello linje slutar men förvänta LF om Linux-program.

Du kan använda följande PowerShell-instruktioner tooremove hello CR tecken innan du laddar upp hello filen tooHDInsight hello:

```powershell
$original_file ='c:\path\to\hiveudf.py'
$text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
[IO.File]::WriteAllText($original_file, $text)
```

### <a name="powershell-scripts"></a>PowerShell-skript

Både hello exempel PowerShell-skript används toorun hello exempel innehåller en kommenterad rad som visar Felutdata för hello jobb. Om du inte ser hello förväntades utdata för jobbet hello Avkommentera hello följande rad och se om hello felinformation uppstått problem.

```powershell
# Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

hello felinformation (STDERR) och hello resultatet hello jobbet (STDOUT) är också loggade tooyour HDInsight lagring.

| För det här jobbet... | Titta på de här filerna i hello blob-behållaren |
| --- | --- |
| Hive |/ HivePython/stderr<p>/ HivePython/stdout |
| Pig |/ PigPython/stderr<p>/ PigPython/stdout |

## <a name="next"></a>Nästa steg

Om du behöver tooload Python-moduler som inte är som standard finns [hur toodeploy modul-tooAzure HDInsight](http://blogs.msdn.com/b/benjguin/archive/2014/03/03/how-to-deploy-a-python-module-to-windows-azure-hdinsight.aspx).

Andra sätt toouse Pig finns Hive och toolearn om hur du använder MapReduce, i hello följande dokument:

* [Använda Hive med HDInsight](hdinsight-use-hive.md)
* [Använda Pig med HDInsight](hdinsight-use-pig.md)
* [Använda MapReduce med HDInsight](hdinsight-use-mapreduce.md)
