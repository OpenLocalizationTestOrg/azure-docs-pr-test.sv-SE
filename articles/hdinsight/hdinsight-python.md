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
# <a name="use-python-user-defined-functions-udf-with-hive-and-pig-in-hdinsight"></a><span data-ttu-id="c89eb-103">Använd användardefinierade Python funktioner (UDF) med Hive och Pig i HDInsight</span><span class="sxs-lookup"><span data-stu-id="c89eb-103">Use Python User Defined Functions (UDF) with Hive and Pig in HDInsight</span></span>

<span data-ttu-id="c89eb-104">Lär dig hur toouse Python användardefinierade funktioner (UDF) med Apache Hive och Pig i Hadoop på Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c89eb-104">Learn how toouse Python user-defined functions (UDF) with Apache Hive and Pig in Hadoop on Azure HDInsight.</span></span>

## <span data-ttu-id="c89eb-105"><a name="python"></a>Python på HDInsight</span><span class="sxs-lookup"><span data-stu-id="c89eb-105"><a name="python"></a>Python on HDInsight</span></span>

<span data-ttu-id="c89eb-106">Python2.7 installeras som standard på HDInsight 3.0 och senare.</span><span class="sxs-lookup"><span data-stu-id="c89eb-106">Python2.7 is installed by default on HDInsight 3.0 and later.</span></span> <span data-ttu-id="c89eb-107">Apache Hive kan användas med den här versionen av Python för bearbetning av dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="c89eb-107">Apache Hive can be used with this version of Python for stream processing.</span></span> <span data-ttu-id="c89eb-108">Bearbetning av dataströmmen använder STDOUT- och STDIN toopass data mellan Hive och hello UDF.</span><span class="sxs-lookup"><span data-stu-id="c89eb-108">Stream processing uses STDOUT and STDIN toopass data between Hive and hello UDF.</span></span>

<span data-ttu-id="c89eb-109">HDInsight innehåller också Jython, vilket är en Python-implementering skriven i Java.</span><span class="sxs-lookup"><span data-stu-id="c89eb-109">HDInsight also includes Jython, which is a Python implementation written in Java.</span></span> <span data-ttu-id="c89eb-110">Jython körs direkt på hello Java Virtual Machine och använder inte strömning.</span><span class="sxs-lookup"><span data-stu-id="c89eb-110">Jython runs directly on hello Java Virtual Machine and does not use streaming.</span></span> <span data-ttu-id="c89eb-111">Jython är hello rekommenderade Python-tolkning när du använder Python med Pig.</span><span class="sxs-lookup"><span data-stu-id="c89eb-111">Jython is hello recommended Python interpreter when using Python with Pig.</span></span>

