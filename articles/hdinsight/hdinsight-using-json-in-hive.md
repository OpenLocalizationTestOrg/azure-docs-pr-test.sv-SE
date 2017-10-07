---
title: aaaAnalyze och processen JSON-dokument med Hive i HDInsight | Microsoft Docs
description: "Lär dig hur toouse JSON-dokument och analysera dem med Hive i HDInsight."
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
ms.openlocfilehash: b4b20172e8553f91a446615dc52f2ea2ef24cd04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="process-and-analyze-json-documents-using-hive-in-hdinsight"></a><span data-ttu-id="74477-103">Bearbeta och analysera JSON-dokument med hjälp av Hive i HDInsight</span><span class="sxs-lookup"><span data-stu-id="74477-103">Process and analyze JSON documents using Hive in HDInsight</span></span>

<span data-ttu-id="74477-104">Lär dig hur tooprocess och analysera JSON-filer med hjälp av Hive i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="74477-104">Learn how tooprocess and analyze JSON files using Hive in HDInsight.</span></span> <span data-ttu-id="74477-105">hello följande JSON-dokumentet används i självstudiekursen hello:</span><span class="sxs-lookup"><span data-stu-id="74477-105">hello following JSON document is used in hello tutorial:</span></span>

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

<span data-ttu-id="74477-106">hello-filen finns på wasb://processjson@hditutorialdata.blob.core.windows.net/.</span><span class="sxs-lookup"><span data-stu-id="74477-106">hello file can be found at wasb://processjson@hditutorialdata.blob.core.windows.net/.</span></span> <span data-ttu-id="74477-107">Mer information om hur du använder Azure Blob storage med HDInsight finns [använda HDFS-kompatibla Azure Blob storage med Hadoop i HDInsight](hdinsight-hadoop-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="74477-107">For more information on using Azure Blob storage with HDInsight, see [Use HDFS-compatible Azure Blob storage with Hadoop in HDInsight](hdinsight-hadoop-use-blob-storage.md).</span></span> <span data-ttu-id="74477-108">Du kan kopiera filen hello toohello standardbehållaren på klustret.</span><span class="sxs-lookup"><span data-stu-id="74477-108">You can copy hello file toohello default container of your cluster.</span></span>

<span data-ttu-id="74477-109">I den här kursen använder du hello Hive-konsolen.</span><span class="sxs-lookup"><span data-stu-id="74477-109">In this tutorial, you use hello Hive console.</span></span>  <span data-ttu-id="74477-110">Instruktioner för att öppna hello Hive-konsolen finns i [använda Hive med Hadoop i HDInsight med fjärrskrivbord](hdinsight-hadoop-use-hive-remote-desktop.md).</span><span class="sxs-lookup"><span data-stu-id="74477-110">For instructions of opening hello Hive console, see [Use Hive with Hadoop on HDInsight with Remote Desktop](hdinsight-hadoop-use-hive-remote-desktop.md).</span></span>

## <a name="flatten-json-documents"></a><span data-ttu-id="74477-111">Förenkla JSON-dokument</span><span class="sxs-lookup"><span data-stu-id="74477-111">Flatten JSON documents</span></span>
<span data-ttu-id="74477-112">hello-metoder som anges i nästa avsnitt av hello kräver hello JSON-dokument i en enda rad.</span><span class="sxs-lookup"><span data-stu-id="74477-112">hello methods listed in hello next section require hello JSON document in a single row.</span></span> <span data-ttu-id="74477-113">Därför måste du förenkla hello JSON-dokument tooa sträng.</span><span class="sxs-lookup"><span data-stu-id="74477-113">So you must flatten hello JSON document tooa string.</span></span> <span data-ttu-id="74477-114">Om JSON-dokumentet redan förenklas, kan du hoppa över detta steg och fortsätter raka toohello nästa avsnitt analysera JSON-data.</span><span class="sxs-lookup"><span data-stu-id="74477-114">If your JSON document is already flattened, you can skip this step and go straight toohello next section on Analyzing JSON data.</span></span>

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

