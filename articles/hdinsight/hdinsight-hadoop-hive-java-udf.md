---
title: "aaaJava användardefinierad funktion (UDF) med Hive i HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur toocreate en Java-baserad användardefinierad funktion (UDF) som fungerar med Hive. Det här exemplet UDF konverterar en tabell med text strängar toolowercase."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 8d4f8efe-2f01-4a61-8619-651e873c7982
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/26/2017
ms.author: larryfr
ms.openlocfilehash: 392b4cfb73299d2f6c1e8e825a4201b48d501388
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-a-java-udf-with-hive-in-hdinsight"></a><span data-ttu-id="b61d5-104">Använda en Java UDF med Hive i HDInsight</span><span class="sxs-lookup"><span data-stu-id="b61d5-104">Use a Java UDF with Hive in HDInsight</span></span>

<span data-ttu-id="b61d5-105">Lär dig hur toocreate en Java-baserad användardefinierad funktion (UDF) som fungerar med Hive.</span><span class="sxs-lookup"><span data-stu-id="b61d5-105">Learn how toocreate a Java-based user-defined function (UDF) that works with Hive.</span></span> <span data-ttu-id="b61d5-106">i det här exemplet Hello Java UDF konverterar en tabell på strängar tooall gemena tecken.</span><span class="sxs-lookup"><span data-stu-id="b61d5-106">hello Java UDF in this example converts a table of text strings tooall-lowercase characters.</span></span>

## <a name="requirements"></a><span data-ttu-id="b61d5-107">Krav</span><span class="sxs-lookup"><span data-stu-id="b61d5-107">Requirements</span></span>

* <span data-ttu-id="b61d5-108">Ett HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="b61d5-108">An HDInsight cluster</span></span> 

    > [!IMPORTANT]
    > <span data-ttu-id="b61d5-109">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="b61d5-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="b61d5-110">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="b61d5-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

    <span data-ttu-id="b61d5-111">De flesta stegen i det här dokumentet fungerar för både Windows - och Linux-baserade kluster.</span><span class="sxs-lookup"><span data-stu-id="b61d5-111">Most steps in this document work on both Windows- and Linux-based clusters.</span></span> <span data-ttu-id="b61d5-112">Dock kompileras hello steg som används för tooupload hello UDF toohello kluster och kör det finns specifika tooLinux-baserade kluster.</span><span class="sxs-lookup"><span data-stu-id="b61d5-112">However, hello steps used tooupload hello compiled UDF toohello cluster and run it are specific tooLinux-based clusters.</span></span> <span data-ttu-id="b61d5-113">Länkar finns tooinformation som kan användas med Windows-baserade kluster.</span><span class="sxs-lookup"><span data-stu-id="b61d5-113">Links are provided tooinformation that can be used with Windows-based clusters.</span></span>