> [!WARNING]
> <span data-ttu-id="c89eb-112">hello stegen i det här dokumentet att Hej följande förutsättningar:</span><span class="sxs-lookup"><span data-stu-id="c89eb-112">hello steps in this document make hello following assumptions:</span></span> 
>
> * <span data-ttu-id="c89eb-113">Skapar du hello Python-skript på din lokala utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="c89eb-113">You create hello Python scripts on your local development environment.</span></span>
> * <span data-ttu-id="c89eb-114">Du överför hello skript tooHDInsight med hjälp av antingen hello `scp` från en lokal Bash-session eller hello tillhandahålls PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="c89eb-114">You upload hello scripts tooHDInsight using either hello `scp` command from a local Bash session or hello provided PowerShell script.</span></span>
>
> <span data-ttu-id="c89eb-115">Om du vill toouse hello [Azure Cloud Shell (bash)](https://docs.microsoft.com/azure/cloud-shell/overview) Förhandsgranska toowork med HDInsight, måste du:</span><span class="sxs-lookup"><span data-stu-id="c89eb-115">If you want toouse hello [Azure Cloud Shell (bash)](https://docs.microsoft.com/azure/cloud-shell/overview) preview toowork with HDInsight, then you must:</span></span>
>
> * <span data-ttu-id="c89eb-116">Skapa hello skript i hello shell molnmiljö.</span><span class="sxs-lookup"><span data-stu-id="c89eb-116">Create hello scripts inside hello cloud shell environment.</span></span>
> * <span data-ttu-id="c89eb-117">Använd `scp` tooupload hello filer från hello molnet shell tooHDInsight.</span><span class="sxs-lookup"><span data-stu-id="c89eb-117">Use `scp` tooupload hello files from hello cloud shell tooHDInsight.</span></span>
> * <span data-ttu-id="c89eb-118">Använd `ssh` från hello molnet shell tooconnect tooHDInsight och kör hello exempel.</span><span class="sxs-lookup"><span data-stu-id="c89eb-118">Use `ssh` from hello cloud shell tooconnect tooHDInsight and run hello examples.</span></span>

## <span data-ttu-id="c89eb-119"><a name="hivepython"></a>Hive UDF</span><span class="sxs-lookup"><span data-stu-id="c89eb-119"><a name="hivepython"></a>Hive UDF</span></span>

<span data-ttu-id="c89eb-120">Python kan användas som en UDF från Hive via hello HiveQL `TRANSFORM` instruktionen.</span><span class="sxs-lookup"><span data-stu-id="c89eb-120">Python can be used as a UDF from Hive through hello HiveQL `TRANSFORM` statement.</span></span> <span data-ttu-id="c89eb-121">Till exempel hello följande HiveQL anropar hello `hiveudf.py` filen lagras i hello Azure standardkontot för lagring för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="c89eb-121">For example, hello following HiveQL invokes hello `hiveudf.py` file stored in hello default Azure Storage account for hello cluster.</span></span>

<span data-ttu-id="c89eb-122">**Linux-baserat HDInsight**</span><span class="sxs-lookup"><span data-stu-id="c89eb-122">**Linux-based HDInsight**</span></span>

```hiveql
add file wasb:///hiveudf.py;

SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'python hiveudf.py' AS
    (clientid string, phoneLable string, phoneHash string)
FROM hivesampletable
ORDER BY clientid LIMIT 50;
```

<span data-ttu-id="c89eb-123">**Windows-baserat HDInsight**</span><span class="sxs-lookup"><span data-stu-id="c89eb-123">**Windows-based HDInsight**</span></span>

```hiveql
add file wasb:///hiveudf.py;

SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'D:\Python27\python.exe hiveudf.py' AS
    (clientid string, phoneLable string, phoneHash string)
FROM hivesampletable
ORDER BY clientid LIMIT 50;
```

> [!NOTE]
> <span data-ttu-id="c89eb-124">På Windows-baserade HDInsight-kluster hello `USING` -satsen måste innehålla hello fullständig sökväg toopython.exe.</span><span class="sxs-lookup"><span data-stu-id="c89eb-124">On Windows-based HDInsight clusters, hello `USING` clause must specify hello full path toopython.exe.</span></span>

<span data-ttu-id="c89eb-125">Här är det här exemplet har:</span><span class="sxs-lookup"><span data-stu-id="c89eb-125">Here's what this example does:</span></span>

1. <span data-ttu-id="c89eb-126">Hej `add file` hello början av hello fil läggs hello `hiveudf.py` filen toohello distribuerad cache, så är den tillgänglig för alla noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="c89eb-126">hello `add file` statement at hello beginning of hello file adds hello `hiveudf.py` file toohello distributed cache, so it's accessible by all nodes in hello cluster.</span></span>
2. <span data-ttu-id="c89eb-127">Hej `SELECT TRANSFORM ... USING` instruktionen väljer data från hello `hivesampletable`.</span><span class="sxs-lookup"><span data-stu-id="c89eb-127">hello `SELECT TRANSFORM ... USING` statement selects data from hello `hivesampletable`.</span></span> <span data-ttu-id="c89eb-128">Skickar också hello clientid, devicemake och devicemodel värden toohello `hiveudf.py` skript.</span><span class="sxs-lookup"><span data-stu-id="c89eb-128">It also passes hello clientid, devicemake, and devicemodel values toohello `hiveudf.py` script.</span></span>
3. <span data-ttu-id="c89eb-129">Hej `AS` satsen beskrivs hello fält som returneras från `hiveudf.py`.</span><span class="sxs-lookup"><span data-stu-id="c89eb-129">hello `AS` clause describes hello fields returned from `hiveudf.py`.</span></span>

<a name="streamingpy"></a>

### <a name="create-hello-hiveudfpy-file"></a><span data-ttu-id="c89eb-130">Skapa hello hiveudf.py fil</span><span class="sxs-lookup"><span data-stu-id="c89eb-130">Create hello hiveudf.py file</span></span>


<span data-ttu-id="c89eb-131">Skapa en textfil med namnet på din utvecklingsmiljö `hiveudf.py`.</span><span class="sxs-lookup"><span data-stu-id="c89eb-131">On your development environment, create a text file named `hiveudf.py`.</span></span> <span data-ttu-id="c89eb-132">Använd hello efter koden som hello innehållet i hello-fil:</span><span class="sxs-lookup"><span data-stu-id="c89eb-132">Use hello following code as hello contents of hello file:</span></span>

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

<span data-ttu-id="c89eb-133">Det här skriptet utför hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="c89eb-133">This script performs hello following actions:</span></span>

1. <span data-ttu-id="c89eb-134">Läsa en rad med data från STDIN.</span><span class="sxs-lookup"><span data-stu-id="c89eb-134">Read a line of data from STDIN.</span></span>
2. <span data-ttu-id="c89eb-135">hello avslutande radmatningstecken tas bort med `string.strip(line, "\n ")`.</span><span class="sxs-lookup"><span data-stu-id="c89eb-135">hello trailing newline character is removed using `string.strip(line, "\n ")`.</span></span>
3. <span data-ttu-id="c89eb-136">När du gör dataströmmen bearbetning, innehåller en enda rad hello värden med ett tabbtecken mellan varje värde.</span><span class="sxs-lookup"><span data-stu-id="c89eb-136">When doing stream processing, a single line contains all hello values with a tab character between each value.</span></span> <span data-ttu-id="c89eb-137">Så `string.split(line, "\t")` kan vara används toosplit hello indata på varje flik, returnerar bara hello fält.</span><span class="sxs-lookup"><span data-stu-id="c89eb-137">So `string.split(line, "\t")` can be used toosplit hello input at each tab, returning just hello fields.</span></span>
4. <span data-ttu-id="c89eb-138">När bearbetningen är klar måste hello utdata skrivas tooSTDOUT som en enda rad med en flik mellan varje fält.</span><span class="sxs-lookup"><span data-stu-id="c89eb-138">When processing is complete, hello output must be written tooSTDOUT as a single line, with a tab between each field.</span></span> <span data-ttu-id="c89eb-139">Till exempel `print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])`.</span><span class="sxs-lookup"><span data-stu-id="c89eb-139">For example, `print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])`.</span></span>
5. <span data-ttu-id="c89eb-140">Hej `while` loop upprepas tills inga `line` läses.</span><span class="sxs-lookup"><span data-stu-id="c89eb-140">hello `while` loop repeats until no `line` is read.</span></span>

<span data-ttu-id="c89eb-141">hello skriptets utdata är en sammansättning av hello indatavärden för `devicemake` och `devicemodel`, och en hash av hello sammanfogas värde.</span><span class="sxs-lookup"><span data-stu-id="c89eb-141">hello script output is a concatenation of hello input values for `devicemake` and `devicemodel`, and a hash of hello concatenated value.</span></span>

<span data-ttu-id="c89eb-142">Se [köra hello exempel](#running) för hur toorun det här exemplet på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="c89eb-142">See [Running hello examples](#running) for how toorun this example on your HDInsight cluster.</span></span>

## <span data-ttu-id="c89eb-143"><a name="pigpython"></a>Pig UDF</span><span class="sxs-lookup"><span data-stu-id="c89eb-143"><a name="pigpython"></a>Pig UDF</span></span>

<span data-ttu-id="c89eb-144">Python-skriptet kan användas som en UDF från Pig via hello `GENERATE` instruktionen.</span><span class="sxs-lookup"><span data-stu-id="c89eb-144">A Python script can be used as a UDF from Pig through hello `GENERATE` statement.</span></span> <span data-ttu-id="c89eb-145">Du kan köra hello skript med Jython eller C Python.</span><span class="sxs-lookup"><span data-stu-id="c89eb-145">You can run hello script using either Jython or C Python.</span></span>

* <span data-ttu-id="c89eb-146">Jython körs på hello JVM och internt kan anropas från Pig.</span><span class="sxs-lookup"><span data-stu-id="c89eb-146">Jython runs on hello JVM, and can natively be called from Pig.</span></span>
* <span data-ttu-id="c89eb-147">C Python är en extern process så hello data från Pig på hello JVM skickas ut toohello skript som körs i en Python-process.</span><span class="sxs-lookup"><span data-stu-id="c89eb-147">C Python is an external process, so hello data from Pig on hello JVM is sent out toohello script running in a Python process.</span></span> <span data-ttu-id="c89eb-148">hello utdata från hello Python-skriptet skickas tillbaka till Pig.</span><span class="sxs-lookup"><span data-stu-id="c89eb-148">hello output of hello Python script is sent back into Pig.</span></span>

<span data-ttu-id="c89eb-149">toospecify hello Python-tolken Använd `register` när du refererar till hello Python-skriptet.</span><span class="sxs-lookup"><span data-stu-id="c89eb-149">toospecify hello Python interpreter, use `register` when referencing hello Python script.</span></span> <span data-ttu-id="c89eb-150">hello följande exempel registrera skript med Pig som `myfuncs`:</span><span class="sxs-lookup"><span data-stu-id="c89eb-150">hello following examples register scripts with Pig as `myfuncs`:</span></span>

* <span data-ttu-id="c89eb-151">**toouse Jython**:`register '/path/to/pigudf.py' using jython as myfuncs;`</span><span class="sxs-lookup"><span data-stu-id="c89eb-151">**toouse Jython**: `register '/path/to/pigudf.py' using jython as myfuncs;`</span></span>
* <span data-ttu-id="c89eb-152">**toouse C Python**:`register '/path/to/pigudf.py' using streaming_python as myfuncs;`</span><span class="sxs-lookup"><span data-stu-id="c89eb-152">**toouse C Python**: `register '/path/to/pigudf.py' using streaming_python as myfuncs;`</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c89eb-153">När du använder Jython hello sökvägen toohello pig_jython filen kan vara en lokal sökväg eller en WASB: / / sökväg.</span><span class="sxs-lookup"><span data-stu-id="c89eb-153">When using Jython, hello path toohello pig_jython file can be either a local path or a WASB:// path.</span></span> <span data-ttu-id="c89eb-154">När du använder C Python, måste du referera en fil på hello lokalt filsystem för hello nod att du använder toosubmit hello Pig-jobbet.</span><span class="sxs-lookup"><span data-stu-id="c89eb-154">However, when using C Python, you must reference a file on hello local file system of hello node that you are using toosubmit hello Pig job.</span></span>

<span data-ttu-id="c89eb-155">När efter registreringen hello Pig Latin för det här exemplet är hello samma för både:</span><span class="sxs-lookup"><span data-stu-id="c89eb-155">Once past registration, hello Pig Latin for this example is hello same for both:</span></span>

```pig
LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
LOG = FILTER LOGS by LINE is not null;
DETAILS = FOREACH LOG GENERATE myfuncs.create_structure(LINE);
DUMP DETAILS;
```

<span data-ttu-id="c89eb-156">Här är det här exemplet har:</span><span class="sxs-lookup"><span data-stu-id="c89eb-156">Here's what this example does:</span></span>

1. <span data-ttu-id="c89eb-157">hello första raden läser in hello exempeldatafil `sample.log` till `LOGS`.</span><span class="sxs-lookup"><span data-stu-id="c89eb-157">hello first line loads hello sample data file, `sample.log` into `LOGS`.</span></span> <span data-ttu-id="c89eb-158">Varje post som definierar också en `chararray`.</span><span class="sxs-lookup"><span data-stu-id="c89eb-158">It also defines each record as a `chararray`.</span></span>
2. <span data-ttu-id="c89eb-159">hello nästa rad filtrerar ut eventuella null-värden, lagra hello resultatet av hello i `LOG`.</span><span class="sxs-lookup"><span data-stu-id="c89eb-159">hello next line filters out any null values, storing hello result of hello operation into `LOG`.</span></span>
3. <span data-ttu-id="c89eb-160">Därefter den itererar över hello poster i `LOG` och använder `GENERATE` tooinvoke hello `create_structure` metod i hello Python/Jython skript som läses in som `myfuncs`.</span><span class="sxs-lookup"><span data-stu-id="c89eb-160">Next, it iterates over hello records in `LOG` and uses `GENERATE` tooinvoke hello `create_structure` method contained in hello Python/Jython script loaded as `myfuncs`.</span></span> <span data-ttu-id="c89eb-161">`LINE`är används toopass hello aktuella poster toohello funktion.</span><span class="sxs-lookup"><span data-stu-id="c89eb-161">`LINE` is used toopass hello current record toohello function.</span></span>
4. <span data-ttu-id="c89eb-162">Slutligen hello utdata är dumpade tooSTDOUT med hello `DUMP` kommando.</span><span class="sxs-lookup"><span data-stu-id="c89eb-162">Finally, hello outputs are dumped tooSTDOUT using hello `DUMP` command.</span></span> <span data-ttu-id="c89eb-163">Detta kommando visar hello resultaten efter hello har slutförts.</span><span class="sxs-lookup"><span data-stu-id="c89eb-163">This command displays hello results after hello operation completes.</span></span>

### <a name="create-hello-pigudfpy-file"></a><span data-ttu-id="c89eb-164">Skapa hello pigudf.py fil</span><span class="sxs-lookup"><span data-stu-id="c89eb-164">Create hello pigudf.py file</span></span>

<span data-ttu-id="c89eb-165">Skapa en textfil med namnet på din utvecklingsmiljö `pigudf.py`.</span><span class="sxs-lookup"><span data-stu-id="c89eb-165">On your development environment, create a text file named `pigudf.py`.</span></span> <span data-ttu-id="c89eb-166">Använd hello efter koden som hello innehållet i hello-fil:</span><span class="sxs-lookup"><span data-stu-id="c89eb-166">Use hello following code as hello contents of hello file:</span></span>

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

<span data-ttu-id="c89eb-167">Vi har definierat hello i hello Pig Latin exempel `LINE` indata som en chararray eftersom det finns inget konsekvent schema för hello indata.</span><span class="sxs-lookup"><span data-stu-id="c89eb-167">In hello Pig Latin example, we defined hello `LINE` input as a chararray because there is no consistent schema for hello input.</span></span> <span data-ttu-id="c89eb-168">hello Python-skriptet omvandlar hello data till ett konsekvent schema för utdata.</span><span class="sxs-lookup"><span data-stu-id="c89eb-168">hello Python script transforms hello data into a consistent schema for output.</span></span>

1. <span data-ttu-id="c89eb-169">Hej `@outputSchema` uttrycket definierar hello data som returneras tooPig hello format.</span><span class="sxs-lookup"><span data-stu-id="c89eb-169">hello `@outputSchema` statement defines hello format of hello data that is returned tooPig.</span></span> <span data-ttu-id="c89eb-170">I det här fallet har en **data egenskapsuppsättning**, vilket är en Pig-datatyp.</span><span class="sxs-lookup"><span data-stu-id="c89eb-170">In this case, it's a **data bag**, which is a Pig data type.</span></span> <span data-ttu-id="c89eb-171">hello egenskapsuppsättning innehåller hello följande fält, alla är chararray (strängar):</span><span class="sxs-lookup"><span data-stu-id="c89eb-171">hello bag contains hello following fields, all of which are chararray (strings):</span></span>

   * <span data-ttu-id="c89eb-172">Date - hello datum hello loggposten skapades</span><span class="sxs-lookup"><span data-stu-id="c89eb-172">date - hello date hello log entry was created</span></span>
   * <span data-ttu-id="c89eb-173">tid - hello tid hello loggpost skapades</span><span class="sxs-lookup"><span data-stu-id="c89eb-173">time - hello time hello log entry was created</span></span>
   * <span data-ttu-id="c89eb-174">Klassnamn - hello klassen namnet hello transaktionen skapades för</span><span class="sxs-lookup"><span data-stu-id="c89eb-174">classname - hello class name hello entry was created for</span></span>
   * <span data-ttu-id="c89eb-175">nivå - hello Loggnivå</span><span class="sxs-lookup"><span data-stu-id="c89eb-175">level - hello log level</span></span>
   * <span data-ttu-id="c89eb-176">detaljer - utförlig information för hello loggpost</span><span class="sxs-lookup"><span data-stu-id="c89eb-176">detail - verbose details for hello log entry</span></span>

2. <span data-ttu-id="c89eb-177">Därefter hello `def create_structure(input)` definierar hello-funktion som Pig skickar objekt till.</span><span class="sxs-lookup"><span data-stu-id="c89eb-177">Next, hello `def create_structure(input)` defines hello function that Pig passes line items to.</span></span>

3. <span data-ttu-id="c89eb-178">Hej exempeldata `sample.log`, främst överensstämmer toohello datum, tid, classname, nivå, och detaljerat schema som vi vill tooreturn.</span><span class="sxs-lookup"><span data-stu-id="c89eb-178">hello example data, `sample.log`, mostly conforms toohello date, time, classname, level, and detail schema we want tooreturn.</span></span> <span data-ttu-id="c89eb-179">Men den innehåller några rader som börjar med `*java.lang.Exception*`.</span><span class="sxs-lookup"><span data-stu-id="c89eb-179">However, it contains a few lines that begin with `*java.lang.Exception*`.</span></span> <span data-ttu-id="c89eb-180">Dessa rader måste vara ändrade toomatch hello schemat.</span><span class="sxs-lookup"><span data-stu-id="c89eb-180">These lines must be modified toomatch hello schema.</span></span> <span data-ttu-id="c89eb-181">Hej `if` instruktionen kontrollerar för dem och sedan massageinstitut hello indata toomove hello `*java.lang.Exception*` sträng toohello syfte att hello data i nivå med vår utdata som förväntas schemat.</span><span class="sxs-lookup"><span data-stu-id="c89eb-181">hello `if` statement checks for those, then massages hello input data toomove hello `*java.lang.Exception*` string toohello end, bringing hello data in-line with our expected output schema.</span></span>

4. <span data-ttu-id="c89eb-182">Därefter hello `split` kommandot är används toosplit hello data på hello första fyra blanksteg.</span><span class="sxs-lookup"><span data-stu-id="c89eb-182">Next, hello `split` command is used toosplit hello data at hello first four space characters.</span></span> <span data-ttu-id="c89eb-183">hello utdata har tilldelats till `date`, `time`, `classname`, `level`, och `detail`.</span><span class="sxs-lookup"><span data-stu-id="c89eb-183">hello output is assigned into `date`, `time`, `classname`, `level`, and `detail`.</span></span>

5. <span data-ttu-id="c89eb-184">Slutligen returneras hello värden tooPig.</span><span class="sxs-lookup"><span data-stu-id="c89eb-184">Finally, hello values are returned tooPig.</span></span>

<span data-ttu-id="c89eb-185">När hello data returneras tooPig har ett konsekvent schema som definierats i hello `@outputSchema` instruktionen.</span><span class="sxs-lookup"><span data-stu-id="c89eb-185">When hello data is returned tooPig, it has a consistent schema as defined in hello `@outputSchema` statement.</span></span>

## <span data-ttu-id="c89eb-186"><a name="running"></a>Ladda upp och köra hello-exempel</span><span class="sxs-lookup"><span data-stu-id="c89eb-186"><a name="running"></a>Upload and run hello examples</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c89eb-187">Hej **SSH** stegen fungerar endast med en Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="c89eb-187">hello **SSH** steps only work with a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="c89eb-188">Hej **PowerShell** stegen fungerar med en Linux- eller Windows-baserade HDInsight-kluster, men kräver en Windows-klient.</span><span class="sxs-lookup"><span data-stu-id="c89eb-188">hello **PowerShell** steps work with either a Linux or Windows-based HDInsight cluster, but require a Windows client.</span></span>

### <a name="ssh"></a><span data-ttu-id="c89eb-189">SSH</span><span class="sxs-lookup"><span data-stu-id="c89eb-189">SSH</span></span>

<span data-ttu-id="c89eb-190">Mer information om hur du använder SSH finns [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="c89eb-190">For more information on using SSH, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

1. <span data-ttu-id="c89eb-191">Använd `scp` toocopy hello filer tooyour HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="c89eb-191">Use `scp` toocopy hello files tooyour HDInsight cluster.</span></span> <span data-ttu-id="c89eb-192">Hej exempelvis följande kommando kopior hello filer tooa kluster med namnet **mycluster**.</span><span class="sxs-lookup"><span data-stu-id="c89eb-192">For example, hello following command copies hello files tooa cluster named **mycluster**.</span></span>

    ```bash
    scp hiveudf.py pigudf.py myuser@mycluster-ssh.azurehdinsight.net:
    ```

2. <span data-ttu-id="c89eb-193">Använda SSH tooconnect toohello kluster.</span><span class="sxs-lookup"><span data-stu-id="c89eb-193">Use SSH tooconnect toohello cluster.</span></span>

    ```bash
    ssh myuser@mycluster-ssh.azurehdinsight.net
    ```

3. <span data-ttu-id="c89eb-194">Lägg till hello python filer överförs tidigare toohello WASB lagringen för hello klustret från hello SSH-sessionen.</span><span class="sxs-lookup"><span data-stu-id="c89eb-194">From hello SSH session, add hello python files uploaded previously toohello WASB storage for hello cluster.</span></span>

    ```bash
    hdfs dfs -put hiveudf.py /hiveudf.py
    hdfs dfs -put pigudf.py /pigudf.py
    ```

<span data-ttu-id="c89eb-195">Använd hello följande steg toorun hello Hive och Pig-jobb efter överföring hello-filer.</span><span class="sxs-lookup"><span data-stu-id="c89eb-195">After uploading hello files, use hello following steps toorun hello Hive and Pig jobs.</span></span>

#### <a name="use-hello-hive-udf"></a><span data-ttu-id="c89eb-196">Använd hello Hive UDF</span><span class="sxs-lookup"><span data-stu-id="c89eb-196">Use hello Hive UDF</span></span>

1. <span data-ttu-id="c89eb-197">Använd hello `hive` kommandogränssnittet toostart hello hive.</span><span class="sxs-lookup"><span data-stu-id="c89eb-197">Use hello `hive` command toostart hello hive shell.</span></span> <span data-ttu-id="c89eb-198">Du bör se en `hive>` fråga när hello shell har lästs in.</span><span class="sxs-lookup"><span data-stu-id="c89eb-198">You should see a `hive>` prompt once hello shell has loaded.</span></span>

2. <span data-ttu-id="c89eb-199">Ange följande fråga på hello hello `hive>` prompten:</span><span class="sxs-lookup"><span data-stu-id="c89eb-199">Enter hello following query at hello `hive>` prompt:</span></span>

   ```hive
   add file wasb:///hiveudf.py;
   SELECT TRANSFORM (clientid, devicemake, devicemodel)
       USING 'python hiveudf.py' AS
       (clientid string, phoneLabel string, phoneHash string)
   FROM hivesampletable
   ORDER BY clientid LIMIT 50;
   ```

3. <span data-ttu-id="c89eb-200">När du har angett hello sista raden ska hello jobbet starta.</span><span class="sxs-lookup"><span data-stu-id="c89eb-200">After entering hello last line, hello job should start.</span></span> <span data-ttu-id="c89eb-201">När hello jobbet är slutfört, returnerar utdata liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="c89eb-201">Once hello job completes, it returns output similar toohello following example:</span></span>

        100041    RIM 9650    d476f3687700442549a83fac4560c51c
        100041    RIM 9650    d476f3687700442549a83fac4560c51c
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9

#### <a name="use-hello-pig-udf"></a><span data-ttu-id="c89eb-202">Använd hello Pig UDF</span><span class="sxs-lookup"><span data-stu-id="c89eb-202">Use hello Pig UDF</span></span>

1. <span data-ttu-id="c89eb-203">Använd hello `pig` toostart hello kommandogränssnittet.</span><span class="sxs-lookup"><span data-stu-id="c89eb-203">Use hello `pig` command toostart hello shell.</span></span> <span data-ttu-id="c89eb-204">Du ser en `grunt>` fråga när hello shell har lästs in.</span><span class="sxs-lookup"><span data-stu-id="c89eb-204">You see a `grunt>` prompt once hello shell has loaded.</span></span>

2. <span data-ttu-id="c89eb-205">Ange hello följa instruktionerna på hello `grunt>` prompten:</span><span class="sxs-lookup"><span data-stu-id="c89eb-205">Enter hello following statements at hello `grunt>` prompt:</span></span>

   ```pig
   Register wasb:///pigudf.py using jython as myfuncs;
   LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
   LOG = FILTER LOGS by LINE is not null;
   DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
   DUMP DETAILS;
   ```

3. <span data-ttu-id="c89eb-206">När du har angett följande rad hello ska hello jobbet starta.</span><span class="sxs-lookup"><span data-stu-id="c89eb-206">After entering hello following line, hello job should start.</span></span> <span data-ttu-id="c89eb-207">När hello jobbet är slutfört, returnerar utdata liknande toohello följande data:</span><span class="sxs-lookup"><span data-stu-id="c89eb-207">Once hello job completes, it returns output similar toohello following data:</span></span>

        ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
        ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
        ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
        ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
        ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

4. <span data-ttu-id="c89eb-208">Använd `quit` tooexit hello Grunt shell och Använd sedan följande tooedit hello pigudf.py i hello lokala filsystemet hello:</span><span class="sxs-lookup"><span data-stu-id="c89eb-208">Use `quit` tooexit hello Grunt shell, and then use hello following tooedit hello pigudf.py file on hello local file system:</span></span>

    ```bash
    nano pigudf.py
    ```

5. <span data-ttu-id="c89eb-209">En gång i Redigeraren för hello, ta bort kommentarerna hello följande rad genom att ta bort hello `#` tecknet från hello början av hello rad:</span><span class="sxs-lookup"><span data-stu-id="c89eb-209">Once in hello editor, uncomment hello following line by removing hello `#` character from hello beginning of hello line:</span></span>

    ```bash
    #from pig_util import outputSchema
    ```

    <span data-ttu-id="c89eb-210">När hello ändring har gjorts, använder du Ctrl + X tooexit hello editor.</span><span class="sxs-lookup"><span data-stu-id="c89eb-210">Once hello change has been made, use Ctrl+X tooexit hello editor.</span></span> <span data-ttu-id="c89eb-211">Välj Y och ange sedan toosave hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="c89eb-211">Select Y, and then enter toosave hello changes.</span></span>

6. <span data-ttu-id="c89eb-212">Använd hello `pig` kommandogränssnittet toostart hello igen.</span><span class="sxs-lookup"><span data-stu-id="c89eb-212">Use hello `pig` command toostart hello shell again.</span></span> <span data-ttu-id="c89eb-213">När du är på hello `grunt>` fråga använder hello följande toorun hello Python-skriptet med hello C Python-tolken.</span><span class="sxs-lookup"><span data-stu-id="c89eb-213">Once you are at hello `grunt>` prompt, use hello following toorun hello Python script using hello C Python interpreter.</span></span>

   ```pig
   Register 'pigudf.py' using streaming_python as myfuncs;
   LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
   LOG = FILTER LOGS by LINE is not null;
   DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
   DUMP DETAILS;
   ```

    <span data-ttu-id="c89eb-214">När jobbet har slutförts, bör du se hello samma utdata som när du tidigare körde hello skript med Jython.</span><span class="sxs-lookup"><span data-stu-id="c89eb-214">Once this job completes, you should see hello same output as when you previously ran hello script using Jython.</span></span>

### <a name="powershell-upload-hello-files"></a><span data-ttu-id="c89eb-215">PowerShell: Hello filer</span><span class="sxs-lookup"><span data-stu-id="c89eb-215">PowerShell: Upload hello files</span></span>

<span data-ttu-id="c89eb-216">Du kan använda PowerShell tooupload hello filer toohello HDInsight-servern.</span><span class="sxs-lookup"><span data-stu-id="c89eb-216">You can use PowerShell tooupload hello files toohello HDInsight server.</span></span> <span data-ttu-id="c89eb-217">Använd följande Python skriptfiler tooupload hello hello:</span><span class="sxs-lookup"><span data-stu-id="c89eb-217">Use hello following script tooupload hello Python files:</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="c89eb-218">hello stegen i det här avsnittet använder Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c89eb-218">hello steps in this section use Azure PowerShell.</span></span> <span data-ttu-id="c89eb-219">Mer information om hur du använder Azure PowerShell finns [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c89eb-219">For more information on using Azure PowerShell, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

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
> <span data-ttu-id="c89eb-220">Ändra hello `C:\path\to` toohello sökväg toohello filer på din utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="c89eb-220">Change hello `C:\path\to` value toohello path toohello files on your development environment.</span></span>

<span data-ttu-id="c89eb-221">Det här skriptet hämtar information om ditt HDInsight-kluster och extraherar hello-konto och nyckel för hello standardkontot för lagring och överföringar hello filer toohello rot hello behållare.</span><span class="sxs-lookup"><span data-stu-id="c89eb-221">This script retrieves information for your HDInsight cluster, then extracts hello account and key for hello default storage account, and uploads hello files toohello root of hello container.</span></span>

> [!NOTE]
> <span data-ttu-id="c89eb-222">Mer information om överföringen av filer finns hello [överföra data för Hadoop-jobb i HDInsight](hdinsight-upload-data.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="c89eb-222">For more information on uploading files, see hello [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md) document.</span></span>

#### <a name="powershell-use-hello-hive-udf"></a><span data-ttu-id="c89eb-223">PowerShell: Hello Hive UDF använder du</span><span class="sxs-lookup"><span data-stu-id="c89eb-223">PowerShell: Use hello Hive UDF</span></span>

<span data-ttu-id="c89eb-224">PowerShell kan också vara används tooremotely kör Hive-frågor.</span><span class="sxs-lookup"><span data-stu-id="c89eb-224">PowerShell can also be used tooremotely run Hive queries.</span></span> <span data-ttu-id="c89eb-225">Använd hello följande PowerShell-skriptet toorun en Hive-fråga som använder **hiveudf.py** skript:</span><span class="sxs-lookup"><span data-stu-id="c89eb-225">Use hello following PowerShell script toorun a Hive query that uses **hiveudf.py** script:</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c89eb-226">Innan du kör efterfrågar hello skript hello HTTPs/Admin kontoinformationen för ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="c89eb-226">Before running, hello script prompts you for hello HTTPs/Admin account information for your HDInsight cluster.</span></span>

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

<span data-ttu-id="c89eb-227">Hej utdata för hello **Hive** jobbet ska visas liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="c89eb-227">hello output for hello **Hive** job should appear similar toohello following example:</span></span>

    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9

#### <a name="pig-jython"></a><span data-ttu-id="c89eb-228">Pig (Jython)</span><span class="sxs-lookup"><span data-stu-id="c89eb-228">Pig (Jython)</span></span>

<span data-ttu-id="c89eb-229">PowerShell kan också vara används toorun Pig Latin jobb.</span><span class="sxs-lookup"><span data-stu-id="c89eb-229">PowerShell can also be used toorun Pig Latin jobs.</span></span> <span data-ttu-id="c89eb-230">toorun ett Pig Latin-jobb som använder hello **pigudf.py** skript använder hello följande PowerShell-skript:</span><span class="sxs-lookup"><span data-stu-id="c89eb-230">toorun a Pig Latin job that uses hello **pigudf.py** script, use hello following PowerShell script:</span></span>

> [!NOTE]
> <span data-ttu-id="c89eb-231">När du skickar in ett jobb som använder PowerShell via fjärranslutning, är det inte möjligt toouse C Python som hello tolk.</span><span class="sxs-lookup"><span data-stu-id="c89eb-231">When remotely submitting a job using PowerShell, it is not possible toouse C Python as hello interpreter.</span></span>

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

<span data-ttu-id="c89eb-232">Hej utdata för hello **Pig** jobbet ska visas liknande toohello följande data:</span><span class="sxs-lookup"><span data-stu-id="c89eb-232">hello output for hello **Pig** job should appear similar toohello following data:</span></span>

    ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
    ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
    ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
    ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
    ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

## <span data-ttu-id="c89eb-233"><a name="troubleshooting"></a>Felsökning</span><span class="sxs-lookup"><span data-stu-id="c89eb-233"><a name="troubleshooting"></a>Troubleshooting</span></span>

### <a name="errors-when-running-jobs"></a><span data-ttu-id="c89eb-234">Fel när du kör jobb</span><span class="sxs-lookup"><span data-stu-id="c89eb-234">Errors when running jobs</span></span>

<span data-ttu-id="c89eb-235">När du kör hello hive-jobb kan stöta du på ett felmeddelande liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="c89eb-235">When running hello hive job, you may encounter an error similar toohello following text:</span></span>

    Caused by: org.apache.hadoop.hive.ql.metadata.HiveException: [Error 20001]: An error occurred while reading or writing tooyour custom script. It may have crashed with an error.

<span data-ttu-id="c89eb-236">Det här problemet kan orsakas av hello radbrytningar i hello Python-fil.</span><span class="sxs-lookup"><span data-stu-id="c89eb-236">This problem may be caused by hello line endings in hello Python file.</span></span> <span data-ttu-id="c89eb-237">Många Windows redigerare standard toousing CRLF som hello linje slutar men förvänta LF om Linux-program.</span><span class="sxs-lookup"><span data-stu-id="c89eb-237">Many Windows editors default toousing CRLF as hello line ending, but Linux applications usually expect LF.</span></span>

<span data-ttu-id="c89eb-238">Du kan använda följande PowerShell-instruktioner tooremove hello CR tecken innan du laddar upp hello filen tooHDInsight hello:</span><span class="sxs-lookup"><span data-stu-id="c89eb-238">You can use hello following PowerShell statements tooremove hello CR characters before uploading hello file tooHDInsight:</span></span>

```powershell
$original_file ='c:\path\to\hiveudf.py'
$text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
[IO.File]::WriteAllText($original_file, $text)
```

### <a name="powershell-scripts"></a><span data-ttu-id="c89eb-239">PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="c89eb-239">PowerShell scripts</span></span>

<span data-ttu-id="c89eb-240">Både hello exempel PowerShell-skript används toorun hello exempel innehåller en kommenterad rad som visar Felutdata för hello jobb.</span><span class="sxs-lookup"><span data-stu-id="c89eb-240">Both of hello example PowerShell scripts used toorun hello examples contain a commented line that displays error output for hello job.</span></span> <span data-ttu-id="c89eb-241">Om du inte ser hello förväntades utdata för jobbet hello Avkommentera hello följande rad och se om hello felinformation uppstått problem.</span><span class="sxs-lookup"><span data-stu-id="c89eb-241">If you are not seeing hello expected output for hello job, uncomment hello following line and see if hello error information indicates a problem.</span></span>

```powershell
# Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

<span data-ttu-id="c89eb-242">hello felinformation (STDERR) och hello resultatet hello jobbet (STDOUT) är också loggade tooyour HDInsight lagring.</span><span class="sxs-lookup"><span data-stu-id="c89eb-242">hello error information (STDERR) and hello result of hello job (STDOUT) are also logged tooyour HDInsight storage.</span></span>

| <span data-ttu-id="c89eb-243">För det här jobbet...</span><span class="sxs-lookup"><span data-stu-id="c89eb-243">For this job...</span></span> | <span data-ttu-id="c89eb-244">Titta på de här filerna i hello blob-behållaren</span><span class="sxs-lookup"><span data-stu-id="c89eb-244">Look at these files in hello blob container</span></span> |
| --- | --- |
| <span data-ttu-id="c89eb-245">Hive</span><span class="sxs-lookup"><span data-stu-id="c89eb-245">Hive</span></span> |<span data-ttu-id="c89eb-246">/ HivePython/stderr</span><span class="sxs-lookup"><span data-stu-id="c89eb-246">/HivePython/stderr</span></span><p><span data-ttu-id="c89eb-247">/ HivePython/stdout</span><span class="sxs-lookup"><span data-stu-id="c89eb-247">/HivePython/stdout</span></span> |
| <span data-ttu-id="c89eb-248">Pig</span><span class="sxs-lookup"><span data-stu-id="c89eb-248">Pig</span></span> |<span data-ttu-id="c89eb-249">/ PigPython/stderr</span><span class="sxs-lookup"><span data-stu-id="c89eb-249">/PigPython/stderr</span></span><p><span data-ttu-id="c89eb-250">/ PigPython/stdout</span><span class="sxs-lookup"><span data-stu-id="c89eb-250">/PigPython/stdout</span></span> |

## <span data-ttu-id="c89eb-251"><a name="next"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c89eb-251"><a name="next"></a>Next steps</span></span>

<span data-ttu-id="c89eb-252">Om du behöver tooload Python-moduler som inte är som standard finns [hur toodeploy modul-tooAzure HDInsight](http://blogs.msdn.com/b/benjguin/archive/2014/03/03/how-to-deploy-a-python-module-to-windows-azure-hdinsight.aspx).</span><span class="sxs-lookup"><span data-stu-id="c89eb-252">If you need tooload Python modules that aren't provided by default, see [How toodeploy a module tooAzure HDInsight](http://blogs.msdn.com/b/benjguin/archive/2014/03/03/how-to-deploy-a-python-module-to-windows-azure-hdinsight.aspx).</span></span>

<span data-ttu-id="c89eb-253">Andra sätt toouse Pig finns Hive och toolearn om hur du använder MapReduce, i hello följande dokument:</span><span class="sxs-lookup"><span data-stu-id="c89eb-253">For other ways toouse Pig, Hive, and toolearn about using MapReduce, see hello following documents:</span></span>

* [<span data-ttu-id="c89eb-254">Använda Hive med HDInsight</span><span class="sxs-lookup"><span data-stu-id="c89eb-254">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="c89eb-255">Använda Pig med HDInsight</span><span class="sxs-lookup"><span data-stu-id="c89eb-255">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="c89eb-256">Använda MapReduce med HDInsight</span><span class="sxs-lookup"><span data-stu-id="c89eb-256">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)
