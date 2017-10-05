---
title: Analysera och processen JSON-dokument med Hive i HDInsight | Microsoft Docs
description: "Lär dig hur du använder JSON-dokument och analysera dem med Hive i HDInsight."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: e17794e8-faae-4264-9434-67f61ea78f13
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/26/2017
ms.author: jgao
ms.openlocfilehash: bd136afebeceb0cd9c24cfc5f15601caa80a755e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="process-and-analyze-json-documents-using-hive-in-hdinsight"></a><span data-ttu-id="91fde-103">Bearbeta och analysera JSON-dokument med hjälp av Hive i HDInsight</span><span class="sxs-lookup"><span data-stu-id="91fde-103">Process and analyze JSON documents using Hive in HDInsight</span></span>

<span data-ttu-id="91fde-104">Lär dig mer om att bearbeta och analysera JSON-filer med hjälp av Hive i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="91fde-104">Learn how to process and analyze JSON files using Hive in HDInsight.</span></span> <span data-ttu-id="91fde-105">Följande JSON-dokumentet används i självstudiekursen:</span><span class="sxs-lookup"><span data-stu-id="91fde-105">The following JSON document is used in the tutorial:</span></span>

    {
        "StudentId": "trgfg-5454-fdfdg-4346",
        "Grade": 7,
        "StudentDetails": [
            {
                "FirstName": "Peggy",
                "LastName": "Williams",
                "YearJoined": 2012
            }
        ],
        "StudentClassCollection": [
            {
                "ClassId": "89084343",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "High",
                "Score": 93,
                "PerformedActivity": false
            },
            {
                "ClassId": "78547522",
                "ClassParticipation": "NotSatisfied",
                "ClassParticipationRank": "None",
                "Score": 74,
                "PerformedActivity": false
            },
            {
                "ClassId": "78675563",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "Low",
                "Score": 83,
                "PerformedActivity": true
            }
        ]
    }

<span data-ttu-id="91fde-106">Filen finns på wasb://processjson@hditutorialdata.blob.core.windows.net/.</span><span class="sxs-lookup"><span data-stu-id="91fde-106">The file can be found at wasb://processjson@hditutorialdata.blob.core.windows.net/.</span></span> <span data-ttu-id="91fde-107">Mer information om hur du använder Azure Blob storage med HDInsight finns [använda HDFS-kompatibla Azure Blob storage med Hadoop i HDInsight](hdinsight-hadoop-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="91fde-107">For more information on using Azure Blob storage with HDInsight, see [Use HDFS-compatible Azure Blob storage with Hadoop in HDInsight](hdinsight-hadoop-use-blob-storage.md).</span></span> <span data-ttu-id="91fde-108">Du kan kopiera filen till standardbehållaren på klustret.</span><span class="sxs-lookup"><span data-stu-id="91fde-108">You can copy the file to the default container of your cluster.</span></span>

<span data-ttu-id="91fde-109">I den här kursen använder du Hive-konsolen.</span><span class="sxs-lookup"><span data-stu-id="91fde-109">In this tutorial, you use the Hive console.</span></span>  <span data-ttu-id="91fde-110">Anvisningar för att öppna konsolen Hive finns [använda Hive med Hadoop i HDInsight med fjärrskrivbord](hdinsight-hadoop-use-hive-remote-desktop.md).</span><span class="sxs-lookup"><span data-stu-id="91fde-110">For instructions of opening the Hive console, see [Use Hive with Hadoop on HDInsight with Remote Desktop](hdinsight-hadoop-use-hive-remote-desktop.md).</span></span>