<span data-ttu-id="74477-115">hello raw JSON-filen finns på  **wasb://processjson@hditutorialdata.blob.core.windows.net/** .</span><span class="sxs-lookup"><span data-stu-id="74477-115">hello raw JSON file is located at **wasb://processjson@hditutorialdata.blob.core.windows.net/**.</span></span> <span data-ttu-id="74477-116">Hej *StudentsRaw* Hive-tabell pekar toohello raw förenklade JSON-dokumentet.</span><span class="sxs-lookup"><span data-stu-id="74477-116">hello *StudentsRaw* Hive table points toohello raw unflattened JSON document.</span></span>

<span data-ttu-id="74477-117">Hej *StudentsOneLine* Hive-tabell lagrar hello data i hello HDInsight standardfilsystem under hello */json/studenter/* sökväg.</span><span class="sxs-lookup"><span data-stu-id="74477-117">hello *StudentsOneLine* Hive table stores hello data in hello HDInsight default file system under hello */json/students/* path.</span></span>

<span data-ttu-id="74477-118">hello INSERT-instruktionen fyller hello StudentOneLine tabell med hello förenklas JSON-data.</span><span class="sxs-lookup"><span data-stu-id="74477-118">hello INSERT statement populates hello StudentOneLine table with hello flattened JSON data.</span></span>

<span data-ttu-id="74477-119">hello SELECT-instruktionen kan bara returnera en rad.</span><span class="sxs-lookup"><span data-stu-id="74477-119">hello SELECT statement shall only return one row.</span></span>

<span data-ttu-id="74477-120">Här är hello utdata från hello SELECT-instruktion:</span><span class="sxs-lookup"><span data-stu-id="74477-120">Here is hello output of hello SELECT statement:</span></span>

![Förenkling av hello JSON-dokumentet.][image-hdi-hivejson-flatten]

## <a name="analyze-json-documents-in-hive"></a><span data-ttu-id="74477-122">Analysera JSON-dokument i Hive</span><span class="sxs-lookup"><span data-stu-id="74477-122">Analyze JSON documents in Hive</span></span>
<span data-ttu-id="74477-123">Hive innehåller tre olika mekanismer toorun frågor på JSON-dokument:</span><span class="sxs-lookup"><span data-stu-id="74477-123">Hive provides three different mechanisms toorun queries on JSON documents:</span></span>

* <span data-ttu-id="74477-124">Använd hello GET\_JSON\_OBJEKTET UDF (användardefinierad funktion)</span><span class="sxs-lookup"><span data-stu-id="74477-124">use hello GET\_JSON\_OBJECT UDF (User-defined function)</span></span>
* <span data-ttu-id="74477-125">Använd hello JSON_TUPLE UDF</span><span class="sxs-lookup"><span data-stu-id="74477-125">use hello JSON_TUPLE UDF</span></span>
* <span data-ttu-id="74477-126">Använda anpassade SerDe</span><span class="sxs-lookup"><span data-stu-id="74477-126">use custom SerDe</span></span>
* <span data-ttu-id="74477-127">skriva du äger UDF med Python eller andra språk.</span><span class="sxs-lookup"><span data-stu-id="74477-127">write you own UDF using Python or other languages.</span></span> <span data-ttu-id="74477-128">Se [i den här artikeln] [ hdinsight-python] på köra Python koden med Hive.</span><span class="sxs-lookup"><span data-stu-id="74477-128">See [this article][hdinsight-python] on running your own Python code with Hive.</span></span>

### <a name="use-hello-getjsonobject-udf"></a><span data-ttu-id="74477-129">Använd hello GET\_JSON_OBJECT UDF</span><span class="sxs-lookup"><span data-stu-id="74477-129">Use hello GET\_JSON_OBJECT UDF</span></span>
<span data-ttu-id="74477-130">Hive ger en inbyggda UDF kallas [hämta json-objekt](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object), som kan utföra JSON fråga under körning.</span><span class="sxs-lookup"><span data-stu-id="74477-130">Hive provides a built-in UDF called [get json object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object), which can perform JSON querying during run time.</span></span> <span data-ttu-id="74477-131">Den här metoden tar två argument – hello tabellnamnet och metodnamnet som har hello Flat JSON dokumentet och hello JSON fält som behöver toobe parsas.</span><span class="sxs-lookup"><span data-stu-id="74477-131">This method takes two arguments – hello table name and method name, which has hello flattened JSON document and hello JSON field that needs toobe parsed.</span></span> <span data-ttu-id="74477-132">Nu ska vi titta på en exempel-toosee hur detta UDF fungerar.</span><span class="sxs-lookup"><span data-stu-id="74477-132">Let’s look at an example toosee how this UDF works.</span></span>

<span data-ttu-id="74477-133">Hämta hello förnamn och efternamn för varje student</span><span class="sxs-lookup"><span data-stu-id="74477-133">Get hello first name and last name for each student</span></span>

    SELECT
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.FirstName'),
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.LastName')
    FROM StudentsOneLine;

<span data-ttu-id="74477-134">Här är hello utdata när du kör den här frågan i konsolfönstret.</span><span class="sxs-lookup"><span data-stu-id="74477-134">Here is hello output when running this query in console window.</span></span>

![get_json_object UDF][image-hdi-hivejson-getjsonobject]

<span data-ttu-id="74477-136">Det finns några begränsningar av hello get-json_object UDF.</span><span class="sxs-lookup"><span data-stu-id="74477-136">There are a few limitations of hello get-json_object UDF.</span></span>

* <span data-ttu-id="74477-137">Eftersom varje fält i hello frågan kräver reparsing hello frågan, påverkar hello prestanda.</span><span class="sxs-lookup"><span data-stu-id="74477-137">Because each field in hello query requires reparsing hello query, it affects hello performance.</span></span>
* <span data-ttu-id="74477-138">Hämta\_JSON_OBJECT() returnerar hello strängrepresentation av en matris.</span><span class="sxs-lookup"><span data-stu-id="74477-138">GET\_JSON_OBJECT() returns hello string representation of an array.</span></span> <span data-ttu-id="74477-139">tooconvert denna matris tooa Hive matris har toouse reguljära uttryck tooreplace hello hakparenteserna ' [' och ']' och sedan också anropet delar tooget hello matris.</span><span class="sxs-lookup"><span data-stu-id="74477-139">tooconvert this array tooa Hive array, you have toouse regular expressions tooreplace hello square brackets ‘[‘ and ‘]’ and then also call split tooget hello array.</span></span>

<span data-ttu-id="74477-140">Det är därför hello Hive wiki rekommenderar att du använder json_tuple.</span><span class="sxs-lookup"><span data-stu-id="74477-140">This is why hello Hive wiki recommends using json_tuple.</span></span>  

### <a name="use-hello-jsontuple-udf"></a><span data-ttu-id="74477-141">Använd hello JSON_TUPLE UDF</span><span class="sxs-lookup"><span data-stu-id="74477-141">Use hello JSON_TUPLE UDF</span></span>
<span data-ttu-id="74477-142">En annan UDF som tillhandahålls av Hive kallas [json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple), vilken som presterar bättre än [get_ json _object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object).</span><span class="sxs-lookup"><span data-stu-id="74477-142">Another UDF provided by Hive is called [json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple), which performs better than [get_ json _object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object).</span></span> <span data-ttu-id="74477-143">Den här metoden tar en uppsättning nycklar och en JSON-sträng, och returnerar en tuppel av värden med hjälp av en funktion.</span><span class="sxs-lookup"><span data-stu-id="74477-143">This method takes a set of keys and a JSON string, and returns a tuple of values using one function.</span></span> <span data-ttu-id="74477-144">hello returnerar följande fråga hello student id och hello klass från hello JSON-dokument:</span><span class="sxs-lookup"><span data-stu-id="74477-144">hello following query returns hello student id and hello grade from hello JSON document:</span></span>

    SELECT q1.StudentId, q1.Grade
      FROM StudentsOneLine jt
      LATERAL VIEW JSON_TUPLE(jt.json_body, 'StudentId', 'Grade') q1
        AS StudentId, Grade;

<span data-ttu-id="74477-145">hello utdata från skriptet i hello Hive-konsolen:</span><span class="sxs-lookup"><span data-stu-id="74477-145">hello output of this script in hello Hive console:</span></span>

![json_tuple UDF][image-hdi-hivejson-jsontuple]

<span data-ttu-id="74477-147">JSON\_TUPPEL använder hello [lateral visa](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) syntax i Hive, vilket gör att json\_tuppel toocreate en virtuell tabell genom att använda hello UDT funktionen tooeach raden i hello ursprungliga tabellen.</span><span class="sxs-lookup"><span data-stu-id="74477-147">JSON\_TUPLE uses hello [lateral view](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) syntax in Hive, which allows json\_tuple toocreate a virtual table by applying hello UDT function tooeach row of hello original table.</span></span>  <span data-ttu-id="74477-148">Komplexa JSONs blir för ohanterlig på grund av hello upprepad användning av LATERALA VYN.</span><span class="sxs-lookup"><span data-stu-id="74477-148">Complex JSONs become too unwieldy because of hello repeated use of LATERAL VIEW.</span></span> <span data-ttu-id="74477-149">Dessutom kan inte JSON_TUPLE hantera kapslade JSONs.</span><span class="sxs-lookup"><span data-stu-id="74477-149">Furthermore, JSON_TUPLE cannot handle nested JSONs.</span></span>

### <a name="use-custom-serde"></a><span data-ttu-id="74477-150">Använda anpassade SerDe</span><span class="sxs-lookup"><span data-stu-id="74477-150">Use custom SerDe</span></span>
<span data-ttu-id="74477-151">SerDe hello bästa valet för parsning av kapslade JSON-dokument, kan du toodefine hello JSON-schema och Använd hello tooparse hello-schemadokument.</span><span class="sxs-lookup"><span data-stu-id="74477-151">SerDe is hello best choice for parsing nested JSON documents, it allows you toodefine hello JSON schema, and use hello schema tooparse hello documents.</span></span> <span data-ttu-id="74477-152">I kursen får du använder någon av hello mer populära SerDe som har utvecklats av [Roberto Congiu](https://github.com/rcongiu).</span><span class="sxs-lookup"><span data-stu-id="74477-152">In this tutorial, you use one of hello more popular SerDe that has been developed by [Roberto Congiu](https://github.com/rcongiu).</span></span>

<span data-ttu-id="74477-153">**toouse hello anpassade SerDe:**</span><span class="sxs-lookup"><span data-stu-id="74477-153">**toouse hello custom SerDe:**</span></span>

1. <span data-ttu-id="74477-154">Installera [Java SE Development sats 7u55 JDK 1.7.0_55](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u55-oth-JPR).</span><span class="sxs-lookup"><span data-stu-id="74477-154">Install [Java SE Development Kit 7u55 JDK 1.7.0_55](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u55-oth-JPR).</span></span> <span data-ttu-id="74477-155">Välj hello Windows X64 version av hello JDK om du ska toobe med hello Windows-distribution av HDInsight</span><span class="sxs-lookup"><span data-stu-id="74477-155">Choose hello Windows X64 version of hello JDK if you are going toobe using hello Windows deployment of HDInsight</span></span>
   
   > [!WARNING]
   > <span data-ttu-id="74477-156">JDK 1.8 fungerar inte med den här SerDe.</span><span class="sxs-lookup"><span data-stu-id="74477-156">JDK 1.8 doesn't work with this SerDe.</span></span>
   > 
   > 
   
    <span data-ttu-id="74477-157">Lägg till en ny miljövariabel för användaren när hello installationen är klar:</span><span class="sxs-lookup"><span data-stu-id="74477-157">After hello installation is completed, add a new user environment variable:</span></span>
   
   1. <span data-ttu-id="74477-158">Öppna **visa avancerade systeminställningar** från Windows hello-skärmen.</span><span class="sxs-lookup"><span data-stu-id="74477-158">Open **View advanced system settings** from hello Windows screen.</span></span>
   2. <span data-ttu-id="74477-159">Klicka på **miljövariabler**.</span><span class="sxs-lookup"><span data-stu-id="74477-159">Click **Environment Variables**.</span></span>  
   3. <span data-ttu-id="74477-160">Lägg till en ny **JAVA_HOME** miljövariabeln pekar för**C:\Program Files\Java\jdk1.7.0_55** eller var din JDK har installerats.</span><span class="sxs-lookup"><span data-stu-id="74477-160">Add a new **JAVA_HOME** environment variable is pointing too**C:\Program Files\Java\jdk1.7.0_55** or wherever your JDK is installed.</span></span>
      
      ![Konfigurera rätt konfigurationsvärden för JDK][image-hdi-hivejson-jdk]
2. <span data-ttu-id="74477-162">Installera [Maven 3.3.1](http://mirror.olnevhost.net/pub/apache/maven/maven-3/3.3.1/binaries/apache-maven-3.3.1-bin.zip)</span><span class="sxs-lookup"><span data-stu-id="74477-162">Install [Maven 3.3.1](http://mirror.olnevhost.net/pub/apache/maven/maven-3/3.3.1/binaries/apache-maven-3.3.1-bin.zip)</span></span>
   
    <span data-ttu-id="74477-163">Lägg till hello bin tooyour mappsökväg genom att gå tooControl panelen--> Redigera hello systemvariabler för ditt konto miljövariabler.</span><span class="sxs-lookup"><span data-stu-id="74477-163">Add hello bin folder tooyour path by going tooControl Panel-->Edit hello System Variables for your account Environment variables.</span></span> <span data-ttu-id="74477-164">hello följande skärmbild visas hur toodo detta.</span><span class="sxs-lookup"><span data-stu-id="74477-164">hello following screenshot shows you how toodo this.</span></span>
   
    ![Konfigurera Maven][image-hdi-hivejson-maven]
3. <span data-ttu-id="74477-166">Klona hello projektet från [Hive-JSON-SerDe](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) github-platsen.</span><span class="sxs-lookup"><span data-stu-id="74477-166">Clone hello project from [Hive-JSON-SerDe](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) github site.</span></span> <span data-ttu-id="74477-167">Du kan göra detta genom att klicka på ”Hämta Zip” hello-knappen som visas i följande skärmbild hello.</span><span class="sxs-lookup"><span data-stu-id="74477-167">You can do this by clicking on hello “Download Zip” button as shown in hello following screenshot.</span></span>
   
    ![Kloningen hello-projekt][image-hdi-hivejson-serde]

<span data-ttu-id="74477-169">4: gå toohello mappen där du har hämtat det här paketet och typ ”mvn package”.</span><span class="sxs-lookup"><span data-stu-id="74477-169">4: Go toohello folder where you have downloaded this package and then type “mvn package”.</span></span> <span data-ttu-id="74477-170">Detta bör skapa hello nödvändiga jar-filer som du sedan kan kopiera över toohello klustret.</span><span class="sxs-lookup"><span data-stu-id="74477-170">This should create hello necessary jar files that you can then copy over toohello cluster.</span></span>

<span data-ttu-id="74477-171">5: gå toohello målmappen hello rot i mappen dit du hämtade hello-paketet.</span><span class="sxs-lookup"><span data-stu-id="74477-171">5: Go toohello target folder under hello root folder where you downloaded hello package.</span></span> <span data-ttu-id="74477-172">Överför hello json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar filen toohead-nod i klustret.</span><span class="sxs-lookup"><span data-stu-id="74477-172">Upload hello json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar file toohead-node of your cluster.</span></span> <span data-ttu-id="74477-173">Jag vanligtvis placera den under hello hive binära mapp: C:\apps\dist\hive-0.13.0.2.1.11.0-2316\bin eller liknande.</span><span class="sxs-lookup"><span data-stu-id="74477-173">I usually put it under hello hive binary folder: C:\apps\dist\hive-0.13.0.2.1.11.0-2316\bin or something similar.</span></span>

<span data-ttu-id="74477-174">6: Skriv ”Lägg till jar /path/to/json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar” hello hive-fråga.</span><span class="sxs-lookup"><span data-stu-id="74477-174">6: In hello hive prompt, type “add jar /path/to/json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar”.</span></span> <span data-ttu-id="74477-175">Eftersom i min fallet hello jar i hello C:\apps\dist\hive-0.13.x\bin mapp kan jag direkt till hello jar med hello namnet som visas:</span><span class="sxs-lookup"><span data-stu-id="74477-175">Since in my case, hello jar is in hello C:\apps\dist\hive-0.13.x\bin folder, I can directly add hello jar with hello name as shown:</span></span>

    add jar json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar;

   ![Lägga till JAR tooyour projekt][image-hdi-hivejson-addjar]

<span data-ttu-id="74477-177">Du är nu redo toouse hello SerDe toorun frågor mot hello JSON-dokumentet.</span><span class="sxs-lookup"><span data-stu-id="74477-177">Now, you are ready toouse hello SerDe toorun queries against hello JSON document.</span></span>

<span data-ttu-id="74477-178">Följande instruktion hello skapar en tabell med ett definierat schema:</span><span class="sxs-lookup"><span data-stu-id="74477-178">hello following statement creates a table with a defined schema:</span></span>

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

<span data-ttu-id="74477-179">toolist hello förnamn och efternamn hello student</span><span class="sxs-lookup"><span data-stu-id="74477-179">toolist hello first name and last name of hello student</span></span>

    SELECT StudentDetails.FirstName, StudentDetails.LastName FROM json_table;

<span data-ttu-id="74477-180">Här är hello resultatet från hello Hive-konsolen.</span><span class="sxs-lookup"><span data-stu-id="74477-180">Here is hello result from hello Hive console.</span></span>

![SerDe frågan 1][image-hdi-hivejson-serde_query1]

<span data-ttu-id="74477-182">toocalculate hello summan av resultat av hello JSON-dokumentet</span><span class="sxs-lookup"><span data-stu-id="74477-182">toocalculate hello sum of scores of hello JSON document</span></span>

    SELECT SUM(scores)
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as scores;

<span data-ttu-id="74477-183">föregående fråga använder hello [lateral Visa Expandera](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) UDF tooexpand hello matris med resultat så att de kan summeras.</span><span class="sxs-lookup"><span data-stu-id="74477-183">hello preceding query uses [lateral view explode](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) UDF tooexpand hello array of scores so that they can be summed.</span></span>

<span data-ttu-id="74477-184">Här är hello utdata från hello Hive-konsolen.</span><span class="sxs-lookup"><span data-stu-id="74477-184">Here is hello output from hello Hive console.</span></span>

![SerDe fråga 2][image-hdi-hivejson-serde_query2]

<span data-ttu-id="74477-186">toofind som ämnen angivna student har fått fler än 80 poäng:</span><span class="sxs-lookup"><span data-stu-id="74477-186">toofind which subjects a given student has scored more than 80 points:</span></span>

    SELECT  
      jt.StudentClassCollection.ClassId
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as score  where score > 80;

<span data-ttu-id="74477-187">hello föregående fråga returnerar en Hive-matris till skillnad från get\_json\_-objekt som returnerar en sträng.</span><span class="sxs-lookup"><span data-stu-id="74477-187">hello preceding query returns a Hive array unlike get\_json\_object, which returns a string.</span></span>

![SerDe fråga 3][image-hdi-hivejson-serde_query3]

<span data-ttu-id="74477-189">Om du vill tooskil felaktig JSON sedan som förklaras i hello [wiki-sida](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) för den här SerDe kan du uppnå som genom att skriva följande kod hello:</span><span class="sxs-lookup"><span data-stu-id="74477-189">If you want tooskil malformed JSON, then as explained in hello [wiki page](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) of this SerDe you can achieve that by typing hello following code:</span></span>  

    ALTER TABLE json_table SET SERDEPROPERTIES ( "ignore.malformed.json" = "true");




## <a name="summary"></a><span data-ttu-id="74477-190">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="74477-190">Summary</span></span>
<span data-ttu-id="74477-191">Sammanfattningsvis hello typ av JSON-operator i struktur som du väljer beror på ditt scenario.</span><span class="sxs-lookup"><span data-stu-id="74477-191">In conclusion, hello type of JSON operator in Hive that you choose depends on your scenario.</span></span> <span data-ttu-id="74477-192">Om du har ett enkelt JSON-dokument och du bara har ett fält toolook på – du kan välja toouse hello Hive UDF get\_json\_objekt.</span><span class="sxs-lookup"><span data-stu-id="74477-192">If you have a simple JSON document and you only have one field toolook up on – you can choose toouse hello Hive UDF get\_json\_object.</span></span> <span data-ttu-id="74477-193">Om du har flera viktiga toolook på, kan du använda json_tuple.</span><span class="sxs-lookup"><span data-stu-id="74477-193">If you have more than one key toolook up on, then you can use json_tuple.</span></span> <span data-ttu-id="74477-194">Om du har en kapslad dokument, bör du använda hello JSON SerDe.</span><span class="sxs-lookup"><span data-stu-id="74477-194">If you have a nested document, then you should use hello JSON SerDe.</span></span>

## <a name="next-steps"></a><span data-ttu-id="74477-195">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="74477-195">Next steps</span></span>

<span data-ttu-id="74477-196">Andra relaterade artiklar finns</span><span class="sxs-lookup"><span data-stu-id="74477-196">For other related articles, see</span></span>

* [<span data-ttu-id="74477-197">Använda Hive och HiveQL med Hadoop i HDInsight tooanalyze ett exempel Apache log4j-fil</span><span class="sxs-lookup"><span data-stu-id="74477-197">Use Hive and HiveQL with Hadoop in HDInsight tooanalyze a sample Apache log4j file</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="74477-198">Analysera svarta fördröjning data med hjälp av Hive i HDInsight</span><span class="sxs-lookup"><span data-stu-id="74477-198">Analyze flight delay data by using Hive in HDInsight</span></span>](hdinsight-analyze-flight-delay-data.md)
* [<span data-ttu-id="74477-199">Analysera Twitter-data med Hive i HDInsight</span><span class="sxs-lookup"><span data-stu-id="74477-199">Analyze Twitter data using Hive in HDInsight</span></span>](hdinsight-analyze-twitter-data.md)
* [<span data-ttu-id="74477-200">Kör ett Hadoop-jobb med hjälp av Azure Cosmos DB och HDInsight</span><span class="sxs-lookup"><span data-stu-id="74477-200">Run a Hadoop job using Azure Cosmos DB and HDInsight</span></span>](../documentdb/documentdb-run-hadoop-with-hdinsight.md)

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