* <span data-ttu-id="b61d5-114">[Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 8 eller senare (eller en motsvarande, till exempel OpenJDK)</span><span class="sxs-lookup"><span data-stu-id="b61d5-114">[Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 8 or later (or an equivalent, such as OpenJDK)</span></span>

* [<span data-ttu-id="b61d5-115">Apache Maven</span><span class="sxs-lookup"><span data-stu-id="b61d5-115">Apache Maven</span></span>](http://maven.apache.org/)

* <span data-ttu-id="b61d5-116">En textredigerare eller IDE för Java</span><span class="sxs-lookup"><span data-stu-id="b61d5-116">A text editor or Java IDE</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="b61d5-117">Om du skapar hello Python-filer på en Windows-klient, måste du använda ett redigeringsprogram som använder LF som en rad avslutades.</span><span class="sxs-lookup"><span data-stu-id="b61d5-117">If you create hello Python files on a Windows client, you must use an editor that uses LF as a line ending.</span></span> <span data-ttu-id="b61d5-118">Om du inte är säker på om redigeringsprogram använder LF eller CRLF finns hello [felsökning](#troubleshooting) avsnittet för instruktioner för att ta bort hello CR-tecken.</span><span class="sxs-lookup"><span data-stu-id="b61d5-118">If you are not sure whether your editor uses LF or CRLF, see hello [Troubleshooting](#troubleshooting) section for steps on removing hello CR character.</span></span>

## <a name="create-an-example-java-udf"></a><span data-ttu-id="b61d5-119">Skapa ett exempel Java UDF</span><span class="sxs-lookup"><span data-stu-id="b61d5-119">Create an example Java UDF</span></span> 

1. <span data-ttu-id="b61d5-120">Använd hello följande toocreate ett nytt Maven-projekt från en kommandorad:</span><span class="sxs-lookup"><span data-stu-id="b61d5-120">From a command line, use hello following toocreate a new Maven project:</span></span>

    ```bash
    mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=ExampleUDF -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

   > [!NOTE]
   > <span data-ttu-id="b61d5-121">Om du använder PowerShell, måste du placera citattecken runt hello parametrar.</span><span class="sxs-lookup"><span data-stu-id="b61d5-121">If you are using PowerShell, you must put quotes around hello parameters.</span></span> <span data-ttu-id="b61d5-122">Till exempel `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`.</span><span class="sxs-lookup"><span data-stu-id="b61d5-122">For example, `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`.</span></span>

    <span data-ttu-id="b61d5-123">Det här kommandot skapar en katalog med namnet **exampleudf**, som innehåller hello Maven-projekt.</span><span class="sxs-lookup"><span data-stu-id="b61d5-123">This command creates a directory named **exampleudf**, which contains hello Maven project.</span></span>

2. <span data-ttu-id="b61d5-124">När hello projektet har skapats kan du ta bort hello **exampleudf/src/test** katalogen som har skapats som en del av hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="b61d5-124">Once hello project has been created, delete hello **exampleudf/src/test** directory that was created as part of hello project.</span></span>

3. <span data-ttu-id="b61d5-125">Öppna hello **exampleudf/pom.xml**, och ersätta befintliga hello `<dependencies>` post med följande XML hello:</span><span class="sxs-lookup"><span data-stu-id="b61d5-125">Open hello **exampleudf/pom.xml**, and replace hello existing `<dependencies>` entry with hello following XML:</span></span>

    ```xml
    <dependencies>
        <dependency>
            <groupId>org.apache.hadoop</groupId>
            <artifactId>hadoop-client</artifactId>
            <version>2.7.3</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.apache.hive</groupId>
            <artifactId>hive-exec</artifactId>
            <version>1.2.1</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>
    ```

    <span data-ttu-id="b61d5-126">De här posterna ange hello version av Hadoop och Hive som ingår i HDInsight 3.5.</span><span class="sxs-lookup"><span data-stu-id="b61d5-126">These entries specify hello version of Hadoop and Hive included with HDInsight 3.5.</span></span> <span data-ttu-id="b61d5-127">Du kan hitta information om hello versioner av Hadoop och Hive som tillhandahålls med HDInsight från hello [HDInsight component-versioning](hdinsight-component-versioning.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="b61d5-127">You can find information on hello versions of Hadoop and Hive provided with HDInsight from hello [HDInsight component versioning](hdinsight-component-versioning.md) document.</span></span>

    <span data-ttu-id="b61d5-128">Lägg till en `<build>` avsnittet före hello `</project>` rad hello slutet av hello-filen.</span><span class="sxs-lookup"><span data-stu-id="b61d5-128">Add a `<build>` section before hello `</project>` line at hello end of hello file.</span></span> <span data-ttu-id="b61d5-129">Det här avsnittet bör innehålla följande XML hello:</span><span class="sxs-lookup"><span data-stu-id="b61d5-129">This section should contain hello following XML:</span></span>

    ```xml
    <build>
        <plugins>
            <!-- build for Java 1.8. This is required by HDInsight 3.5  -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.3</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
            <!-- build an uber jar -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.3</version>
                <configuration>
                    <!-- Keep us from getting a can't overwrite file error -->
                    <transformers>
                        <transformer
                                implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                        </transformer>
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer">
                        </transformer>
                    </transformers>
                    <!-- Keep us from getting a bad signature error -->
                    <filters>
                        <filter>
                            <artifact>*:*</artifact>
                            <excludes>
                                <exclude>META-INF/*.SF</exclude>
                                <exclude>META-INF/*.DSA</exclude>
                                <exclude>META-INF/*.RSA</exclude>
                            </excludes>
                        </filter>
                    </filters>
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
        </plugins>
    </build>
    ```

    <span data-ttu-id="b61d5-130">Dessa poster definierar hur toobuild hello projektet.</span><span class="sxs-lookup"><span data-stu-id="b61d5-130">These entries define how toobuild hello project.</span></span> <span data-ttu-id="b61d5-131">Mer specifikt hello version av Java som hello projektet använder och hur toobuild en uberjar för distribution toohello kluster.</span><span class="sxs-lookup"><span data-stu-id="b61d5-131">Specifically, hello version of Java that hello project uses and how toobuild an uberjar for deployment toohello cluster.</span></span>

    <span data-ttu-id="b61d5-132">Spara hello när hello ändringar har gjorts.</span><span class="sxs-lookup"><span data-stu-id="b61d5-132">Save hello file once hello changes have been made.</span></span>

4. <span data-ttu-id="b61d5-133">Byt namn på **exampleudf/src/main/java/com/microsoft/examples/App.java** för**ExampleUDF.java**, och sedan öppna hello-filen i redigeringsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="b61d5-133">Rename **exampleudf/src/main/java/com/microsoft/examples/App.java** too**ExampleUDF.java**, and then open hello file in your editor.</span></span>

5. <span data-ttu-id="b61d5-134">Ersätt hello innehållet i hello **ExampleUDF.java** med följande hello och spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="b61d5-134">Replace hello contents of hello **ExampleUDF.java** file with hello following, then save hello file.</span></span>

    ```java
    package com.microsoft.examples;

    import org.apache.hadoop.hive.ql.exec.Description;
    import org.apache.hadoop.hive.ql.exec.UDF;
    import org.apache.hadoop.io.*;

    // Description of hello UDF
    @Description(
        name="ExampleUDF",
        value="returns a lower case version of hello input string.",
        extended="select ExampleUDF(deviceplatform) from hivesampletable limit 10;"
    )
    public class ExampleUDF extends UDF {
        // Accept a string input
        public String evaluate(String input) {
            // If hello value is null, return a null
            if(input == null)
                return null;
            // Lowercase hello input string and return it
            return input.toLowerCase();
        }
    }
    ```

    <span data-ttu-id="b61d5-135">Den här koden implementerar en UDF som accepterar ett strängvärde och returnerar en gemen version av hello-sträng.</span><span class="sxs-lookup"><span data-stu-id="b61d5-135">This code implements a UDF that accepts a string value, and returns a lowercase version of hello string.</span></span>

## <a name="build-and-install-hello-udf"></a><span data-ttu-id="b61d5-136">Skapa och installera hello UDF</span><span class="sxs-lookup"><span data-stu-id="b61d5-136">Build and install hello UDF</span></span>

1. <span data-ttu-id="b61d5-137">Använd följande kommando toocompile hello och paketet hello UDF:</span><span class="sxs-lookup"><span data-stu-id="b61d5-137">Use hello following command toocompile and package hello UDF:</span></span>

    ```bash
    mvn compile package
    ```

    <span data-ttu-id="b61d5-138">Det här kommandot skapar och paket hello UDF i hello `exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar` fil.</span><span class="sxs-lookup"><span data-stu-id="b61d5-138">This command builds and packages hello UDF into hello `exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar` file.</span></span>

2. <span data-ttu-id="b61d5-139">Använd hello `scp` kommandot toocopy hello filen toohello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="b61d5-139">Use hello `scp` command toocopy hello file toohello HDInsight cluster.</span></span>

    ```bash
    scp ./target/ExampleUDF-1.0-SNAPSHOT.jar myuser@mycluster-ssh.azurehdinsight
    ```

    <span data-ttu-id="b61d5-140">Ersätt `myuser` med hello SSH-användarkontot för klustret.</span><span class="sxs-lookup"><span data-stu-id="b61d5-140">Replace `myuser` with hello SSH user account for your cluster.</span></span> <span data-ttu-id="b61d5-141">Ersätt `mycluster` med hello klusternamnet.</span><span class="sxs-lookup"><span data-stu-id="b61d5-141">Replace `mycluster` with hello cluster name.</span></span> <span data-ttu-id="b61d5-142">Om du har använt ett lösenord toosecure hello SSH-konto kan du ange tooenter hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="b61d5-142">If you used a password toosecure hello SSH account, you are prompted tooenter hello password.</span></span> <span data-ttu-id="b61d5-143">Om du använder ett certifikat, måste du kanske toouse hello `-i` parametern toospecify hello fil för privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="b61d5-143">If you used a certificate, you may need toouse hello `-i` parameter toospecify hello private key file.</span></span>

3. <span data-ttu-id="b61d5-144">Ansluta toohello kluster med SSH.</span><span class="sxs-lookup"><span data-stu-id="b61d5-144">Connect toohello cluster using SSH.</span></span>

    ```bash
    ssh myuser@mycluster-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="b61d5-145">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="b61d5-145">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

4. <span data-ttu-id="b61d5-146">Kopiera hello jar fillagring tooHDInsight från hello SSH-sessionen.</span><span class="sxs-lookup"><span data-stu-id="b61d5-146">From hello SSH session, copy hello jar file tooHDInsight storage.</span></span>

    ```bash
    hdfs dfs -put ExampleUDF-1.0-SNAPSHOT.jar /example/jars
    ```

## <a name="use-hello-udf-from-hive"></a><span data-ttu-id="b61d5-147">Använda hello UDF från Hive</span><span class="sxs-lookup"><span data-stu-id="b61d5-147">Use hello UDF from Hive</span></span>

1. <span data-ttu-id="b61d5-148">Använd hello följande toostart hello Beeline klienten från hello SSH-session.</span><span class="sxs-lookup"><span data-stu-id="b61d5-148">Use hello following toostart hello Beeline client from hello SSH session.</span></span>

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin
    ```

    <span data-ttu-id="b61d5-149">Det här kommandot förutsätter att du använde hello standardvärdet **admin** för hello inloggningskonto för klustret.</span><span class="sxs-lookup"><span data-stu-id="b61d5-149">This command assumes that you used hello default of **admin** for hello login account for your cluster.</span></span>

2. <span data-ttu-id="b61d5-150">När du kommer till hello `jdbc:hive2://localhost:10001/>` fråga, ange hello följande tooadd hello UDF tooHive och exponera som en funktion.</span><span class="sxs-lookup"><span data-stu-id="b61d5-150">Once you arrive at hello `jdbc:hive2://localhost:10001/>` prompt, enter hello following tooadd hello UDF tooHive and expose it as a function.</span></span>

    ```hiveql
    ADD JAR wasb:///example/jars/ExampleUDF-1.0-SNAPSHOT.jar;
    CREATE TEMPORARY FUNCTION tolower as 'com.microsoft.examples.ExampleUDF';
    ```

    > [!NOTE]
    > <span data-ttu-id="b61d5-151">Det här exemplet förutsätter att lagringen Azure standardlagring för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="b61d5-151">This example assumes that Azure Storage is default storage for hello cluster.</span></span> <span data-ttu-id="b61d5-152">Om klustret använder Data Lake Store i stället ändrar hello `wasb:///` värdet för`adl:///`.</span><span class="sxs-lookup"><span data-stu-id="b61d5-152">If your cluster uses Data Lake Store instead, change hello `wasb:///` value too`adl:///`.</span></span>

3. <span data-ttu-id="b61d5-153">Använd hello UDF tooconvert värdena som hämtas från en tabell toolower case strängar.</span><span class="sxs-lookup"><span data-stu-id="b61d5-153">Use hello UDF tooconvert values retrieved from a table toolower case strings.</span></span>

    ```hiveql
    SELECT tolower(deviceplatform) FROM hivesampletable LIMIT 10;
    ```

    <span data-ttu-id="b61d5-154">Den här frågan väljer hello plattform för enheter (Android, Windows, iOS, etc.) från hello tabell, konvertera hello sträng toolower fallet och visa dem sedan.</span><span class="sxs-lookup"><span data-stu-id="b61d5-154">This query selects hello device platform (Android, Windows, iOS, etc.) from hello table, convert hello string toolower case, and then display them.</span></span> <span data-ttu-id="b61d5-155">hello utdata visas liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="b61d5-155">hello output appears similar toohello following text:</span></span>

        +----------+--+
        |   _c0    |
        +----------+--+
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        +----------+--+

## <a name="next-steps"></a><span data-ttu-id="b61d5-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b61d5-156">Next steps</span></span>

<span data-ttu-id="b61d5-157">Andra sätt toowork med Hive finns [använda Hive med HDInsight](hdinsight-use-hive.md).</span><span class="sxs-lookup"><span data-stu-id="b61d5-157">For other ways toowork with Hive, see [Use Hive with HDInsight](hdinsight-use-hive.md).</span></span>

<span data-ttu-id="b61d5-158">Mer information om Hive User-Defined funktioner finns [Hive operatorer och användardefinierade funktioner](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) avsnitt i hello Hive wiki på apache.org.</span><span class="sxs-lookup"><span data-stu-id="b61d5-158">For more information on Hive User-Defined Functions, see [Hive Operators and User-Defined Functions](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) section of hello Hive wiki at apache.org.</span></span>
