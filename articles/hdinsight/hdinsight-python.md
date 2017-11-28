---
title: Python UDF med Apache Hive och svin - Azure HDInsight | Microsoft Docs
description: "Lär dig hur du använder Python användaren definierat funktioner (UDF) från Hive och Pig i HDInsight Hadoop-teknikstacken på Azure."
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
ms.openlocfilehash: 9b67ded05a52f1e68580434667495cf6cf939871
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="use-python-user-defined-functions-udf-with-hive-and-pig-in-hdinsight"></a><span data-ttu-id="97eb3-103">Använd användardefinierade Python funktioner (UDF) med Hive och Pig i HDInsight</span><span class="sxs-lookup"><span data-stu-id="97eb3-103">Use Python User Defined Functions (UDF) with Hive and Pig in HDInsight</span></span>

<span data-ttu-id="97eb3-104">Lär dig hur du använder Python användardefinierade funktioner (UDF) med Apache Hive och Pig i Hadoop på Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="97eb3-104">Learn how to use Python user-defined functions (UDF) with Apache Hive and Pig in Hadoop on Azure HDInsight.</span></span>

## <span data-ttu-id="97eb3-105"><a name="python"></a>Python på HDInsight</span><span class="sxs-lookup"><span data-stu-id="97eb3-105"><a name="python"></a>Python on HDInsight</span></span>

<span data-ttu-id="97eb3-106">Python2.7 installeras som standard på HDInsight 3.0 och senare.</span><span class="sxs-lookup"><span data-stu-id="97eb3-106">Python2.7 is installed by default on HDInsight 3.0 and later.</span></span> <span data-ttu-id="97eb3-107">Apache Hive kan användas med den här versionen av Python för bearbetning av dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="97eb3-107">Apache Hive can be used with this version of Python for stream processing.</span></span> <span data-ttu-id="97eb3-108">Dataströmmen bearbetning använder STDOUT- och STDIN för att överföra data mellan Hive och UDF-filen.</span><span class="sxs-lookup"><span data-stu-id="97eb3-108">Stream processing uses STDOUT and STDIN to pass data between Hive and the UDF.</span></span>

<span data-ttu-id="97eb3-109">HDInsight innehåller också Jython, vilket är en Python-implementering skriven i Java.</span><span class="sxs-lookup"><span data-stu-id="97eb3-109">HDInsight also includes Jython, which is a Python implementation written in Java.</span></span> <span data-ttu-id="97eb3-110">Jython körs direkt på Java Virtual Machine och använder inte strömning.</span><span class="sxs-lookup"><span data-stu-id="97eb3-110">Jython runs directly on the Java Virtual Machine and does not use streaming.</span></span> <span data-ttu-id="97eb3-111">Jython är den rekommenderade Python-tolkning när du använder Python med Pig.</span><span class="sxs-lookup"><span data-stu-id="97eb3-111">Jython is the recommended Python interpreter when using Python with Pig.</span></span>

