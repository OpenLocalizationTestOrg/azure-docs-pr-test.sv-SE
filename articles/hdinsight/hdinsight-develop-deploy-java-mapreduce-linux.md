---
title: "aaaCreate Java MapReduce för Hadoop - Azure HDInsight | Microsoft Docs"
description: "Lär dig hur toouse Apache Maven toocreate en Java-baserad MapReduce programmet sedan köra den med Hadoop på Azure HDInsight."
services: hdinsight
editor: cgronlun
manager: jhubbard
author: Blackmist
documentationcenter: 
tags: azure-portal
ms.assetid: 9ee6384c-cb61-4087-8273-fb53fa27c1c3
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 08/07/2017
ms.author: larryfr
ms.openlocfilehash: 903a57a482395f7da79002188399a4d6288ff0af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="develop-java-mapreduce-programs-for-hadoop-on-hdinsight"></a><span data-ttu-id="d8971-103">Utveckla Java-MapReduce-program för Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="d8971-103">Develop Java MapReduce programs for Hadoop on HDInsight</span></span>

<span data-ttu-id="d8971-104">Lär dig hur toouse Apache Maven toocreate en Java-baserad MapReduce programmet sedan köra den med Hadoop på Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d8971-104">Learn how toouse Apache Maven toocreate a Java-based MapReduce application, then run it with Hadoop on Azure HDInsight.</span></span>

> [!NOTE]
> <span data-ttu-id="d8971-105">Det här exemplet har senast har testats på HDInsight 3,6.</span><span class="sxs-lookup"><span data-stu-id="d8971-105">This example was most recently tested on HDInsight 3.6.</span></span>

## <span data-ttu-id="d8971-106"><a name="prerequisites"></a>Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="d8971-106"><a name="prerequisites"></a>Prerequisites</span></span>

