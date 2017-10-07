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
# <a name="develop-java-mapreduce-programs-for-hadoop-on-hdinsight"></a>Utveckla Java-MapReduce-program för Hadoop i HDInsight

Lär dig hur toouse Apache Maven toocreate en Java-baserad MapReduce programmet sedan köra den med Hadoop på Azure HDInsight.

> [!NOTE]
> Det här exemplet har senast har testats på HDInsight 3,6.

## <a name="prerequisites"></a>Förhandskrav

* [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 8 eller senare (eller en motsvarande, till exempel OpenJDK).
    
    > [!NOTE]
    > HDInsight-version 3.4 och tidigare använda Java 7. HDInsight 3.5 och senare använder Java 8.

* [Apache Maven](http://maven.apache.org/)

## <a name="configure-development-environment"></a>Konfigurera utvecklingsmiljön

hello kan följande miljövariabler anges när du installerar Java och hello JDK. Dock bör du kontrollera att de finns och att de innehåller hello rätt värden för ditt system.

* `JAVA_HOME`-måste peka toohello katalog där hello Java runtime environment (JRE) har installerats. Till exempel på en OS X-, Unix- eller Linux-system, den måste ha ett värde liknande för`/usr/lib/jvm/java-7-oracle`. I Windows, skulle det ha ett värde som är liknande för`c:\Program Files (x86)\Java\jre1.7`

* `PATH`-bör innehålla hello följande sökvägar:
  
  * `JAVA_HOME`(eller motsvarande hello-sökväg)

  * `JAVA_HOME\bin`(eller motsvarande hello-sökväg)

  * hello katalog där Maven har installerats

## <a name="create-a-maven-project"></a>Skapa ett Maven-projekt

1. Från en terminalsession eller Kommandotolken i din utvecklingsmiljö, ändra kataloger toohello plats toostore projektet.

2. Använd hello `mvn` kommandot, som installeras med Maven toogenerate hello scaffold-teknik för hello-projekt.

   ```bash
   mvn archetype:generate -DgroupId=org.apache.hadoop.examples -DartifactId=wordcountjava -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
   ```

    > [!NOTE]
    > Om du använder PowerShell kan du ange hello `-D` parametrar inom dubbla citattecken.
    >
    > `mvn archetype:generate "-DgroupId=org.apache.hadoop.examples" "-DartifactId=wordcountjava" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`

    Det här kommandot skapar en katalog med hello namn anges av hello `artifactID` parameter (**wordcountjava** i det här exemplet.) Den här katalogen innehåller hello följande objekt:

   * `pom.xml`-hello [projekt Object Model (POM)](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html) som innehåller information och konfigurationsinformation används toobuild hello projektet.

   * `src`-hello-katalogen som innehåller programmet hello.

3. Ta bort hello `src/test/java/org/apache/hadoop/examples/apptest.java` fil. Den används inte i det här exemplet.

## <a name="add-dependencies"></a>Lägga till beroenden

1. Redigera hello `pom.xml` och Lägg till följande text i hello hello `<dependencies>` avsnitt:
   
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

    Detta definierar bibliotek som krävs (anges i &lt;artefakt-ID\>) med en specifik version (listade i &lt;version\>). Dessa beroenden hämtas vid kompileringen, från hello standarddatabas Maven. Du kan använda hello [Maven databasen Sök](http://search.maven.org/#artifactdetails%7Corg.apache.hadoop%7Chadoop-mapreduce-examples%7C2.5.1%7Cjar) tooview mer.
   
    Hej `<scope>provided</scope>` talar om Maven dessa beroenden inte paketeras med hello program som de som tillhandahålls av HDInsight-kluster i hello vid körning.

    > [!IMPORTANT]
    > hello-version som används ska matcha hello version av Hadoop finns på ditt kluster. Mer information om versioner finns hello [HDInsight component-versioning](hdinsight-component-versioning.md) dokumentet.

2. Lägg till följande toohello hello `pom.xml` fil. Den här texten måste finnas i hello `<project>...</project>` taggar i hello-filen, till exempel mellan `</dependencies>` och `</project>`.

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

    hello första plugin konfigurerar hello [Maven skugga Plugin](http://maven.apache.org/plugins/maven-shade-plugin/), vilket är används toobuild en uberjar (kallas ibland ett fatjar) som innehåller beroenden som krävs av hello program. Den också förhindrar duplicering av licenser i hello jar-paket, vilket kan orsaka problem på vissa datorer.

    hello andra plugin konfigurerar hello målversionen Java.

    > [!NOTE]
    > HDInsight 3.4 och tidigare användning Java 7. HDInsight 3.5 och senare använder Java 8.

3. Spara hello `pom.xml` fil.

## <a name="create-hello-mapreduce-application"></a>Skapa hello MapReduce-program

1. Gå toohello `wordcountjava/src/main/java/org/apache/hadoop/examples` katalog och Byt namn på hello `App.java` filen för`WordCount.java`.

2. Öppna hello `WordCount.java` filen i en textredigerare och Ersätt hello innehållet med hello följande text:
   
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
   
    Meddelande hello paketnamnet är `org.apache.hadoop.examples` och hello klassnamn är `WordCount`. Du kan använda dessa namn när du skickar hello MapReduce-jobb.

3. Spara hello-filen.

## <a name="build-hello-application"></a>Skapa hello program

1. Ändra toohello `wordcountjava` directory om du inte redan har det.

2. Använd följande kommando toobuild en JAR-filen som innehåller hello programmet hello:

   ```
   mvn clean package
   ```

    Det här kommandot rensar alla tidigare build-artefakter, laddar ned alla beroenden som inte redan installerats, och sedan versioner och paketera hello programmet.

3. När hello kommandot har slutförts, hello `wordcountjava/target` katalogen innehåller en fil med namnet `wordcountjava-1.0-SNAPSHOT.jar`.
   
   > [!NOTE]
   > Hej `wordcountjava-1.0-SNAPSHOT.jar` filen är en uberjar som innehåller inte bara hello WordCount jobb, men även beroenden som hello jobbet kräver vid körning.

## <a id="upload"></a>Överför hello jar

Använd följande kommando tooupload hello jar-filen toohello HDInsight headnode hello:

   ```bash
   scp target/wordcountjava-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:
   ```

    Replace __USERNAME__ with your SSH user name for hello cluster. Replace __CLUSTERNAME__ with hello HDInsight cluster name.

Det här kommandot kopierar hello filer från lokala system hello toohello huvudnod. Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

## <a name="run"></a>Kör hello MapReduce-jobb på Hadoop

1. Ansluta tooHDInsight via SSH. Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

2. Använd följande kommando toorun hello MapReduce programmet hello från hello SSH-sessionen:
   
   ```bash
   yarn jar wordcountjava-1.0-SNAPSHOT.jar org.apache.hadoop.examples.WordCount /example/data/gutenberg/davinci.txt /example/data/wordcountout
   ```
   
    Detta kommando startar hello WordCount MapReduce-programmet. hello indatafilen är `/example/data/gutenberg/davinci.txt`, och hello målkatalogen är `/example/data/wordcountout`. Både hello filen med indata och utdata är lagrade toohello standardlagring för hello-kluster.

3. När hello jobbet är slutfört, använder du följande kommandoresultat tooview hello hello:
   
   ```bash
   hdfs dfs -cat /example/data/wordcountout/*
   ```

    Du bör få en lista över ord och antal med värden liknande toohello följande text:
   
        zeal    1
        zelus   1
        zenith  2

## <a id="nextsteps"></a>Nästa steg

I det här dokumentet har du lärt dig hur toodevelop ett Java-MapReduce-jobb. Se hello efter dokument för andra sätt toowork med HDInsight.

* [Använda Hive med HDInsight][hdinsight-use-hive]
* [Använda Pig med HDInsight][hdinsight-use-pig]
* [Använda MapReduce med HDInsight](hdinsight-use-mapreduce.md)

Mer information finns också hello [Java-Utvecklingscenter](https://azure.microsoft.com/develop/java/).

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