## <a name="flatten-json-documents"></a><span data-ttu-id="91fde-111">Förenkla JSON-dokument</span><span class="sxs-lookup"><span data-stu-id="91fde-111">Flatten JSON documents</span></span>
<span data-ttu-id="91fde-112">De metoder som anges i nästa avsnitt kräver JSON-dokument i en enda rad.</span><span class="sxs-lookup"><span data-stu-id="91fde-112">The methods listed in the next section require the JSON document in a single row.</span></span> <span data-ttu-id="91fde-113">Därför måste du förenkla JSON-dokument till en sträng.</span><span class="sxs-lookup"><span data-stu-id="91fde-113">So you must flatten the JSON document to a string.</span></span> <span data-ttu-id="91fde-114">Om JSON-dokumentet redan förenklas du hoppa över det här steget och gå direkt till nästa avsnitt på Analysera JSON-data.</span><span class="sxs-lookup"><span data-stu-id="91fde-114">If your JSON document is already flattened, you can skip this step and go straight to the next section on Analyzing JSON data.</span></span>

    DROP TABLE IF EXISTS StudentsRaw;
    CREATE EXTERNAL TABLE StudentsRaw (textcol string) STORED AS TEXTFILE LOCATION "wasb://processjson@hditutorialdata.blob.core.windows.net/";

    DROP TABLE IF EXISTS StudentsOneLine;
    CREATE EXTERNAL TABLE StudentsOneLine
    (
      json_body string
    )
    STORED AS TEXTFILE LOCATION '/json/students';

    INSERT OVERWRITE TABLE StudentsOneLine
    SELECT CONCAT_WS(' ',COLLECT_LIST(textcol)) AS singlelineJSON
          FROM (SELECT INPUT__FILE__NAME,BLOCK__OFFSET__INSIDE__FILE, textcol FROM StudentsRaw DISTRIBUTE BY INPUT__FILE__NAME SORT BY BLOCK__OFFSET__INSIDE__FILE) x
          GROUP BY INPUT__FILE__NAME;

    SELECT * FROM StudentsOneLine

<span data-ttu-id="91fde-115">Rådata JSON-filen finns på  **wasb://processjson@hditutorialdata.blob.core.windows.net/** .</span><span class="sxs-lookup"><span data-stu-id="91fde-115">The raw JSON file is located at **wasb://processjson@hditutorialdata.blob.core.windows.net/**.</span></span> <span data-ttu-id="91fde-116">Den *StudentsRaw* Hive tabell pekar till rå förenklade JSON-dokumentet.</span><span class="sxs-lookup"><span data-stu-id="91fde-116">The *StudentsRaw* Hive table points to the raw unflattened JSON document.</span></span>