> [!WARNING]
> <span data-ttu-id="97eb3-112">Stegen i det här dokumentet gör följande antaganden:</span><span class="sxs-lookup"><span data-stu-id="97eb3-112">The steps in this document make the following assumptions:</span></span> 
>
> * <span data-ttu-id="97eb3-113">Du kan skapa Python-skript på din lokala utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="97eb3-113">You create the Python scripts on your local development environment.</span></span>
> * <span data-ttu-id="97eb3-114">Du överför skript till HDInsight med antingen den `scp` från en lokal Bash-session eller PowerShell-skriptet.</span><span class="sxs-lookup"><span data-stu-id="97eb3-114">You upload the scripts to HDInsight using either the `scp` command from a local Bash session or the provided PowerShell script.</span></span>
>
> <span data-ttu-id="97eb3-115">Om du vill använda den [Azure Cloud Shell (bash)](https://docs.microsoft.com/azure/cloud-shell/overview) Förhandsgranska för att arbeta med HDInsight, måste du:</span><span class="sxs-lookup"><span data-stu-id="97eb3-115">If you want to use the [Azure Cloud Shell (bash)](https://docs.microsoft.com/azure/cloud-shell/overview) preview to work with HDInsight, then you must:</span></span>
>
> * <span data-ttu-id="97eb3-116">Skapa skript i molnet shell-miljö.</span><span class="sxs-lookup"><span data-stu-id="97eb3-116">Create the scripts inside the cloud shell environment.</span></span>
> * <span data-ttu-id="97eb3-117">Använd `scp` att överföra filer från molnet shell till HDInsight.</span><span class="sxs-lookup"><span data-stu-id="97eb3-117">Use `scp` to upload the files from the cloud shell to HDInsight.</span></span>
> * <span data-ttu-id="97eb3-118">Använd `ssh` från molnet shell att ansluta till HDInsight och köra exemplen.</span><span class="sxs-lookup"><span data-stu-id="97eb3-118">Use `ssh` from the cloud shell to connect to HDInsight and run the examples.</span></span>

## <span data-ttu-id="97eb3-119"><a name="hivepython"></a>Hive UDF</span><span class="sxs-lookup"><span data-stu-id="97eb3-119"><a name="hivepython"></a>Hive UDF</span></span>

<span data-ttu-id="97eb3-120">Python kan användas som en UDF från Hive via HiveQL `TRANSFORM` instruktionen.</span><span class="sxs-lookup"><span data-stu-id="97eb3-120">Python can be used as a UDF from Hive through the HiveQL `TRANSFORM` statement.</span></span> <span data-ttu-id="97eb3-121">Till exempel följande HiveQL anropar den `hiveudf.py` filen lagras i Azure Storage standardkontot för klustret.</span><span class="sxs-lookup"><span data-stu-id="97eb3-121">For example, the following HiveQL invokes the `hiveudf.py` file stored in the default Azure Storage account for the cluster.</span></span>

<span data-ttu-id="97eb3-122">**Linux-baserat HDInsight**</span><span class="sxs-lookup"><span data-stu-id="97eb3-122">**Linux-based HDInsight**</span></span>

```hiveql
add file wasb:///hiveudf.py;

SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'python hiveudf.py' AS
    (clientid string, phoneLable string, phoneHash string)
FROM hivesampletable
ORDER BY clientid LIMIT 50;
```

<span data-ttu-id="97eb3-123">**Windows-baserat HDInsight**</span><span class="sxs-lookup"><span data-stu-id="97eb3-123">**Windows-based HDInsight**</span></span>

```hiveql
add file wasb:///hiveudf.py;

SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'D:\Python27\python.exe hiveudf.py' AS
    (clientid string, phoneLable string, phoneHash string)
FROM hivesampletable
ORDER BY clientid LIMIT 50;
```

> [!NOTE]
> <span data-ttu-id="97eb3-124">På Windows-baserade HDInsight-kluster i `USING` sats måste ange den fullständiga sökvägen till python.exe.</span><span class="sxs-lookup"><span data-stu-id="97eb3-124">On Windows-based HDInsight clusters, the `USING` clause must specify the full path to python.exe.</span></span>

<span data-ttu-id="97eb3-125">Här är det här exemplet har:</span><span class="sxs-lookup"><span data-stu-id="97eb3-125">Here's what this example does:</span></span>

1. <span data-ttu-id="97eb3-126">Den `add file` i början av filen läggs den `hiveudf.py` filen till det distribuerade cacheminnet, så är den tillgänglig för alla noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="97eb3-126">The `add file` statement at the beginning of the file adds the `hiveudf.py` file to the distributed cache, so it's accessible by all nodes in the cluster.</span></span>
2. <span data-ttu-id="97eb3-127">Den `SELECT TRANSFORM ... USING` uttrycket hämtar data från den `hivesampletable`.</span><span class="sxs-lookup"><span data-stu-id="97eb3-127">The `SELECT TRANSFORM ... USING` statement selects data from the `hivesampletable`.</span></span> <span data-ttu-id="97eb3-128">Skickar också värden för clientid, devicemake och devicemodel till den `hiveudf.py` skript.</span><span class="sxs-lookup"><span data-stu-id="97eb3-128">It also passes the clientid, devicemake, and devicemodel values to the `hiveudf.py` script.</span></span>
3. <span data-ttu-id="97eb3-129">Den `AS` satsen beskriver de fält som returneras från `hiveudf.py`.</span><span class="sxs-lookup"><span data-stu-id="97eb3-129">The `AS` clause describes the fields returned from `hiveudf.py`.</span></span>

<a name="streamingpy"></a>

### <a name="create-the-hiveudfpy-file"></a><span data-ttu-id="97eb3-130">Skapa filen hiveudf.py</span><span class="sxs-lookup"><span data-stu-id="97eb3-130">Create the hiveudf.py file</span></span>


<span data-ttu-id="97eb3-131">Skapa en textfil med namnet på din utvecklingsmiljö `hiveudf.py`.</span><span class="sxs-lookup"><span data-stu-id="97eb3-131">On your development environment, create a text file named `hiveudf.py`.</span></span> <span data-ttu-id="97eb3-132">Använd följande kod som innehållet i filen:</span><span class="sxs-lookup"><span data-stu-id="97eb3-132">Use the following code as the contents of the file:</span></span>

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

<span data-ttu-id="97eb3-133">Det här skriptet utförs följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="97eb3-133">This script performs the following actions:</span></span>

1. <span data-ttu-id="97eb3-134">Läsa en rad med data från STDIN.</span><span class="sxs-lookup"><span data-stu-id="97eb3-134">Read a line of data from STDIN.</span></span>
2. <span data-ttu-id="97eb3-135">Avslutande radmatningstecken tas bort med `string.strip(line, "\n ")`.</span><span class="sxs-lookup"><span data-stu-id="97eb3-135">The trailing newline character is removed using `string.strip(line, "\n ")`.</span></span>
3. <span data-ttu-id="97eb3-136">När du utför bearbetning innehåller alla värdena med tabbtecken mellan varje värde i en enda rad.</span><span class="sxs-lookup"><span data-stu-id="97eb3-136">When doing stream processing, a single line contains all the values with a tab character between each value.</span></span> <span data-ttu-id="97eb3-137">Så `string.split(line, "\t")` kan användas för att dela indata på varje flik, returnerar bara fält.</span><span class="sxs-lookup"><span data-stu-id="97eb3-137">So `string.split(line, "\t")` can be used to split the input at each tab, returning just the fields.</span></span>
4. <span data-ttu-id="97eb3-138">När bearbetningen är klar måste utdata skrivs STDOUT som en enda rad med en flik mellan varje fält.</span><span class="sxs-lookup"><span data-stu-id="97eb3-138">When processing is complete, the output must be written to STDOUT as a single line, with a tab between each field.</span></span> <span data-ttu-id="97eb3-139">Till exempel `print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])`.</span><span class="sxs-lookup"><span data-stu-id="97eb3-139">For example, `print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])`.</span></span>
5. <span data-ttu-id="97eb3-140">Den `while` loop upprepas tills inga `line` läses.</span><span class="sxs-lookup"><span data-stu-id="97eb3-140">The `while` loop repeats until no `line` is read.</span></span>

<span data-ttu-id="97eb3-141">Utdata från skriptet är en sammansättning av indatavärden för `devicemake` och `devicemodel`, och en hash av sammanfogad värdet.</span><span class="sxs-lookup"><span data-stu-id="97eb3-141">The script output is a concatenation of the input values for `devicemake` and `devicemodel`, and a hash of the concatenated value.</span></span>

<span data-ttu-id="97eb3-142">Se [köra exemplen](#running) för hur du kör det här exemplet på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="97eb3-142">See [Running the examples](#running) for how to run this example on your HDInsight cluster.</span></span>

## <span data-ttu-id="97eb3-143"><a name="pigpython"></a>Pig UDF</span><span class="sxs-lookup"><span data-stu-id="97eb3-143"><a name="pigpython"></a>Pig UDF</span></span>

<span data-ttu-id="97eb3-144">Python-skriptet kan användas som en UDF från Pig via den `GENERATE` instruktionen.</span><span class="sxs-lookup"><span data-stu-id="97eb3-144">A Python script can be used as a UDF from Pig through the `GENERATE` statement.</span></span> <span data-ttu-id="97eb3-145">Du kan köra skriptet med Jython eller C Python.</span><span class="sxs-lookup"><span data-stu-id="97eb3-145">You can run the script using either Jython or C Python.</span></span>

* <span data-ttu-id="97eb3-146">Jython körs på JVM och internt kan anropas från Pig.</span><span class="sxs-lookup"><span data-stu-id="97eb3-146">Jython runs on the JVM, and can natively be called from Pig.</span></span>
* <span data-ttu-id="97eb3-147">C Python är en extern process så att data från Pig på JVM skickas till det skript som körs i en Python-process.</span><span class="sxs-lookup"><span data-stu-id="97eb3-147">C Python is an external process, so the data from Pig on the JVM is sent out to the script running in a Python process.</span></span> <span data-ttu-id="97eb3-148">Utdata från skriptet Python skickas tillbaka till Pig.</span><span class="sxs-lookup"><span data-stu-id="97eb3-148">The output of the Python script is sent back into Pig.</span></span>

<span data-ttu-id="97eb3-149">Om du vill ange Python-tolkning `register` referera till Python-skriptet.</span><span class="sxs-lookup"><span data-stu-id="97eb3-149">To specify the Python interpreter, use `register` when referencing the Python script.</span></span> <span data-ttu-id="97eb3-150">I följande exempel registrera skript med Pig som `myfuncs`:</span><span class="sxs-lookup"><span data-stu-id="97eb3-150">The following examples register scripts with Pig as `myfuncs`:</span></span>

* <span data-ttu-id="97eb3-151">**Att använda Jython**:`register '/path/to/pigudf.py' using jython as myfuncs;`</span><span class="sxs-lookup"><span data-stu-id="97eb3-151">**To use Jython**: `register '/path/to/pigudf.py' using jython as myfuncs;`</span></span>
* <span data-ttu-id="97eb3-152">**Att använda C Python**:`register '/path/to/pigudf.py' using streaming_python as myfuncs;`</span><span class="sxs-lookup"><span data-stu-id="97eb3-152">**To use C Python**: `register '/path/to/pigudf.py' using streaming_python as myfuncs;`</span></span>

> [!IMPORTANT]
> <span data-ttu-id="97eb3-153">När du använder Jython, sökvägen till filen pig_jython kan vara en lokal sökväg eller en WASB: / / sökväg.</span><span class="sxs-lookup"><span data-stu-id="97eb3-153">When using Jython, the path to the pig_jython file can be either a local path or a WASB:// path.</span></span> <span data-ttu-id="97eb3-154">När du använder C Python, måste du referera en fil på det lokala filsystemet på den nod som du använder för att skicka Pig-jobbet.</span><span class="sxs-lookup"><span data-stu-id="97eb3-154">However, when using C Python, you must reference a file on the local file system of the node that you are using to submit the Pig job.</span></span>

<span data-ttu-id="97eb3-155">En gång tidigare registrering, Pig Latin för det här exemplet är samma för både:</span><span class="sxs-lookup"><span data-stu-id="97eb3-155">Once past registration, the Pig Latin for this example is the same for both:</span></span>

```pig
LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
LOG = FILTER LOGS by LINE is not null;
DETAILS = FOREACH LOG GENERATE myfuncs.create_structure(LINE);
DUMP DETAILS;
```

<span data-ttu-id="97eb3-156">Här är det här exemplet har:</span><span class="sxs-lookup"><span data-stu-id="97eb3-156">Here's what this example does:</span></span>

1. <span data-ttu-id="97eb3-157">Den första raden läser in data exempelfilen `sample.log` till `LOGS`.</span><span class="sxs-lookup"><span data-stu-id="97eb3-157">The first line loads the sample data file, `sample.log` into `LOGS`.</span></span> <span data-ttu-id="97eb3-158">Varje post som definierar också en `chararray`.</span><span class="sxs-lookup"><span data-stu-id="97eb3-158">It also defines each record as a `chararray`.</span></span>
2. <span data-ttu-id="97eb3-159">Nästa rad filtrerar ut eventuella null-värden som lagrar resultatet av åtgärden i `LOG`.</span><span class="sxs-lookup"><span data-stu-id="97eb3-159">The next line filters out any null values, storing the result of the operation into `LOG`.</span></span>
3. <span data-ttu-id="97eb3-160">Därefter den itererar över poster i `LOG` och använder `GENERATE` att anropa den `create_structure` metod i Python/Jython-skript som läses in som `myfuncs`.</span><span class="sxs-lookup"><span data-stu-id="97eb3-160">Next, it iterates over the records in `LOG` and uses `GENERATE` to invoke the `create_structure` method contained in the Python/Jython script loaded as `myfuncs`.</span></span> <span data-ttu-id="97eb3-161">`LINE`används för att skicka den aktuella posten till funktionen.</span><span class="sxs-lookup"><span data-stu-id="97eb3-161">`LINE` is used to pass the current record to the function.</span></span>
4. <span data-ttu-id="97eb3-162">Slutligen utdata dumpas STDOUT med hjälp av den `DUMP` kommando.</span><span class="sxs-lookup"><span data-stu-id="97eb3-162">Finally, the outputs are dumped to STDOUT using the `DUMP` command.</span></span> <span data-ttu-id="97eb3-163">Detta kommando visar resultaten när åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="97eb3-163">This command displays the results after the operation completes.</span></span>

### <a name="create-the-pigudfpy-file"></a><span data-ttu-id="97eb3-164">Skapa filen pigudf.py</span><span class="sxs-lookup"><span data-stu-id="97eb3-164">Create the pigudf.py file</span></span>

<span data-ttu-id="97eb3-165">Skapa en textfil med namnet på din utvecklingsmiljö `pigudf.py`.</span><span class="sxs-lookup"><span data-stu-id="97eb3-165">On your development environment, create a text file named `pigudf.py`.</span></span> <span data-ttu-id="97eb3-166">Använd följande kod som innehållet i filen:</span><span class="sxs-lookup"><span data-stu-id="97eb3-166">Use the following code as the contents of the file:</span></span>

<a name="streamingpy"></a>

```python
# Uncomment the following if using C Python
#from pig_util import outputSchema

@outputSchema("log: {(date:chararray, time:chararray, classname:chararray, level:chararray, detail:chararray)}")
def create_structure(input):
    if (input.startswith('java.lang.Exception')):
        input = input[21:len(input)] + ' - java.lang.Exception'
    date, time, classname, level, detail = input.split(' ', 4)
    return date, time, classname, level, detail
```

<span data-ttu-id="97eb3-167">I exemplet Pig Latin vi har definierat den `LINE` indata som en chararray eftersom det finns inget konsekvent schema för indata.</span><span class="sxs-lookup"><span data-stu-id="97eb3-167">In the Pig Latin example, we defined the `LINE` input as a chararray because there is no consistent schema for the input.</span></span> <span data-ttu-id="97eb3-168">Python-skriptet omvandlar data till ett konsekvent schema för utdata.</span><span class="sxs-lookup"><span data-stu-id="97eb3-168">The Python script transforms the data into a consistent schema for output.</span></span>

1. <span data-ttu-id="97eb3-169">Den `@outputSchema` uttrycket definierar formatet för data som returneras till Pig.</span><span class="sxs-lookup"><span data-stu-id="97eb3-169">The `@outputSchema` statement defines the format of the data that is returned to Pig.</span></span> <span data-ttu-id="97eb3-170">I det här fallet har en **data egenskapsuppsättning**, vilket är en Pig-datatyp.</span><span class="sxs-lookup"><span data-stu-id="97eb3-170">In this case, it's a **data bag**, which is a Pig data type.</span></span> <span data-ttu-id="97eb3-171">Uppsättningen innehåller följande fält som är chararray (strängar):</span><span class="sxs-lookup"><span data-stu-id="97eb3-171">The bag contains the following fields, all of which are chararray (strings):</span></span>

   * <span data-ttu-id="97eb3-172">datum – datumet loggposten skapades</span><span class="sxs-lookup"><span data-stu-id="97eb3-172">date - the date the log entry was created</span></span>
   * <span data-ttu-id="97eb3-173">tid - tiden loggposten skapades</span><span class="sxs-lookup"><span data-stu-id="97eb3-173">time - the time the log entry was created</span></span>
   * <span data-ttu-id="97eb3-174">Klassnamn - klassnamnet posten skapades för</span><span class="sxs-lookup"><span data-stu-id="97eb3-174">classname - the class name the entry was created for</span></span>
   * <span data-ttu-id="97eb3-175">nivå - loggningsnivån</span><span class="sxs-lookup"><span data-stu-id="97eb3-175">level - the log level</span></span>
   * <span data-ttu-id="97eb3-176">detaljer - utförlig information för loggposten</span><span class="sxs-lookup"><span data-stu-id="97eb3-176">detail - verbose details for the log entry</span></span>

2. <span data-ttu-id="97eb3-177">Sedan den `def create_structure(input)` definierar vilken funktion Pig skickar objekt till.</span><span class="sxs-lookup"><span data-stu-id="97eb3-177">Next, the `def create_structure(input)` defines the function that Pig passes line items to.</span></span>

3. <span data-ttu-id="97eb3-178">Exempeldata `sample.log`, främst överensstämmer med datum, tid, classname, nivå, och innehåller information om schemat som vi vill returnera.</span><span class="sxs-lookup"><span data-stu-id="97eb3-178">The example data, `sample.log`, mostly conforms to the date, time, classname, level, and detail schema we want to return.</span></span> <span data-ttu-id="97eb3-179">Men den innehåller några rader som börjar med `*java.lang.Exception*`.</span><span class="sxs-lookup"><span data-stu-id="97eb3-179">However, it contains a few lines that begin with `*java.lang.Exception*`.</span></span> <span data-ttu-id="97eb3-180">Dessa rader måste ändras för att matcha schemat.</span><span class="sxs-lookup"><span data-stu-id="97eb3-180">These lines must be modified to match the schema.</span></span> <span data-ttu-id="97eb3-181">Den `if` instruktionen kontrollerar för dem och sedan massages indata för att flytta den `*java.lang.Exception*` strängen i syfte att samla data i-raden med vår utdata som förväntas schemat.</span><span class="sxs-lookup"><span data-stu-id="97eb3-181">The `if` statement checks for those, then massages the input data to move the `*java.lang.Exception*` string to the end, bringing the data in-line with our expected output schema.</span></span>

4. <span data-ttu-id="97eb3-182">Sedan den `split` kommandot används för att dela data på de första fyra blanksteg.</span><span class="sxs-lookup"><span data-stu-id="97eb3-182">Next, the `split` command is used to split the data at the first four space characters.</span></span> <span data-ttu-id="97eb3-183">Utdata har tilldelats till `date`, `time`, `classname`, `level`, och `detail`.</span><span class="sxs-lookup"><span data-stu-id="97eb3-183">The output is assigned into `date`, `time`, `classname`, `level`, and `detail`.</span></span>

5. <span data-ttu-id="97eb3-184">Slutligen returneras värdena till Pig.</span><span class="sxs-lookup"><span data-stu-id="97eb3-184">Finally, the values are returned to Pig.</span></span>

<span data-ttu-id="97eb3-185">När data returneras till Pig, har ett konsekvent schema som definierats i den `@outputSchema` instruktionen.</span><span class="sxs-lookup"><span data-stu-id="97eb3-185">When the data is returned to Pig, it has a consistent schema as defined in the `@outputSchema` statement.</span></span>

## <span data-ttu-id="97eb3-186"><a name="running"></a>Ladda upp och köra exemplen</span><span class="sxs-lookup"><span data-stu-id="97eb3-186"><a name="running"></a>Upload and run the examples</span></span>

> [!IMPORTANT]
> <span data-ttu-id="97eb3-187">Den **SSH** stegen fungerar endast med en Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="97eb3-187">The **SSH** steps only work with a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="97eb3-188">Den **PowerShell** stegen fungerar med en Linux- eller Windows-baserade HDInsight-kluster, men kräver en Windows-klient.</span><span class="sxs-lookup"><span data-stu-id="97eb3-188">The **PowerShell** steps work with either a Linux or Windows-based HDInsight cluster, but require a Windows client.</span></span>

### <a name="ssh"></a><span data-ttu-id="97eb3-189">SSH</span><span class="sxs-lookup"><span data-stu-id="97eb3-189">SSH</span></span>

<span data-ttu-id="97eb3-190">Mer information om hur du använder SSH finns [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="97eb3-190">For more information on using SSH, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

1. <span data-ttu-id="97eb3-191">Använd `scp` att kopiera filer till ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="97eb3-191">Use `scp` to copy the files to your HDInsight cluster.</span></span> <span data-ttu-id="97eb3-192">Till exempel följande kommando kopierar filerna till ett kluster med namnet **mycluster**.</span><span class="sxs-lookup"><span data-stu-id="97eb3-192">For example, the following command copies the files to a cluster named **mycluster**.</span></span>

    ```bash
    scp hiveudf.py pigudf.py myuser@mycluster-ssh.azurehdinsight.net:
    ```

2. <span data-ttu-id="97eb3-193">Använda SSH för att ansluta till klustret.</span><span class="sxs-lookup"><span data-stu-id="97eb3-193">Use SSH to connect to the cluster.</span></span>

    ```bash
    ssh myuser@mycluster-ssh.azurehdinsight.net
    ```

3. <span data-ttu-id="97eb3-194">Lägga till python-filer som tidigare har överförts till WASB-lagring för klustret från SSH-session.</span><span class="sxs-lookup"><span data-stu-id="97eb3-194">From the SSH session, add the python files uploaded previously to the WASB storage for the cluster.</span></span>

    ```bash
    hdfs dfs -put hiveudf.py /hiveudf.py
    hdfs dfs -put pigudf.py /pigudf.py
    ```

<span data-ttu-id="97eb3-195">Använd följande steg när du överför filerna för att köra Hive och Pig-jobb.</span><span class="sxs-lookup"><span data-stu-id="97eb3-195">After uploading the files, use the following steps to run the Hive and Pig jobs.</span></span>

#### <a name="use-the-hive-udf"></a><span data-ttu-id="97eb3-196">Använda Hive UDF</span><span class="sxs-lookup"><span data-stu-id="97eb3-196">Use the Hive UDF</span></span>

1. <span data-ttu-id="97eb3-197">Använd den `hive` kommando för att starta hive-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="97eb3-197">Use the `hive` command to start the hive shell.</span></span> <span data-ttu-id="97eb3-198">Du bör se en `hive>` fråga när gränssnittet har lästs in.</span><span class="sxs-lookup"><span data-stu-id="97eb3-198">You should see a `hive>` prompt once the shell has loaded.</span></span>

2. <span data-ttu-id="97eb3-199">Ange följande fråga i den `hive>` prompten:</span><span class="sxs-lookup"><span data-stu-id="97eb3-199">Enter the following query at the `hive>` prompt:</span></span>

   ```hive
   add file wasb:///hiveudf.py;
   SELECT TRANSFORM (clientid, devicemake, devicemodel)
       USING 'python hiveudf.py' AS
       (clientid string, phoneLabel string, phoneHash string)
   FROM hivesampletable
   ORDER BY clientid LIMIT 50;
   ```

3. <span data-ttu-id="97eb3-200">När du har angett den sista raden ska jobbet starta.</span><span class="sxs-lookup"><span data-stu-id="97eb3-200">After entering the last line, the job should start.</span></span> <span data-ttu-id="97eb3-201">När jobbet har slutförts, returnerar utdata liknar följande exempel:</span><span class="sxs-lookup"><span data-stu-id="97eb3-201">Once the job completes, it returns output similar to the following example:</span></span>

        100041    RIM 9650    d476f3687700442549a83fac4560c51c
        100041    RIM 9650    d476f3687700442549a83fac4560c51c
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9

#### <a name="use-the-pig-udf"></a><span data-ttu-id="97eb3-202">Använda Pig UDF</span><span class="sxs-lookup"><span data-stu-id="97eb3-202">Use the Pig UDF</span></span>

1. <span data-ttu-id="97eb3-203">Använd den `pig` kommando för att starta Windows-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="97eb3-203">Use the `pig` command to start the shell.</span></span> <span data-ttu-id="97eb3-204">Du ser en `grunt>` fråga när gränssnittet har lästs in.</span><span class="sxs-lookup"><span data-stu-id="97eb3-204">You see a `grunt>` prompt once the shell has loaded.</span></span>

2. <span data-ttu-id="97eb3-205">Ange följande uttryck på den `grunt>` prompten:</span><span class="sxs-lookup"><span data-stu-id="97eb3-205">Enter the following statements at the `grunt>` prompt:</span></span>

   ```pig
   Register wasb:///pigudf.py using jython as myfuncs;
   LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
   LOG = FILTER LOGS by LINE is not null;
   DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
   DUMP DETAILS;
   ```

3. <span data-ttu-id="97eb3-206">När du har angett följande rad ska jobbet starta.</span><span class="sxs-lookup"><span data-stu-id="97eb3-206">After entering the following line, the job should start.</span></span> <span data-ttu-id="97eb3-207">När jobbet har slutförts, returnerar utdata liknar följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="97eb3-207">Once the job completes, it returns output similar to the following data:</span></span>

        ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
        ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
        ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
        ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
        ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

4. <span data-ttu-id="97eb3-208">Använd `quit` att lämna gränssnittet Grunt och Använd sedan följande för att redigera filen pigudf.py på det lokala filsystemet:</span><span class="sxs-lookup"><span data-stu-id="97eb3-208">Use `quit` to exit the Grunt shell, and then use the following to edit the pigudf.py file on the local file system:</span></span>

    ```bash
    nano pigudf.py
    ```

5. <span data-ttu-id="97eb3-209">En gång i redigeraren, ta bort kommentarerna följande rad genom att ta bort den `#` tecken från början av raden:</span><span class="sxs-lookup"><span data-stu-id="97eb3-209">Once in the editor, uncomment the following line by removing the `#` character from the beginning of the line:</span></span>

    ```bash
    #from pig_util import outputSchema
    ```

    <span data-ttu-id="97eb3-210">När ändringen har gjorts, använder du Ctrl + X för att avsluta redigeraren.</span><span class="sxs-lookup"><span data-stu-id="97eb3-210">Once the change has been made, use Ctrl+X to exit the editor.</span></span> <span data-ttu-id="97eb3-211">Välj Y och ange sedan om du vill spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="97eb3-211">Select Y, and then enter to save the changes.</span></span>

6. <span data-ttu-id="97eb3-212">Använd den `pig` kommando för att starta gränssnittet igen.</span><span class="sxs-lookup"><span data-stu-id="97eb3-212">Use the `pig` command to start the shell again.</span></span> <span data-ttu-id="97eb3-213">När du är på den `grunt>` uppmanar, Använd följande för att köra skriptet Python med Python C-tolken.</span><span class="sxs-lookup"><span data-stu-id="97eb3-213">Once you are at the `grunt>` prompt, use the following to run the Python script using the C Python interpreter.</span></span>

   ```pig
   Register 'pigudf.py' using streaming_python as myfuncs;
   LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
   LOG = FILTER LOGS by LINE is not null;
   DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
   DUMP DETAILS;
   ```

    <span data-ttu-id="97eb3-214">När jobbet har slutförts, bör du se samma utdata som när du redan har kört skriptet med Jython.</span><span class="sxs-lookup"><span data-stu-id="97eb3-214">Once this job completes, you should see the same output as when you previously ran the script using Jython.</span></span>

### <a name="powershell-upload-the-files"></a><span data-ttu-id="97eb3-215">PowerShell: Överför filerna</span><span class="sxs-lookup"><span data-stu-id="97eb3-215">PowerShell: Upload the files</span></span>

<span data-ttu-id="97eb3-216">Du kan använda PowerShell för att överföra filer till HDInsight-servern.</span><span class="sxs-lookup"><span data-stu-id="97eb3-216">You can use PowerShell to upload the files to the HDInsight server.</span></span> <span data-ttu-id="97eb3-217">Använd följande skript för att ladda upp Python-filer:</span><span class="sxs-lookup"><span data-stu-id="97eb3-217">Use the following script to upload the Python files:</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="97eb3-218">Stegen i det här avsnittet använder Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="97eb3-218">The steps in this section use Azure PowerShell.</span></span> <span data-ttu-id="97eb3-219">Mer information om hur du använder Azure PowerShell finns [hur du installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="97eb3-219">For more information on using Azure PowerShell, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

```powershell
# Login to your Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Add-AzureRmAccount
}

# Get cluster info
$clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
# Change the path to match the file location on your system
$pathToStreamingFile = "C:\path\to\hiveudf.py"
$pathToJythonFile = "C:\path\to\pigudf.py"

$clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroup = $clusterInfo.ResourceGroup
$storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
$container=$clusterInfo.DefaultStorageContainer
$storageAccountKey=(Get-AzureRmStorageAccountKey `
    -Name $storageAccountName `
-ResourceGroupName $resourceGroup)[0].Value

#Create a storage content and upload the file
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
> <span data-ttu-id="97eb3-220">Ändra den `C:\path\to` värde till sökvägen till filerna på din utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="97eb3-220">Change the `C:\path\to` value to the path to the files on your development environment.</span></span>

<span data-ttu-id="97eb3-221">Detta skript hämtar information om ditt HDInsight-kluster och sedan extraherar konto och nyckel för standardkontot för lagring och överför filer till roten i behållaren.</span><span class="sxs-lookup"><span data-stu-id="97eb3-221">This script retrieves information for your HDInsight cluster, then extracts the account and key for the default storage account, and uploads the files to the root of the container.</span></span>

> [!NOTE]
> <span data-ttu-id="97eb3-222">Mer information om överföringen av filer finns i [överföra data för Hadoop-jobb i HDInsight](hdinsight-upload-data.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="97eb3-222">For more information on uploading files, see the [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md) document.</span></span>

#### <a name="powershell-use-the-hive-udf"></a><span data-ttu-id="97eb3-223">PowerShell: Använda Hive UDF</span><span class="sxs-lookup"><span data-stu-id="97eb3-223">PowerShell: Use the Hive UDF</span></span>

<span data-ttu-id="97eb3-224">PowerShell kan också användas för att köra Hive-frågor via fjärranslutning.</span><span class="sxs-lookup"><span data-stu-id="97eb3-224">PowerShell can also be used to remotely run Hive queries.</span></span> <span data-ttu-id="97eb3-225">Använd följande PowerShell-skript för att köra en Hive-fråga som använder **hiveudf.py** skript:</span><span class="sxs-lookup"><span data-stu-id="97eb3-225">Use the following PowerShell script to run a Hive query that uses **hiveudf.py** script:</span></span>

> [!IMPORTANT]
> <span data-ttu-id="97eb3-226">Innan du kör uppmanas du att HTTPs/Admin kontoinformationen för ditt HDInsight-kluster i skriptet.</span><span class="sxs-lookup"><span data-stu-id="97eb3-226">Before running, the script prompts you for the HTTPs/Admin account information for your HDInsight cluster.</span></span>

```powershell
# Login to your Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Add-AzureRmAccount
}

# Get cluster info
$clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
$creds=Get-Credential -Message "Enter the login for the cluster"

# If using a Windows-based HDInsight cluster, change the USING statement to:
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
Write-Host "Wait for the Hive job to complete ..." -ForegroundColor Green
Wait-AzureRmHDInsightJob `
    -JobId $job.JobId `
    -ClusterName $clusterName `
    -HttpCredential $creds
# Uncomment the following to see stderr output
# Get-AzureRmHDInsightJobOutput `
#   -Clustername $clusterName `
#   -JobId $job.JobId `
#   -HttpCredential $creds `
#   -DisplayOutputType StandardError
Write-Host "Display the standard output ..." -ForegroundColor Green
Get-AzureRmHDInsightJobOutput `
    -Clustername $clusterName `
    -JobId $job.JobId `
    -HttpCredential $creds
```

<span data-ttu-id="97eb3-227">Utdata för den **Hive** jobbet ska se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="97eb3-227">The output for the **Hive** job should appear similar to the following example:</span></span>

    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9

#### <a name="pig-jython"></a><span data-ttu-id="97eb3-228">Pig (Jython)</span><span class="sxs-lookup"><span data-stu-id="97eb3-228">Pig (Jython)</span></span>

<span data-ttu-id="97eb3-229">PowerShell kan också användas för att köra Pig Latin-jobb.</span><span class="sxs-lookup"><span data-stu-id="97eb3-229">PowerShell can also be used to run Pig Latin jobs.</span></span> <span data-ttu-id="97eb3-230">Att köra ett Pig Latin-jobb som använder den **pigudf.py** skript, Använd följande PowerShell-skript:</span><span class="sxs-lookup"><span data-stu-id="97eb3-230">To run a Pig Latin job that uses the **pigudf.py** script, use the following PowerShell script:</span></span>

> [!NOTE]
> <span data-ttu-id="97eb3-231">När du skickar in ett jobb som använder PowerShell via fjärranslutning, går det inte att använda C Python som tolkens.</span><span class="sxs-lookup"><span data-stu-id="97eb3-231">When remotely submitting a job using PowerShell, it is not possible to use C Python as the interpreter.</span></span>

```powershell
# Login to your Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Add-AzureRmAccount
}

# Get cluster info
$clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
$creds=Get-Credential -Message "Enter the login for the cluster"

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

Write-Host "Wait for the Pig job to complete ..." -ForegroundColor Green
Wait-AzureRmHDInsightJob `
    -Job $job.JobId `
    -ClusterName $clusterName `
    -HttpCredential $creds
# Uncomment the following to see stderr output
# Get-AzureRmHDInsightJobOutput `
#    -Clustername $clusterName `
#    -JobId $job.JobId `
#    -HttpCredential $creds `
#    -DisplayOutputType StandardError
Write-Host "Display the standard output ..." -ForegroundColor Green
Get-AzureRmHDInsightJobOutput `
    -Clustername $clusterName `
    -JobId $job.JobId `
    -HttpCredential $creds
```

<span data-ttu-id="97eb3-232">Utdata för den **Pig** jobb bör likna följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="97eb3-232">The output for the **Pig** job should appear similar to the following data:</span></span>

    ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
    ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
    ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
    ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
    ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

## <span data-ttu-id="97eb3-233"><a name="troubleshooting"></a>Felsökning</span><span class="sxs-lookup"><span data-stu-id="97eb3-233"><a name="troubleshooting"></a>Troubleshooting</span></span>

### <a name="errors-when-running-jobs"></a><span data-ttu-id="97eb3-234">Fel när du kör jobb</span><span class="sxs-lookup"><span data-stu-id="97eb3-234">Errors when running jobs</span></span>

<span data-ttu-id="97eb3-235">När du kör hive-jobb kan som uppstå ett fel som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="97eb3-235">When running the hive job, you may encounter an error similar to the following text:</span></span>

    Caused by: org.apache.hadoop.hive.ql.metadata.HiveException: [Error 20001]: An error occurred while reading or writing to your custom script. It may have crashed with an error.

<span data-ttu-id="97eb3-236">Det här problemet kan orsakas av radbrytningar i Python-fil.</span><span class="sxs-lookup"><span data-stu-id="97eb3-236">This problem may be caused by the line endings in the Python file.</span></span> <span data-ttu-id="97eb3-237">Många Windows redigerare som standard använder CRLF som rad avslutas, men Linux-program vanligtvis förväntas LF.</span><span class="sxs-lookup"><span data-stu-id="97eb3-237">Many Windows editors default to using CRLF as the line ending, but Linux applications usually expect LF.</span></span>

<span data-ttu-id="97eb3-238">Du kan använda följande PowerShell-instruktioner för att ta bort CR-tecken före överföra filen till HDInsight:</span><span class="sxs-lookup"><span data-stu-id="97eb3-238">You can use the following PowerShell statements to remove the CR characters before uploading the file to HDInsight:</span></span>

```powershell
$original_file ='c:\path\to\hiveudf.py'
$text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
[IO.File]::WriteAllText($original_file, $text)
```

### <a name="powershell-scripts"></a><span data-ttu-id="97eb3-239">PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="97eb3-239">PowerShell scripts</span></span>

<span data-ttu-id="97eb3-240">Både exempel PowerShell-skript som används för att köra exemplen innehåller en kommenterad rad som visar Felutdata för jobbet.</span><span class="sxs-lookup"><span data-stu-id="97eb3-240">Both of the example PowerShell scripts used to run the examples contain a commented line that displays error output for the job.</span></span> <span data-ttu-id="97eb3-241">Om du inte ser utdata som förväntas för jobbet, ta bort kommentarerna följande rad och se om information om felet uppstått problem.</span><span class="sxs-lookup"><span data-stu-id="97eb3-241">If you are not seeing the expected output for the job, uncomment the following line and see if the error information indicates a problem.</span></span>

```powershell
# Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

<span data-ttu-id="97eb3-242">Information om felet (STDERR) och resultatet av jobbet (STDOUT) loggas också i HDInsight-lagringen.</span><span class="sxs-lookup"><span data-stu-id="97eb3-242">The error information (STDERR) and the result of the job (STDOUT) are also logged to your HDInsight storage.</span></span>

| <span data-ttu-id="97eb3-243">För det här jobbet...</span><span class="sxs-lookup"><span data-stu-id="97eb3-243">For this job...</span></span> | <span data-ttu-id="97eb3-244">Titta på de här filerna i blob-behållaren</span><span class="sxs-lookup"><span data-stu-id="97eb3-244">Look at these files in the blob container</span></span> |
| --- | --- |
| <span data-ttu-id="97eb3-245">Hive</span><span class="sxs-lookup"><span data-stu-id="97eb3-245">Hive</span></span> |<span data-ttu-id="97eb3-246">/ HivePython/stderr</span><span class="sxs-lookup"><span data-stu-id="97eb3-246">/HivePython/stderr</span></span><p><span data-ttu-id="97eb3-247">/ HivePython/stdout</span><span class="sxs-lookup"><span data-stu-id="97eb3-247">/HivePython/stdout</span></span> |
| <span data-ttu-id="97eb3-248">Pig</span><span class="sxs-lookup"><span data-stu-id="97eb3-248">Pig</span></span> |<span data-ttu-id="97eb3-249">/ PigPython/stderr</span><span class="sxs-lookup"><span data-stu-id="97eb3-249">/PigPython/stderr</span></span><p><span data-ttu-id="97eb3-250">/ PigPython/stdout</span><span class="sxs-lookup"><span data-stu-id="97eb3-250">/PigPython/stdout</span></span> |

## <span data-ttu-id="97eb3-251"><a name="next"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="97eb3-251"><a name="next"></a>Next steps</span></span>

<span data-ttu-id="97eb3-252">Om du behöver läsa in Python-moduler som inte är som standard finns [hur du distribuerar en modul till Azure HDInsight](http://blogs.msdn.com/b/benjguin/archive/2014/03/03/how-to-deploy-a-python-module-to-windows-azure-hdinsight.aspx).</span><span class="sxs-lookup"><span data-stu-id="97eb3-252">If you need to load Python modules that aren't provided by default, see [How to deploy a module to Azure HDInsight](http://blogs.msdn.com/b/benjguin/archive/2014/03/03/how-to-deploy-a-python-module-to-windows-azure-hdinsight.aspx).</span></span>

<span data-ttu-id="97eb3-253">Andra sätt att använda svin, Hive, och Läs om hur du använder MapReduce i följande dokument:</span><span class="sxs-lookup"><span data-stu-id="97eb3-253">For other ways to use Pig, Hive, and to learn about using MapReduce, see the following documents:</span></span>

* [<span data-ttu-id="97eb3-254">Använda Hive med HDInsight</span><span class="sxs-lookup"><span data-stu-id="97eb3-254">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="97eb3-255">Använda Pig med HDInsight</span><span class="sxs-lookup"><span data-stu-id="97eb3-255">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="97eb3-256">Använda MapReduce med HDInsight</span><span class="sxs-lookup"><span data-stu-id="97eb3-256">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)