* <span data-ttu-id="d8971-107">[Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 8 eller senare (eller en motsvarande, till exempel OpenJDK).</span><span class="sxs-lookup"><span data-stu-id="d8971-107">[Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 8 or later (or an equivalent, such as OpenJDK).</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="d8971-108">HDInsight-version 3.4 och tidigare använda Java 7.</span><span class="sxs-lookup"><span data-stu-id="d8971-108">HDInsight versions 3.4 and earlier use Java 7.</span></span> <span data-ttu-id="d8971-109">HDInsight 3.5 och senare använder Java 8.</span><span class="sxs-lookup"><span data-stu-id="d8971-109">HDInsight 3.5 and greater uses Java 8.</span></span>

* [<span data-ttu-id="d8971-110">Apache Maven</span><span class="sxs-lookup"><span data-stu-id="d8971-110">Apache Maven</span></span>](http://maven.apache.org/)

## <a name="configure-development-environment"></a><span data-ttu-id="d8971-111">Konfigurera utvecklingsmiljön</span><span class="sxs-lookup"><span data-stu-id="d8971-111">Configure development environment</span></span>

<span data-ttu-id="d8971-112">hello kan följande miljövariabler anges när du installerar Java och hello JDK.</span><span class="sxs-lookup"><span data-stu-id="d8971-112">hello following environment variables may be set when you install Java and hello JDK.</span></span> <span data-ttu-id="d8971-113">Dock bör du kontrollera att de finns och att de innehåller hello rätt värden för ditt system.</span><span class="sxs-lookup"><span data-stu-id="d8971-113">However, you should check that they exist and that they contain hello correct values for your system.</span></span>

* <span data-ttu-id="d8971-114">`JAVA_HOME`-måste peka toohello katalog där hello Java runtime environment (JRE) har installerats.</span><span class="sxs-lookup"><span data-stu-id="d8971-114">`JAVA_HOME` - should point toohello directory where hello Java runtime environment (JRE) is installed.</span></span> <span data-ttu-id="d8971-115">Till exempel på en OS X-, Unix- eller Linux-system, den måste ha ett värde liknande för`/usr/lib/jvm/java-7-oracle`.</span><span class="sxs-lookup"><span data-stu-id="d8971-115">For example, on an OS X, Unix or Linux system, it should have a value similar too`/usr/lib/jvm/java-7-oracle`.</span></span> <span data-ttu-id="d8971-116">I Windows, skulle det ha ett värde som är liknande för`c:\Program Files (x86)\Java\jre1.7`</span><span class="sxs-lookup"><span data-stu-id="d8971-116">In Windows, it would have a value similar too`c:\Program Files (x86)\Java\jre1.7`</span></span>

* <span data-ttu-id="d8971-117">`PATH`-bör innehålla hello följande sökvägar:</span><span class="sxs-lookup"><span data-stu-id="d8971-117">`PATH` - should contain hello following paths:</span></span>
  
  * <span data-ttu-id="d8971-118">`JAVA_HOME`(eller motsvarande hello-sökväg)</span><span class="sxs-lookup"><span data-stu-id="d8971-118">`JAVA_HOME` (or hello equivalent path)</span></span>

  * <span data-ttu-id="d8971-119">`JAVA_HOME\bin`(eller motsvarande hello-sökväg)</span><span class="sxs-lookup"><span data-stu-id="d8971-119">`JAVA_HOME\bin` (or hello equivalent path)</span></span>

  * <span data-ttu-id="d8971-120">hello katalog där Maven har installerats</span><span class="sxs-lookup"><span data-stu-id="d8971-120">hello directory where Maven is installed</span></span>

## <a name="create-a-maven-project"></a><span data-ttu-id="d8971-121">Skapa ett Maven-projekt</span><span class="sxs-lookup"><span data-stu-id="d8971-121">Create a Maven project</span></span>

1. <span data-ttu-id="d8971-122">Från en terminalsession eller Kommandotolken i din utvecklingsmiljö, ändra kataloger toohello plats toostore projektet.</span><span class="sxs-lookup"><span data-stu-id="d8971-122">From a terminal session, or command line in your development environment, change directories toohello location you want toostore this project.</span></span>

2. <span data-ttu-id="d8971-123">Använd hello `mvn` kommandot, som installeras med Maven toogenerate hello scaffold-teknik för hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="d8971-123">Use hello `mvn` command, which is installed with Maven, toogenerate hello scaffolding for hello project.</span></span>

   ```bash
   mvn archetype:generate -DgroupId=org.apache.hadoop.examples -DartifactId=wordcountjava -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
   ```

    > [!NOTE]
    > <span data-ttu-id="d8971-124">Om du använder PowerShell kan du ange hello `-D` parametrar inom dubbla citattecken.</span><span class="sxs-lookup"><span data-stu-id="d8971-124">If you are using PowerShell, you must enclose hello `-D` parameters in double quotes.</span></span>
    >
    > `mvn archetype:generate "-DgroupId=org.apache.hadoop.examples" "-DartifactId=wordcountjava" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`

    <span data-ttu-id="d8971-125">Det här kommandot skapar en katalog med hello namn anges av hello `artifactID` parameter (**wordcountjava** i det här exemplet.) Den här katalogen innehåller hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="d8971-125">This command creates a directory with hello name specified by hello `artifactID` parameter (**wordcountjava** in this example.) This directory contains hello following items:</span></span>

   * <span data-ttu-id="d8971-126">`pom.xml`-hello [projekt Object Model (POM)](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html) som innehåller information och konfigurationsinformation används toobuild hello projektet.</span><span class="sxs-lookup"><span data-stu-id="d8971-126">`pom.xml` - hello [Project Object Model (POM)](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html) that contains information and configuration details used toobuild hello project.</span></span>

   * <span data-ttu-id="d8971-127">`src`-hello-katalogen som innehåller programmet hello.</span><span class="sxs-lookup"><span data-stu-id="d8971-127">`src` - hello directory that contains hello application.</span></span>

3. <span data-ttu-id="d8971-128">Ta bort hello `src/test/java/org/apache/hadoop/examples/apptest.java` fil.</span><span class="sxs-lookup"><span data-stu-id="d8971-128">Delete hello `src/test/java/org/apache/hadoop/examples/apptest.java` file.</span></span> <span data-ttu-id="d8971-129">Den används inte i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="d8971-129">It is not used in this example.</span></span>

## <a name="add-dependencies"></a><span data-ttu-id="d8971-130">Lägga till beroenden</span><span class="sxs-lookup"><span data-stu-id="d8971-130">Add dependencies</span></span>

1. <span data-ttu-id="d8971-131">Redigera hello `pom.xml` och Lägg till följande text i hello hello `<dependencies>` avsnitt:</span><span class="sxs-lookup"><span data-stu-id="d8971-131">Edit hello `pom.xml` file and add hello following text inside hello `<dependencies>` section:</span></span>
   
   ```xml
    <dependency>
        <groupId>org.apache.hadoop</groupId>
        <artifactId>hadoop-mapreduce-examples</artifactId>
        <version>2.7.3</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>org.apache.hadoop</groupId>
        <artifactId>hadoop-mapreduce-client-common</artifactId>
        <version>2.7.3</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>org.apache.hadoop</groupId>
        <artifactId>hadoop-common</artifactId>
        <version>2.7.3</version>
        <scope>provided</scope>
    </dependency>
   ```

    <span data-ttu-id="d8971-132">Detta definierar bibliotek som krävs (anges i &lt;artefakt-ID\>) med en specifik version (listade i &lt;version\>).</span><span class="sxs-lookup"><span data-stu-id="d8971-132">This defines required libraries (listed within &lt;artifactId\>) with a specific version (listed within &lt;version\>).</span></span> <span data-ttu-id="d8971-133">Dessa beroenden hämtas vid kompileringen, från hello standarddatabas Maven.</span><span class="sxs-lookup"><span data-stu-id="d8971-133">At compile time, these dependencies are downloaded from hello default Maven repository.</span></span> <span data-ttu-id="d8971-134">Du kan använda hello [Maven databasen Sök](http://search.maven.org/#artifactdetails%7Corg.apache.hadoop%7Chadoop-mapreduce-examples%7C2.5.1%7Cjar) tooview mer.</span><span class="sxs-lookup"><span data-stu-id="d8971-134">You can use hello [Maven repository search](http://search.maven.org/#artifactdetails%7Corg.apache.hadoop%7Chadoop-mapreduce-examples%7C2.5.1%7Cjar) tooview more.</span></span>
   
    <span data-ttu-id="d8971-135">Hej `<scope>provided</scope>` talar om Maven dessa beroenden inte paketeras med hello program som de som tillhandahålls av HDInsight-kluster i hello vid körning.</span><span class="sxs-lookup"><span data-stu-id="d8971-135">hello `<scope>provided</scope>` tells Maven that these dependencies should not be packaged with hello application, as they are provided by hello HDInsight cluster at run-time.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="d8971-136">hello-version som används ska matcha hello version av Hadoop finns på ditt kluster.</span><span class="sxs-lookup"><span data-stu-id="d8971-136">hello version used should match hello version of Hadoop present on your cluster.</span></span> <span data-ttu-id="d8971-137">Mer information om versioner finns hello [HDInsight component-versioning](hdinsight-component-versioning.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="d8971-137">For more information on versions, see hello [HDInsight component versioning](hdinsight-component-versioning.md) document.</span></span>

2. <span data-ttu-id="d8971-138">Lägg till följande toohello hello `pom.xml` fil.</span><span class="sxs-lookup"><span data-stu-id="d8971-138">Add hello following toohello `pom.xml` file.</span></span> <span data-ttu-id="d8971-139">Den här texten måste finnas i hello `<project>...</project>` taggar i hello-filen, till exempel mellan `</dependencies>` och `</project>`.</span><span class="sxs-lookup"><span data-stu-id="d8971-139">This text must be inside hello `<project>...</project>` tags in hello file; for example, between `</dependencies>` and `</project>`.</span></span>

   ```xml
    <build>
        <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-shade-plugin</artifactId>
            <version>2.3</version>
            <configuration>
            <transformers>
                <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                </transformer>
            </transformers>
            </configuration>
            <executions>
            <execution>
                <phase>package</phase>
                    <goals>
                    <goal>shade</goal>
                    </goals>
            </execution>
            </executions>
            </plugin>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.6.1</version>
            <configuration>
            <source>1.8</source>
            <target>1.8</target>
            </configuration>
        </plugin>
        </plugins>
    </build>
   ```

    <span data-ttu-id="d8971-140">hello första plugin konfigurerar hello [Maven skugga Plugin](http://maven.apache.org/plugins/maven-shade-plugin/), vilket är används toobuild en uberjar (kallas ibland ett fatjar) som innehåller beroenden som krävs av hello program.</span><span class="sxs-lookup"><span data-stu-id="d8971-140">hello first plugin configures hello [Maven Shade Plugin](http://maven.apache.org/plugins/maven-shade-plugin/), which is used toobuild an uberjar (sometimes called a fatjar), which contains dependencies required by hello application.</span></span> <span data-ttu-id="d8971-141">Den också förhindrar duplicering av licenser i hello jar-paket, vilket kan orsaka problem på vissa datorer.</span><span class="sxs-lookup"><span data-stu-id="d8971-141">It also prevents duplication of licenses within hello jar package, which can cause problems on some systems.</span></span>

    <span data-ttu-id="d8971-142">hello andra plugin konfigurerar hello målversionen Java.</span><span class="sxs-lookup"><span data-stu-id="d8971-142">hello second plugin configures hello target Java version.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d8971-143">HDInsight 3.4 och tidigare användning Java 7.</span><span class="sxs-lookup"><span data-stu-id="d8971-143">HDInsight 3.4 and earlier use Java 7.</span></span> <span data-ttu-id="d8971-144">HDInsight 3.5 och senare använder Java 8.</span><span class="sxs-lookup"><span data-stu-id="d8971-144">HDInsight 3.5 and greater uses Java 8.</span></span>

3. <span data-ttu-id="d8971-145">Spara hello `pom.xml` fil.</span><span class="sxs-lookup"><span data-stu-id="d8971-145">Save hello `pom.xml` file.</span></span>

## <a name="create-hello-mapreduce-application"></a><span data-ttu-id="d8971-146">Skapa hello MapReduce-program</span><span class="sxs-lookup"><span data-stu-id="d8971-146">Create hello MapReduce application</span></span>

1. <span data-ttu-id="d8971-147">Gå toohello `wordcountjava/src/main/java/org/apache/hadoop/examples` katalog och Byt namn på hello `App.java` filen för`WordCount.java`.</span><span class="sxs-lookup"><span data-stu-id="d8971-147">Go toohello `wordcountjava/src/main/java/org/apache/hadoop/examples` directory and rename hello `App.java` file too`WordCount.java`.</span></span>

2. <span data-ttu-id="d8971-148">Öppna hello `WordCount.java` filen i en textredigerare och Ersätt hello innehållet med hello följande text:</span><span class="sxs-lookup"><span data-stu-id="d8971-148">Open hello `WordCount.java` file in a text editor and replace hello contents with hello following text:</span></span>
   
    ```java
    package org.apache.hadoop.examples;

    import java.io.IOException;
    import java.util.StringTokenizer;
    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.IntWritable;
    import org.apache.hadoop.io.Text;
    import org.apache.hadoop.mapreduce.Job;
    import org.apache.hadoop.mapreduce.Mapper;
    import org.apache.hadoop.mapreduce.Reducer;
    import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
    import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
    import org.apache.hadoop.util.GenericOptionsParser;

    public class WordCount {

        public static class TokenizerMapper
            extends Mapper<Object, Text, Text, IntWritable>{

        private final static IntWritable one = new IntWritable(1);
        private Text word = new Text();

        public void map(Object key, Text value, Context context
                        ) throws IOException, InterruptedException {
            StringTokenizer itr = new StringTokenizer(value.toString());
            while (itr.hasMoreTokens()) {
            word.set(itr.nextToken());
            context.write(word, one);
            }
        }
    }

    public static class IntSumReducer
            extends Reducer<Text,IntWritable,Text,IntWritable> {
        private IntWritable result = new IntWritable();

        public void reduce(Text key, Iterable<IntWritable> values,
                            Context context
                            ) throws IOException, InterruptedException {
            int sum = 0;
            for (IntWritable val : values) {
            sum += val.get();
            }
            result.set(sum);
            context.write(key, result);
        }
    }

    public static void main(String[] args) throws Exception {
        Configuration conf = new Configuration();
        String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
        if (otherArgs.length != 2) {
            System.err.println("Usage: wordcount <in> <out>");
            System.exit(2);
        }
        Job job = new Job(conf, "word count");
        job.setJarByClass(WordCount.class);
        job.setMapperClass(TokenizerMapper.class);
        job.setCombinerClass(IntSumReducer.class);
        job.setReducerClass(IntSumReducer.class);
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(IntWritable.class);
        FileInputFormat.addInputPath(job, new Path(otherArgs[0]));
        FileOutputFormat.setOutputPath(job, new Path(otherArgs[1]));
        System.exit(job.waitForCompletion(true) ? 0 : 1);
        }
    }
    ```
   
    <span data-ttu-id="d8971-149">Meddelande hello paketnamnet är `org.apache.hadoop.examples` och hello klassnamn är `WordCount`.</span><span class="sxs-lookup"><span data-stu-id="d8971-149">Notice hello package name is `org.apache.hadoop.examples` and hello class name is `WordCount`.</span></span> <span data-ttu-id="d8971-150">Du kan använda dessa namn när du skickar hello MapReduce-jobb.</span><span class="sxs-lookup"><span data-stu-id="d8971-150">You use these names when you submit hello MapReduce job.</span></span>

3. <span data-ttu-id="d8971-151">Spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="d8971-151">Save hello file.</span></span>

## <a name="build-hello-application"></a><span data-ttu-id="d8971-152">Skapa hello program</span><span class="sxs-lookup"><span data-stu-id="d8971-152">Build hello application</span></span>

1. <span data-ttu-id="d8971-153">Ändra toohello `wordcountjava` directory om du inte redan har det.</span><span class="sxs-lookup"><span data-stu-id="d8971-153">Change toohello `wordcountjava` directory, if you are not already there.</span></span>

2. <span data-ttu-id="d8971-154">Använd följande kommando toobuild en JAR-filen som innehåller hello programmet hello:</span><span class="sxs-lookup"><span data-stu-id="d8971-154">Use hello following command toobuild a JAR file containing hello application:</span></span>

   ```
   mvn clean package
   ```

    <span data-ttu-id="d8971-155">Det här kommandot rensar alla tidigare build-artefakter, laddar ned alla beroenden som inte redan installerats, och sedan versioner och paketera hello programmet.</span><span class="sxs-lookup"><span data-stu-id="d8971-155">This command cleans any previous build artifacts, downloads any dependencies that have not already been installed, and then builds and package hello application.</span></span>

3. <span data-ttu-id="d8971-156">När hello kommandot har slutförts, hello `wordcountjava/target` katalogen innehåller en fil med namnet `wordcountjava-1.0-SNAPSHOT.jar`.</span><span class="sxs-lookup"><span data-stu-id="d8971-156">Once hello command finishes, hello `wordcountjava/target` directory contains a file named `wordcountjava-1.0-SNAPSHOT.jar`.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="d8971-157">Hej `wordcountjava-1.0-SNAPSHOT.jar` filen är en uberjar som innehåller inte bara hello WordCount jobb, men även beroenden som hello jobbet kräver vid körning.</span><span class="sxs-lookup"><span data-stu-id="d8971-157">hello `wordcountjava-1.0-SNAPSHOT.jar` file is an uberjar, which contains not only hello WordCount job, but also dependencies that hello job requires at runtime.</span></span>

## <span data-ttu-id="d8971-158"><a id="upload"></a>Överför hello jar</span><span class="sxs-lookup"><span data-stu-id="d8971-158"><a id="upload"></a>Upload hello jar</span></span>

<span data-ttu-id="d8971-159">Använd följande kommando tooupload hello jar-filen toohello HDInsight headnode hello:</span><span class="sxs-lookup"><span data-stu-id="d8971-159">Use hello following command tooupload hello jar file toohello HDInsight headnode:</span></span>

   ```bash
   scp target/wordcountjava-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:
   ```

    Replace __USERNAME__ with your SSH user name for hello cluster. Replace __CLUSTERNAME__ with hello HDInsight cluster name.

<span data-ttu-id="d8971-160">Det här kommandot kopierar hello filer från lokala system hello toohello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="d8971-160">This command copies hello files from hello local system toohello head node.</span></span> <span data-ttu-id="d8971-161">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="d8971-161">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <span data-ttu-id="d8971-162"><a name="run"></a>Kör hello MapReduce-jobb på Hadoop</span><span class="sxs-lookup"><span data-stu-id="d8971-162"><a name="run"></a>Run hello MapReduce job on Hadoop</span></span>

1. <span data-ttu-id="d8971-163">Ansluta tooHDInsight via SSH.</span><span class="sxs-lookup"><span data-stu-id="d8971-163">Connect tooHDInsight using SSH.</span></span> <span data-ttu-id="d8971-164">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="d8971-164">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="d8971-165">Använd följande kommando toorun hello MapReduce programmet hello från hello SSH-sessionen:</span><span class="sxs-lookup"><span data-stu-id="d8971-165">From hello SSH session, use hello following command toorun hello MapReduce application:</span></span>
   
   ```bash
   yarn jar wordcountjava-1.0-SNAPSHOT.jar org.apache.hadoop.examples.WordCount /example/data/gutenberg/davinci.txt /example/data/wordcountout
   ```
   
    <span data-ttu-id="d8971-166">Detta kommando startar hello WordCount MapReduce-programmet.</span><span class="sxs-lookup"><span data-stu-id="d8971-166">This command starts hello WordCount MapReduce application.</span></span> <span data-ttu-id="d8971-167">hello indatafilen är `/example/data/gutenberg/davinci.txt`, och hello målkatalogen är `/example/data/wordcountout`.</span><span class="sxs-lookup"><span data-stu-id="d8971-167">hello input file is `/example/data/gutenberg/davinci.txt`, and hello output directory is `/example/data/wordcountout`.</span></span> <span data-ttu-id="d8971-168">Både hello filen med indata och utdata är lagrade toohello standardlagring för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="d8971-168">Both hello input file and output are stored toohello default storage for hello cluster.</span></span>

3. <span data-ttu-id="d8971-169">När hello jobbet är slutfört, använder du följande kommandoresultat tooview hello hello:</span><span class="sxs-lookup"><span data-stu-id="d8971-169">Once hello job completes, use hello following command tooview hello results:</span></span>
   
   ```bash
   hdfs dfs -cat /example/data/wordcountout/*
   ```

    <span data-ttu-id="d8971-170">Du bör få en lista över ord och antal med värden liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="d8971-170">You should receive a list of words and counts, with values similar toohello following text:</span></span>
   
        zeal    1
        zelus   1
        zenith  2

## <span data-ttu-id="d8971-171"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d8971-171"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="d8971-172">I det här dokumentet har du lärt dig hur toodevelop ett Java-MapReduce-jobb.</span><span class="sxs-lookup"><span data-stu-id="d8971-172">In this document, you have learned how toodevelop a Java MapReduce job.</span></span> <span data-ttu-id="d8971-173">Se hello efter dokument för andra sätt toowork med HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d8971-173">See hello following documents for other ways toowork with HDInsight.</span></span>

* <span data-ttu-id="d8971-174">[Använda Hive med HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="d8971-174">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="d8971-175">[Använda Pig med HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="d8971-175">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* [<span data-ttu-id="d8971-176">Använda MapReduce med HDInsight</span><span class="sxs-lookup"><span data-stu-id="d8971-176">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="d8971-177">Mer information finns också hello [Java-Utvecklingscenter](https://azure.microsoft.com/develop/java/).</span><span class="sxs-lookup"><span data-stu-id="d8971-177">For more information, see also hello [Java Developer Center](https://azure.microsoft.com/develop/java/).</span></span>

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md

[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md

[powershell-PSCredential]: http://social.technet.microsoft.com/wiki/contents/articles/4546.working-with-passwords-secure-strings-and-credentials-in-windows-powershell.aspx