<span data-ttu-id="91fde-117">Den *StudentsOneLine* Hive-tabell lagrar data i HDInsight filsystemet under den */json/studenter/* sökväg.</span><span class="sxs-lookup"><span data-stu-id="91fde-117">The *StudentsOneLine* Hive table stores the data in the HDInsight default file system under the */json/students/* path.</span></span>

<span data-ttu-id="91fde-118">INSERT-instruktionen fyller tabellen StudentOneLine med Flat JSON-data.</span><span class="sxs-lookup"><span data-stu-id="91fde-118">The INSERT statement populates the StudentOneLine table with the flattened JSON data.</span></span>

<span data-ttu-id="91fde-119">SELECT-instruktionen kan bara returnera en rad.</span><span class="sxs-lookup"><span data-stu-id="91fde-119">The SELECT statement shall only return one row.</span></span>

<span data-ttu-id="91fde-120">Här är resultatet av SELECT-instruktionen:</span><span class="sxs-lookup"><span data-stu-id="91fde-120">Here is the output of the SELECT statement:</span></span>

![Förenkling av JSON-dokumentet.][image-hdi-hivejson-flatten]

## <a name="analyze-json-documents-in-hive"></a><span data-ttu-id="91fde-122">Analysera JSON-dokument i Hive</span><span class="sxs-lookup"><span data-stu-id="91fde-122">Analyze JSON documents in Hive</span></span>
<span data-ttu-id="91fde-123">Hive innehåller tre olika metoder för att köra frågor på JSON-dokument:</span><span class="sxs-lookup"><span data-stu-id="91fde-123">Hive provides three different mechanisms to run queries on JSON documents:</span></span>

* <span data-ttu-id="91fde-124">Använd GET\_JSON\_OBJEKTET UDF (användardefinierad funktion)</span><span class="sxs-lookup"><span data-stu-id="91fde-124">use the GET\_JSON\_OBJECT UDF (User-defined function)</span></span>
* <span data-ttu-id="91fde-125">Använd JSON_TUPLE UDF</span><span class="sxs-lookup"><span data-stu-id="91fde-125">use the JSON_TUPLE UDF</span></span>
* <span data-ttu-id="91fde-126">Använda anpassade SerDe</span><span class="sxs-lookup"><span data-stu-id="91fde-126">use custom SerDe</span></span>
* <span data-ttu-id="91fde-127">skriva du äger UDF med Python eller andra språk.</span><span class="sxs-lookup"><span data-stu-id="91fde-127">write you own UDF using Python or other languages.</span></span> <span data-ttu-id="91fde-128">Se [i den här artikeln] [ hdinsight-python] på köra Python koden med Hive.</span><span class="sxs-lookup"><span data-stu-id="91fde-128">See [this article][hdinsight-python] on running your own Python code with Hive.</span></span>

### <a name="use-the-getjsonobject-udf"></a><span data-ttu-id="91fde-129">Använd GET\_JSON_OBJECT UDF</span><span class="sxs-lookup"><span data-stu-id="91fde-129">Use the GET\_JSON_OBJECT UDF</span></span>
<span data-ttu-id="91fde-130">Hive ger en inbyggda UDF kallas [hämta json-objekt](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object), som kan utföra JSON fråga under körning.</span><span class="sxs-lookup"><span data-stu-id="91fde-130">Hive provides a built-in UDF called [get json object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object), which can perform JSON querying during run time.</span></span> <span data-ttu-id="91fde-131">Den här metoden tar två argument – tabellnamnet och metodnamnet som har förenklad JSON-dokumentet och JSON-fält som ska tolkas.</span><span class="sxs-lookup"><span data-stu-id="91fde-131">This method takes two arguments – the table name and method name, which has the flattened JSON document and the JSON field that needs to be parsed.</span></span> <span data-ttu-id="91fde-132">Nu ska vi titta på ett exempel för att se hur den här UDF fungerar.</span><span class="sxs-lookup"><span data-stu-id="91fde-132">Let’s look at an example to see how this UDF works.</span></span>

<span data-ttu-id="91fde-133">Hämta det förnamn och efternamn för varje student</span><span class="sxs-lookup"><span data-stu-id="91fde-133">Get the first name and last name for each student</span></span>

    SELECT
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.FirstName'),
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.LastName')
    FROM StudentsOneLine;

<span data-ttu-id="91fde-134">Här är utdata när du kör den här frågan i konsolfönstret.</span><span class="sxs-lookup"><span data-stu-id="91fde-134">Here is the output when running this query in console window.</span></span>

![get_json_object UDF][image-hdi-hivejson-getjsonobject]

<span data-ttu-id="91fde-136">Det finns några begränsningar för get-json_object UDF.</span><span class="sxs-lookup"><span data-stu-id="91fde-136">There are a few limitations of the get-json_object UDF.</span></span>

* <span data-ttu-id="91fde-137">Eftersom varje fält i frågan kräver reparsing frågan, påverkar prestanda.</span><span class="sxs-lookup"><span data-stu-id="91fde-137">Because each field in the query requires reparsing the query, it affects the performance.</span></span>
* <span data-ttu-id="91fde-138">Hämta\_JSON_OBJECT() returnerar strängrepresentation av en matris.</span><span class="sxs-lookup"><span data-stu-id="91fde-138">GET\_JSON_OBJECT() returns the string representation of an array.</span></span> <span data-ttu-id="91fde-139">Om du vill konvertera denna matris till en Hive-matris, du måste använda reguljära uttryck för att ersätta hakparentes ' [' och ']' och även anropa delning för att få matrisen.</span><span class="sxs-lookup"><span data-stu-id="91fde-139">To convert this array to a Hive array, you have to use regular expressions to replace the square brackets ‘[‘ and ‘]’ and then also call split to get the array.</span></span>

<span data-ttu-id="91fde-140">Det är därför Hive-wiki rekommenderar att du använder json_tuple.</span><span class="sxs-lookup"><span data-stu-id="91fde-140">This is why the Hive wiki recommends using json_tuple.</span></span>  

### <a name="use-the-jsontuple-udf"></a><span data-ttu-id="91fde-141">Använd JSON_TUPLE UDF</span><span class="sxs-lookup"><span data-stu-id="91fde-141">Use the JSON_TUPLE UDF</span></span>
<span data-ttu-id="91fde-142">En annan UDF som tillhandahålls av Hive kallas [json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple), vilken som presterar bättre än [get_ json _object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object).</span><span class="sxs-lookup"><span data-stu-id="91fde-142">Another UDF provided by Hive is called [json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple), which performs better than [get_ json _object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object).</span></span> <span data-ttu-id="91fde-143">Den här metoden tar en uppsättning nycklar och en JSON-sträng, och returnerar en tuppel av värden med hjälp av en funktion.</span><span class="sxs-lookup"><span data-stu-id="91fde-143">This method takes a set of keys and a JSON string, and returns a tuple of values using one function.</span></span> <span data-ttu-id="91fde-144">Följande fråga returnerar student-id och klass från JSON-dokumentet:</span><span class="sxs-lookup"><span data-stu-id="91fde-144">The following query returns the student id and the grade from the JSON document:</span></span>

    SELECT q1.StudentId, q1.Grade
      FROM StudentsOneLine jt
      LATERAL VIEW JSON_TUPLE(jt.json_body, 'StudentId', 'Grade') q1
        AS StudentId, Grade;

<span data-ttu-id="91fde-145">Utdata från skriptet i Hive-konsolen:</span><span class="sxs-lookup"><span data-stu-id="91fde-145">The output of this script in the Hive console:</span></span>

![json_tuple UDF][image-hdi-hivejson-jsontuple]

<span data-ttu-id="91fde-147">JSON\_TUPPEL använder den [lateral visa](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) syntax i Hive, vilket gör att json\_tuppel att skapa en virtuell tabell genom att använda funktionen UDT för varje rad i den ursprungliga tabellen.</span><span class="sxs-lookup"><span data-stu-id="91fde-147">JSON\_TUPLE uses the [lateral view](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) syntax in Hive, which allows json\_tuple to create a virtual table by applying the UDT function to each row of the original table.</span></span>  <span data-ttu-id="91fde-148">Komplexa JSONs bli för ohanterlig på grund av upprepade användningen av LATERALA VYN.</span><span class="sxs-lookup"><span data-stu-id="91fde-148">Complex JSONs become too unwieldy because of the repeated use of LATERAL VIEW.</span></span> <span data-ttu-id="91fde-149">Dessutom kan inte JSON_TUPLE hantera kapslade JSONs.</span><span class="sxs-lookup"><span data-stu-id="91fde-149">Furthermore, JSON_TUPLE cannot handle nested JSONs.</span></span>

### <a name="use-custom-serde"></a><span data-ttu-id="91fde-150">Använda anpassade SerDe</span><span class="sxs-lookup"><span data-stu-id="91fde-150">Use custom SerDe</span></span>
<span data-ttu-id="91fde-151">SerDe är det bästa valet för parsning av kapslade JSON-dokument, kan du definiera JSON-schema och använda schemat för att tolka dokument.</span><span class="sxs-lookup"><span data-stu-id="91fde-151">SerDe is the best choice for parsing nested JSON documents, it allows you to define the JSON schema, and use the schema to parse the documents.</span></span> <span data-ttu-id="91fde-152">I kursen får du använder någon av de vanligaste SerDe som har utvecklats av [Roberto Congiu](https://github.com/rcongiu).</span><span class="sxs-lookup"><span data-stu-id="91fde-152">In this tutorial, you use one of the more popular SerDe that has been developed by [Roberto Congiu](https://github.com/rcongiu).</span></span>

<span data-ttu-id="91fde-153">**Använda anpassade SerDe:**</span><span class="sxs-lookup"><span data-stu-id="91fde-153">**To use the custom SerDe:**</span></span>

1. <span data-ttu-id="91fde-154">Installera [Java SE Development sats 7u55 JDK 1.7.0_55](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u55-oth-JPR).</span><span class="sxs-lookup"><span data-stu-id="91fde-154">Install [Java SE Development Kit 7u55 JDK 1.7.0_55](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u55-oth-JPR).</span></span> <span data-ttu-id="91fde-155">Välj Windows X64-versionen av JDK om du ska använda Windows-distribution av HDInsight</span><span class="sxs-lookup"><span data-stu-id="91fde-155">Choose the Windows X64 version of the JDK if you are going to be using the Windows deployment of HDInsight</span></span>
   
   > [!WARNING]
   > <span data-ttu-id="91fde-156">JDK 1.8 fungerar inte med den här SerDe.</span><span class="sxs-lookup"><span data-stu-id="91fde-156">JDK 1.8 doesn't work with this SerDe.</span></span>
   > 
   > 
   
    <span data-ttu-id="91fde-157">När installationen är klar, lägger du till en ny miljövariabel för användaren:</span><span class="sxs-lookup"><span data-stu-id="91fde-157">After the installation is completed, add a new user environment variable:</span></span>
   
   1. <span data-ttu-id="91fde-158">Öppna **visa avancerade systeminställningar** från Windows-skärmen.</span><span class="sxs-lookup"><span data-stu-id="91fde-158">Open **View advanced system settings** from the Windows screen.</span></span>
   2. <span data-ttu-id="91fde-159">Klicka på **miljövariabler**.</span><span class="sxs-lookup"><span data-stu-id="91fde-159">Click **Environment Variables**.</span></span>  
   3. <span data-ttu-id="91fde-160">Lägg till en ny **JAVA_HOME** miljövariabeln pekar till **C:\Program Files\Java\jdk1.7.0_55** eller var din JDK har installerats.</span><span class="sxs-lookup"><span data-stu-id="91fde-160">Add a new **JAVA_HOME** environment variable is pointing to **C:\Program Files\Java\jdk1.7.0_55** or wherever your JDK is installed.</span></span>
      
      ![Konfigurera rätt konfigurationsvärden för JDK][image-hdi-hivejson-jdk]
2. <span data-ttu-id="91fde-162">Installera [Maven 3.3.1](http://mirror.olnevhost.net/pub/apache/maven/maven-3/3.3.1/binaries/apache-maven-3.3.1-bin.zip)</span><span class="sxs-lookup"><span data-stu-id="91fde-162">Install [Maven 3.3.1](http://mirror.olnevhost.net/pub/apache/maven/maven-3/3.3.1/binaries/apache-maven-3.3.1-bin.zip)</span></span>
   
    <span data-ttu-id="91fde-163">Lägga till bin-mappen i din sökväg genom att gå till Control Panel--> Redigera systemvariabler för ditt konto miljövariabler.</span><span class="sxs-lookup"><span data-stu-id="91fde-163">Add the bin folder to your path by going to Control Panel-->Edit the System Variables for your account Environment variables.</span></span> <span data-ttu-id="91fde-164">Följande skärmbild visar hur du gör detta.</span><span class="sxs-lookup"><span data-stu-id="91fde-164">The following screenshot shows you how to do this.</span></span>
   
    ![Konfigurera Maven][image-hdi-hivejson-maven]
3. <span data-ttu-id="91fde-166">Klona projektet från [Hive-JSON-SerDe](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) github-platsen.</span><span class="sxs-lookup"><span data-stu-id="91fde-166">Clone the project from [Hive-JSON-SerDe](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) github site.</span></span> <span data-ttu-id="91fde-167">Du kan göra detta genom att klicka på knappen ”Hämta Zip” som visas i följande skärmbild.</span><span class="sxs-lookup"><span data-stu-id="91fde-167">You can do this by clicking on the “Download Zip” button as shown in the following screenshot.</span></span>
   
    ![Kloning av projektet][image-hdi-hivejson-serde]

<span data-ttu-id="91fde-169">4: Gå till mappen där du har hämtat det här paketet och typ ”mvn package”.</span><span class="sxs-lookup"><span data-stu-id="91fde-169">4: Go to the folder where you have downloaded this package and then type “mvn package”.</span></span> <span data-ttu-id="91fde-170">Detta bör skapa nödvändiga jar-filer som du sedan kan kopiera över till klustret.</span><span class="sxs-lookup"><span data-stu-id="91fde-170">This should create the necessary jar files that you can then copy over to the cluster.</span></span>

<span data-ttu-id="91fde-171">5: Gå till mappen i rotmappen dit du hämtade paketet.</span><span class="sxs-lookup"><span data-stu-id="91fde-171">5: Go to the target folder under the root folder where you downloaded the package.</span></span> <span data-ttu-id="91fde-172">Ladda upp filen json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar till huvudnod i klustret.</span><span class="sxs-lookup"><span data-stu-id="91fde-172">Upload the json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar file to head-node of your cluster.</span></span> <span data-ttu-id="91fde-173">Jag oftast placera den under mappen hive binära: C:\apps\dist\hive-0.13.0.2.1.11.0-2316\bin eller liknande.</span><span class="sxs-lookup"><span data-stu-id="91fde-173">I usually put it under the hive binary folder: C:\apps\dist\hive-0.13.0.2.1.11.0-2316\bin or something similar.</span></span>

<span data-ttu-id="91fde-174">6: Skriv ”Lägg till jar /path/to/json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar” i hive-fråga.</span><span class="sxs-lookup"><span data-stu-id="91fde-174">6: In the hive prompt, type “add jar /path/to/json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar”.</span></span> <span data-ttu-id="91fde-175">Eftersom min om jar i mappen C:\apps\dist\hive-0.13.x\bin kan jag direkt lägga till jar med namn som:</span><span class="sxs-lookup"><span data-stu-id="91fde-175">Since in my case, the jar is in the C:\apps\dist\hive-0.13.x\bin folder, I can directly add the jar with the name as shown:</span></span>

    add jar json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar;

   ![Lägger till JAR i ditt projekt][image-hdi-hivejson-addjar]

<span data-ttu-id="91fde-177">Du är nu redo att använda SerDe för att köra frågor mot JSON-dokumentet.</span><span class="sxs-lookup"><span data-stu-id="91fde-177">Now, you are ready to use the SerDe to run queries against the JSON document.</span></span>

<span data-ttu-id="91fde-178">Följande exempel skapar en tabell med ett definierat schema:</span><span class="sxs-lookup"><span data-stu-id="91fde-178">The following statement creates a table with a defined schema:</span></span>

    DROP TABLE json_table;
    CREATE EXTERNAL TABLE json_table (
      StudentId string,
      Grade int,
      StudentDetails array<struct<
          FirstName:string,
          LastName:string,
          YearJoined:int
          >
      >,
      StudentClassCollection array<struct<
          ClassId:string,
          ClassParticipation:string,
          ClassParticipationRank:string,
          Score:int,
          PerformedActivity:boolean
          >
      >
    ) ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'
    LOCATION '/json/students';

<span data-ttu-id="91fde-179">Förnamn och efternamn för studenter</span><span class="sxs-lookup"><span data-stu-id="91fde-179">To list the first name and last name of the student</span></span>

    SELECT StudentDetails.FirstName, StudentDetails.LastName FROM json_table;

<span data-ttu-id="91fde-180">Här är ett resultat från Hive-konsolen.</span><span class="sxs-lookup"><span data-stu-id="91fde-180">Here is the result from the Hive console.</span></span>

![SerDe frågan 1][image-hdi-hivejson-serde_query1]

<span data-ttu-id="91fde-182">Att beräkna summan av resultat av JSON-dokumentet</span><span class="sxs-lookup"><span data-stu-id="91fde-182">To calculate the sum of scores of the JSON document</span></span>

    SELECT SUM(scores)
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as scores;

<span data-ttu-id="91fde-183">Den föregående frågan använder [lateral Visa Expandera](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) UDF att expandera matrisen poäng så att de kan summeras.</span><span class="sxs-lookup"><span data-stu-id="91fde-183">The preceding query uses [lateral view explode](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) UDF to expand the array of scores so that they can be summed.</span></span>

<span data-ttu-id="91fde-184">Här är utdata från Hive-konsolen.</span><span class="sxs-lookup"><span data-stu-id="91fde-184">Here is the output from the Hive console.</span></span>

![SerDe fråga 2][image-hdi-hivejson-serde_query2]

<span data-ttu-id="91fde-186">Om du vill hitta som ämnen angivna student har fått fler än 80 poäng:</span><span class="sxs-lookup"><span data-stu-id="91fde-186">To find which subjects a given student has scored more than 80 points:</span></span>

    SELECT  
      jt.StudentClassCollection.ClassId
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as score  where score > 80;

<span data-ttu-id="91fde-187">Den föregående frågan returnerar en Hive-matris till skillnad från get\_json\_-objekt som returnerar en sträng.</span><span class="sxs-lookup"><span data-stu-id="91fde-187">The preceding query returns a Hive array unlike get\_json\_object, which returns a string.</span></span>

![SerDe fråga 3][image-hdi-hivejson-serde_query3]

<span data-ttu-id="91fde-189">Om du vill skil felaktiga JSON sedan som beskrivs i den [wiki-sida](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) för den här SerDe kan du uppnå som genom att skriva följande kod:</span><span class="sxs-lookup"><span data-stu-id="91fde-189">If you want to skil malformed JSON, then as explained in the [wiki page](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) of this SerDe you can achieve that by typing the following code:</span></span>  

    ALTER TABLE json_table SET SERDEPROPERTIES ( "ignore.malformed.json" = "true");




## <a name="summary"></a><span data-ttu-id="91fde-190">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="91fde-190">Summary</span></span>
<span data-ttu-id="91fde-191">Sammanfattningsvis beror typ av JSON-operator i Hive som du väljer på ditt scenario.</span><span class="sxs-lookup"><span data-stu-id="91fde-191">In conclusion, the type of JSON operator in Hive that you choose depends on your scenario.</span></span> <span data-ttu-id="91fde-192">Om du har ett enkelt JSON-dokument och du bara ha ett fält för att leta upp på – du kan välja att använda Hive UDF get\_json\_objekt.</span><span class="sxs-lookup"><span data-stu-id="91fde-192">If you have a simple JSON document and you only have one field to look up on – you can choose to use the Hive UDF get\_json\_object.</span></span> <span data-ttu-id="91fde-193">Om du har fler än en nyckel för att leta upp kan du använda json_tuple.</span><span class="sxs-lookup"><span data-stu-id="91fde-193">If you have more than one key to look up on, then you can use json_tuple.</span></span> <span data-ttu-id="91fde-194">Om du har en kapslad dokument, bör du använda JSON-SerDe.</span><span class="sxs-lookup"><span data-stu-id="91fde-194">If you have a nested document, then you should use the JSON SerDe.</span></span>

## <a name="next-steps"></a><span data-ttu-id="91fde-195">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="91fde-195">Next steps</span></span>

<span data-ttu-id="91fde-196">Andra relaterade artiklar finns</span><span class="sxs-lookup"><span data-stu-id="91fde-196">For other related articles, see</span></span>

* [<span data-ttu-id="91fde-197">Använda Hive och HiveQL med Hadoop i HDInsight för att analysera ett exempel Apache log4j-fil</span><span class="sxs-lookup"><span data-stu-id="91fde-197">Use Hive and HiveQL with Hadoop in HDInsight to analyze a sample Apache log4j file</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="91fde-198">Analysera svarta fördröjning data med hjälp av Hive i HDInsight</span><span class="sxs-lookup"><span data-stu-id="91fde-198">Analyze flight delay data by using Hive in HDInsight</span></span>](hdinsight-analyze-flight-delay-data.md)
* [<span data-ttu-id="91fde-199">Analysera Twitter-data med Hive i HDInsight</span><span class="sxs-lookup"><span data-stu-id="91fde-199">Analyze Twitter data using Hive in HDInsight</span></span>](hdinsight-analyze-twitter-data.md)
* [<span data-ttu-id="91fde-200">Kör ett Hadoop-jobb med hjälp av Azure Cosmos DB och HDInsight</span><span class="sxs-lookup"><span data-stu-id="91fde-200">Run a Hadoop job using Azure Cosmos DB and HDInsight</span></span>](../documentdb/documentdb-run-hadoop-with-hdinsight.md)

[hdinsight-python]: hdinsight-python.md

[image-hdi-hivejson-flatten]: ./media/hdinsight-using-json-in-hive/flatten.png
[image-hdi-hivejson-getjsonobject]: ./media/hdinsight-using-json-in-hive/getjsonobject.png
[image-hdi-hivejson-jsontuple]: ./media/hdinsight-using-json-in-hive/jsontuple.png
[image-hdi-hivejson-jdk]: ./media/hdinsight-using-json-in-hive/jdk.png
[image-hdi-hivejson-maven]: ./media/hdinsight-using-json-in-hive/maven.png
[image-hdi-hivejson-serde]: ./media/hdinsight-using-json-in-hive/serde.png
[image-hdi-hivejson-addjar]: ./media/hdinsight-using-json-in-hive/addjar.png
[image-hdi-hivejson-serde_query1]: ./media/hdinsight-using-json-in-hive/serde_query1.png
[image-hdi-hivejson-serde_query2]: ./media/hdinsight-using-json-in-hive/serde_query2.png
[image-hdi-hivejson-serde_query3]: ./media/hdinsight-using-json-in-hive/serde_query3.png
[image-hdi-hivejson-serde_result]: ./media/hdinsight-using-json-in-hive/serde_result.png
