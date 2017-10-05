---
title: "Skapa Java MapReduce för Hadoop - Azure HDInsight | Microsoft Docs"
description: "Lär dig hur du använder Apache Maven för att skapa ett Java-baserad MapReduce-program och sedan köra den med Hadoop på Azure HDInsight."
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
ms.openlocfilehash: 11d63f22204eb2acb530378f53ac72f16a35a4f2
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="develop-java-mapreduce-programs-for-hadoop-on-hdinsight"></a><span data-ttu-id="23f79-103">Utveckla Java-MapReduce-program för Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="23f79-103">Develop Java MapReduce programs for Hadoop on HDInsight</span></span>

<span data-ttu-id="23f79-104">Lär dig hur du använder Apache Maven för att skapa ett Java-baserad MapReduce-program och sedan köra den med Hadoop på Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="23f79-104">Learn how to use Apache Maven to create a Java-based MapReduce application, then run it with Hadoop on Azure HDInsight.</span></span>

> [!NOTE]
> <span data-ttu-id="23f79-105">Det här exemplet har senast har testats på HDInsight 3,6.</span><span class="sxs-lookup"><span data-stu-id="23f79-105">This example was most recently tested on HDInsight 3.6.</span></span>

## <span data-ttu-id="23f79-106"><a name="prerequisites"></a>Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="23f79-106"><a name="prerequisites"></a>Prerequisites</span></span>

* <span data-ttu-id="23f79-107">[Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 8 eller senare (eller en motsvarande, till exempel OpenJDK).</span><span class="sxs-lookup"><span data-stu-id="23f79-107">[Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 8 or later (or an equivalent, such as OpenJDK).</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="23f79-108">HDInsight-version 3.4 och tidigare använda Java 7.</span><span class="sxs-lookup"><span data-stu-id="23f79-108">HDInsight versions 3.4 and earlier use Java 7.</span></span> <span data-ttu-id="23f79-109">HDInsight 3.5 och senare använder Java 8.</span><span class="sxs-lookup"><span data-stu-id="23f79-109">HDInsight 3.5 and greater uses Java 8.</span></span>

* [<span data-ttu-id="23f79-110">Apache Maven</span><span class="sxs-lookup"><span data-stu-id="23f79-110">Apache Maven</span></span>](http://maven.apache.org/)

## <a name="configure-development-environment"></a><span data-ttu-id="23f79-111">Konfigurera utvecklingsmiljön</span><span class="sxs-lookup"><span data-stu-id="23f79-111">Configure development environment</span></span>

<span data-ttu-id="23f79-112">Följande miljövariabler kan anges när du installerar Java och JDK.</span><span class="sxs-lookup"><span data-stu-id="23f79-112">The following environment variables may be set when you install Java and the JDK.</span></span> <span data-ttu-id="23f79-113">Dock bör du kontrollera att de finns och att de innehåller rätt värden för ditt system.</span><span class="sxs-lookup"><span data-stu-id="23f79-113">However, you should check that they exist and that they contain the correct values for your system.</span></span>

* <span data-ttu-id="23f79-114">`JAVA_HOME`-måste peka på den katalog där med Java runtime environment (JRE) har installerats.</span><span class="sxs-lookup"><span data-stu-id="23f79-114">`JAVA_HOME` - should point to the directory where the Java runtime environment (JRE) is installed.</span></span> <span data-ttu-id="23f79-115">På en OS X-, Unix- eller Linux-system, bör det till exempel ha ett värde som liknar `/usr/lib/jvm/java-7-oracle`.</span><span class="sxs-lookup"><span data-stu-id="23f79-115">For example, on an OS X, Unix or Linux system, it should have a value similar to `/usr/lib/jvm/java-7-oracle`.</span></span> <span data-ttu-id="23f79-116">I Windows, skulle det ha ett värde som liknar`c:\Program Files (x86)\Java\jre1.7`</span><span class="sxs-lookup"><span data-stu-id="23f79-116">In Windows, it would have a value similar to `c:\Program Files (x86)\Java\jre1.7`</span></span>

* <span data-ttu-id="23f79-117">`PATH`-bör innehålla följande sökvägar:</span><span class="sxs-lookup"><span data-stu-id="23f79-117">`PATH` - should contain the following paths:</span></span>
  
  * <span data-ttu-id="23f79-118">`JAVA_HOME`(eller motsvarande sökväg)</span><span class="sxs-lookup"><span data-stu-id="23f79-118">`JAVA_HOME` (or the equivalent path)</span></span>

  * <span data-ttu-id="23f79-119">`JAVA_HOME\bin`(eller motsvarande sökväg)</span><span class="sxs-lookup"><span data-stu-id="23f79-119">`JAVA_HOME\bin` (or the equivalent path)</span></span>

  * <span data-ttu-id="23f79-120">Katalogen där Maven är installerat</span><span class="sxs-lookup"><span data-stu-id="23f79-120">The directory where Maven is installed</span></span>

## <a name="create-a-maven-project"></a><span data-ttu-id="23f79-121">Skapa ett Maven-projekt</span><span class="sxs-lookup"><span data-stu-id="23f79-121">Create a Maven project</span></span>

1. <span data-ttu-id="23f79-122">Från en terminalsession eller Kommandotolken i din utvecklingsmiljö, ändra kataloger till platsen som du vill lagra det här projektet.</span><span class="sxs-lookup"><span data-stu-id="23f79-122">From a terminal session, or command line in your development environment, change directories to the location you want to store this project.</span></span>

2. <span data-ttu-id="23f79-123">Använd den `mvn` kommandot, som installeras med Maven för att generera scaffold-teknik för projektet.</span><span class="sxs-lookup"><span data-stu-id="23f79-123">Use the `mvn` command, which is installed with Maven, to generate the scaffolding for the project.</span></span>

   ```bash
   mvn archetype:generate -DgroupId=org.apache.hadoop.examples -DartifactId=wordcountjava -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
   ```

    > [!NOTE]
    > <span data-ttu-id="23f79-124">Om du använder PowerShell kan du ange den `-D` parametrar inom dubbla citattecken.</span><span class="sxs-lookup"><span data-stu-id="23f79-124">If you are using PowerShell, you must enclose the `-D` parameters in double quotes.</span></span>
    >
    > `mvn archetype:generate "-DgroupId=org.apache.hadoop.examples" "-DartifactId=wordcountjava" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`

    <span data-ttu-id="23f79-125">Det här kommandot skapar en katalog med det namn som anges av den `artifactID` parameter (**wordcountjava** i det här exemplet.) Den här katalogen innehåller följande:</span><span class="sxs-lookup"><span data-stu-id="23f79-125">This command creates a directory with the name specified by the `artifactID` parameter (**wordcountjava** in this example.) This directory contains the following items:</span></span>

   * <span data-ttu-id="23f79-126">`pom.xml`- [Projekt Object Model (POM)](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html) som innehåller information och konfiguration information som används för att skapa projektet.</span><span class="sxs-lookup"><span data-stu-id="23f79-126">`pom.xml` - The [Project Object Model (POM)](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html) that contains information and configuration details used to build the project.</span></span>

   * <span data-ttu-id="23f79-127">`src`-Katalogen som innehåller programmet.</span><span class="sxs-lookup"><span data-stu-id="23f79-127">`src` - The directory that contains the application.</span></span>

3. <span data-ttu-id="23f79-128">Ta bort den `src/test/java/org/apache/hadoop/examples/apptest.java` filen.</span><span class="sxs-lookup"><span data-stu-id="23f79-128">Delete the `src/test/java/org/apache/hadoop/examples/apptest.java` file.</span></span> <span data-ttu-id="23f79-129">Den används inte i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="23f79-129">It is not used in this example.</span></span>

## <a name="add-dependencies"></a><span data-ttu-id="23f79-130">Lägga till beroenden</span><span class="sxs-lookup"><span data-stu-id="23f79-130">Add dependencies</span></span>

1. <span data-ttu-id="23f79-131">Redigera den `pom.xml` och Lägg till följande text i det `<dependencies>` avsnitt:</span><span class="sxs-lookup"><span data-stu-id="23f79-131">Edit the `pom.xml` file and add the following text inside the `<dependencies>` section:</span></span>
   
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

    <span data-ttu-id="23f79-132">Detta definierar bibliotek som krävs (anges i &lt;artefakt-ID\>) med en specifik version (listade i &lt;version\>).</span><span class="sxs-lookup"><span data-stu-id="23f79-132">This defines required libraries (listed within &lt;artifactId\>) with a specific version (listed within &lt;version\>).</span></span> <span data-ttu-id="23f79-133">Dessa beroenden hämtas vid kompileringen, från Maven standarddatabas.</span><span class="sxs-lookup"><span data-stu-id="23f79-133">At compile time, these dependencies are downloaded from the default Maven repository.</span></span> <span data-ttu-id="23f79-134">Du kan använda den [Maven databasen Sök](http://search.maven.org/#artifactdetails%7Corg.apache.hadoop%7Chadoop-mapreduce-examples%7C2.5.1%7Cjar) vill se mer.</span><span class="sxs-lookup"><span data-stu-id="23f79-134">You can use the [Maven repository search](http://search.maven.org/#artifactdetails%7Corg.apache.hadoop%7Chadoop-mapreduce-examples%7C2.5.1%7Cjar) to view more.</span></span>
   
    <span data-ttu-id="23f79-135">Den `<scope>provided</scope>` talar om Maven dessa beroenden inte paketeras med programmet som de som tillhandahålls av HDInsight-kluster under körning.</span><span class="sxs-lookup"><span data-stu-id="23f79-135">The `<scope>provided</scope>` tells Maven that these dependencies should not be packaged with the application, as they are provided by the HDInsight cluster at run-time.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="23f79-136">Den version som används ska matcha versionen av Hadoop finns på ditt kluster.</span><span class="sxs-lookup"><span data-stu-id="23f79-136">The version used should match the version of Hadoop present on your cluster.</span></span> <span data-ttu-id="23f79-137">Mer information om versioner finns i [HDInsight component-versioning](hdinsight-component-versioning.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="23f79-137">For more information on versions, see the [HDInsight component versioning](hdinsight-component-versioning.md) document.</span></span>

2. <span data-ttu-id="23f79-138">Lägg till följande för att den `pom.xml` filen.</span><span class="sxs-lookup"><span data-stu-id="23f79-138">Add the following to the `pom.xml` file.</span></span> <span data-ttu-id="23f79-139">Den här texten måste finnas i den `<project>...</project>` taggar i filen, till exempel mellan `</dependencies>` och `</project>`.</span><span class="sxs-lookup"><span data-stu-id="23f79-139">This text must be inside the `<project>...</project>` tags in the file; for example, between `</dependencies>` and `</project>`.</span></span>

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

    <span data-ttu-id="23f79-140">Första plugin-programmet konfigurerar den [Maven skugga Plugin](http://maven.apache.org/plugins/maven-shade-plugin/), som används för att skapa en uberjar (kallas ibland ett fatjar) som innehåller beroenden som krävs för programmet.</span><span class="sxs-lookup"><span data-stu-id="23f79-140">The first plugin configures the [Maven Shade Plugin](http://maven.apache.org/plugins/maven-shade-plugin/), which is used to build an uberjar (sometimes called a fatjar), which contains dependencies required by the application.</span></span> <span data-ttu-id="23f79-141">Den också förhindrar duplicering av licenser i jar-paketet, vilket kan orsaka problem på vissa datorer.</span><span class="sxs-lookup"><span data-stu-id="23f79-141">It also prevents duplication of licenses within the jar package, which can cause problems on some systems.</span></span>

    <span data-ttu-id="23f79-142">Andra plugin-programmet konfigurerar Java målversionen.</span><span class="sxs-lookup"><span data-stu-id="23f79-142">The second plugin configures the target Java version.</span></span>

    > [!NOTE]
    > <span data-ttu-id="23f79-143">HDInsight 3.4 och tidigare användning Java 7.</span><span class="sxs-lookup"><span data-stu-id="23f79-143">HDInsight 3.4 and earlier use Java 7.</span></span> <span data-ttu-id="23f79-144">HDInsight 3.5 och senare använder Java 8.</span><span class="sxs-lookup"><span data-stu-id="23f79-144">HDInsight 3.5 and greater uses Java 8.</span></span>

3. <span data-ttu-id="23f79-145">Spara den `pom.xml` filen.</span><span class="sxs-lookup"><span data-stu-id="23f79-145">Save the `pom.xml` file.</span></span>

## <a name="create-the-mapreduce-application"></a><span data-ttu-id="23f79-146">Skapa MapReduce-program</span><span class="sxs-lookup"><span data-stu-id="23f79-146">Create the MapReduce application</span></span>

1. <span data-ttu-id="23f79-147">Gå till den `wordcountjava/src/main/java/org/apache/hadoop/examples` katalog och Byt namn på den `App.java` filen till `WordCount.java`.</span><span class="sxs-lookup"><span data-stu-id="23f79-147">Go to the `wordcountjava/src/main/java/org/apache/hadoop/examples` directory and rename the `App.java` file to `WordCount.java`.</span></span>

2. <span data-ttu-id="23f79-148">Öppna den `WordCount.java` filen i en textredigerare och Ersätt det med följande text:</span><span class="sxs-lookup"><span data-stu-id="23f79-148">Open the `WordCount.java` file in a text editor and replace the contents with the following text:</span></span>
   
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
   
    <span data-ttu-id="23f79-149">Lägg märke till paketnamnet `org.apache.hadoop.examples` och klassnamnet har `WordCount`.</span><span class="sxs-lookup"><span data-stu-id="23f79-149">Notice the package name is `org.apache.hadoop.examples` and the class name is `WordCount`.</span></span> <span data-ttu-id="23f79-150">Du använder dessa namn när du skickar MapReduce-jobb.</span><span class="sxs-lookup"><span data-stu-id="23f79-150">You use these names when you submit the MapReduce job.</span></span>

3. <span data-ttu-id="23f79-151">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="23f79-151">Save the file.</span></span>

## <a name="build-the-application"></a><span data-ttu-id="23f79-152">Skapa programmet</span><span class="sxs-lookup"><span data-stu-id="23f79-152">Build the application</span></span>

1. <span data-ttu-id="23f79-153">Ändra till den `wordcountjava` directory om du inte redan har det.</span><span class="sxs-lookup"><span data-stu-id="23f79-153">Change to the `wordcountjava` directory, if you are not already there.</span></span>

2. <span data-ttu-id="23f79-154">Använd följande kommando för att skapa en JAR-fil som innehåller programmet:</span><span class="sxs-lookup"><span data-stu-id="23f79-154">Use the following command to build a JAR file containing the application:</span></span>

   ```
   mvn clean package
   ```

    <span data-ttu-id="23f79-155">Det här kommandot rensar alla tidigare build-artefakter, laddar ned alla beroenden som inte redan installerats, och sedan versioner och paketet programmet.</span><span class="sxs-lookup"><span data-stu-id="23f79-155">This command cleans any previous build artifacts, downloads any dependencies that have not already been installed, and then builds and package the application.</span></span>

3. <span data-ttu-id="23f79-156">När kommandot har slutförts i `wordcountjava/target` katalogen innehåller en fil med namnet `wordcountjava-1.0-SNAPSHOT.jar`.</span><span class="sxs-lookup"><span data-stu-id="23f79-156">Once the command finishes, the `wordcountjava/target` directory contains a file named `wordcountjava-1.0-SNAPSHOT.jar`.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="23f79-157">Den `wordcountjava-1.0-SNAPSHOT.jar` filen är en uberjar som innehåller inte bara den WordCount jobb, utan även beroenden som krävs vid körning.</span><span class="sxs-lookup"><span data-stu-id="23f79-157">The `wordcountjava-1.0-SNAPSHOT.jar` file is an uberjar, which contains not only the WordCount job, but also dependencies that the job requires at runtime.</span></span>

## <span data-ttu-id="23f79-158"><a id="upload"></a>Överför jar</span><span class="sxs-lookup"><span data-stu-id="23f79-158"><a id="upload"></a>Upload the jar</span></span>

<span data-ttu-id="23f79-159">Använd följande kommando för att överföra jar-filen till HDInsight-headnode:</span><span class="sxs-lookup"><span data-stu-id="23f79-159">Use the following command to upload the jar file to the HDInsight headnode:</span></span>

   ```bash
   scp target/wordcountjava-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:
   ```

    Replace __USERNAME__ with your SSH user name for the cluster. Replace __CLUSTERNAME__ with the HDInsight cluster name.

<span data-ttu-id="23f79-160">Det här kommandot kopieras filerna från det lokala systemet till huvudnod.</span><span class="sxs-lookup"><span data-stu-id="23f79-160">This command copies the files from the local system to the head node.</span></span> <span data-ttu-id="23f79-161">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="23f79-161">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <span data-ttu-id="23f79-162"><a name="run"></a>Kör MapReduce-jobb på Hadoop</span><span class="sxs-lookup"><span data-stu-id="23f79-162"><a name="run"></a>Run the MapReduce job on Hadoop</span></span>

1. <span data-ttu-id="23f79-163">Anslut till HDInsight med hjälp av SSH.</span><span class="sxs-lookup"><span data-stu-id="23f79-163">Connect to HDInsight using SSH.</span></span> <span data-ttu-id="23f79-164">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="23f79-164">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="23f79-165">Använd följande kommando för att köra programmet MapReduce från SSH-session:</span><span class="sxs-lookup"><span data-stu-id="23f79-165">From the SSH session, use the following command to run the MapReduce application:</span></span>
   
   ```bash
   yarn jar wordcountjava-1.0-SNAPSHOT.jar org.apache.hadoop.examples.WordCount /example/data/gutenberg/davinci.txt /example/data/wordcountout
   ```
   
    <span data-ttu-id="23f79-166">Detta kommando startar WordCount MapReduce-programmet.</span><span class="sxs-lookup"><span data-stu-id="23f79-166">This command starts the WordCount MapReduce application.</span></span> <span data-ttu-id="23f79-167">Indatafilen är `/example/data/gutenberg/davinci.txt`, och den angivna katalogen är `/example/data/wordcountout`.</span><span class="sxs-lookup"><span data-stu-id="23f79-167">The input file is `/example/data/gutenberg/davinci.txt`, and the output directory is `/example/data/wordcountout`.</span></span> <span data-ttu-id="23f79-168">Både filen med indata och utdata lagras till standardlagring för klustret.</span><span class="sxs-lookup"><span data-stu-id="23f79-168">Both the input file and output are stored to the default storage for the cluster.</span></span>

3. <span data-ttu-id="23f79-169">När jobbet har slutförts använder du följande kommando för att visa resultatet:</span><span class="sxs-lookup"><span data-stu-id="23f79-169">Once the job completes, use the following command to view the results:</span></span>
   
   ```bash
   hdfs dfs -cat /example/data/wordcountout/*
   ```

    <span data-ttu-id="23f79-170">Du bör få en lista över ord och antal med värden som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="23f79-170">You should receive a list of words and counts, with values similar to the following text:</span></span>
   
        zeal    1
        zelus   1
        zenith  2

## <span data-ttu-id="23f79-171"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="23f79-171"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="23f79-172">I det här dokumentet har du lärt dig hur du utvecklar ett Java-MapReduce-jobb.</span><span class="sxs-lookup"><span data-stu-id="23f79-172">In this document, you have learned how to develop a Java MapReduce job.</span></span> <span data-ttu-id="23f79-173">Finns i följande dokument för andra sätt att arbeta med HDInsight.</span><span class="sxs-lookup"><span data-stu-id="23f79-173">See the following documents for other ways to work with HDInsight.</span></span>

* <span data-ttu-id="23f79-174">[Använda Hive med HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="23f79-174">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="23f79-175">[Använda Pig med HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="23f79-175">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* [<span data-ttu-id="23f79-176">Använda MapReduce med HDInsight</span><span class="sxs-lookup"><span data-stu-id="23f79-176">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="23f79-177">Mer information finns också i [Java-Utvecklingscenter](https://azure.microsoft.com/develop/java/).</span><span class="sxs-lookup"><span data-stu-id="23f79-177">For more information, see also the [Java Developer Center](https://azure.microsoft.com/develop/java/).</span></span>

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

