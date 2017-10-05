---
title: "Java användardefinierad funktion (UDF) med Hive i HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur du skapar en Java-baserad användardefinierad funktion (UDF) som fungerar med Hive. Det här exemplet UDF konverterar en tabell med textsträngar till gemener."
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
ms.openlocfilehash: 481d234eaf88bdb210821084ee4154159470eda0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="use-a-java-udf-with-hive-in-hdinsight"></a><span data-ttu-id="f5eb5-104">Använda en Java UDF med Hive i HDInsight</span><span class="sxs-lookup"><span data-stu-id="f5eb5-104">Use a Java UDF with Hive in HDInsight</span></span>

<span data-ttu-id="f5eb5-105">Lär dig hur du skapar en Java-baserad användardefinierad funktion (UDF) som fungerar med Hive.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-105">Learn how to create a Java-based user-defined function (UDF) that works with Hive.</span></span> <span data-ttu-id="f5eb5-106">Java-UDF i det här exemplet konverterar en tabell med textsträngar till alla gemena tecken.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-106">The Java UDF in this example converts a table of text strings to all-lowercase characters.</span></span>

## <a name="requirements"></a><span data-ttu-id="f5eb5-107">Krav</span><span class="sxs-lookup"><span data-stu-id="f5eb5-107">Requirements</span></span>

* <span data-ttu-id="f5eb5-108">Ett HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="f5eb5-108">An HDInsight cluster</span></span> 

    > [!IMPORTANT]
    > <span data-ttu-id="f5eb5-109">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-109">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="f5eb5-110">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="f5eb5-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

    <span data-ttu-id="f5eb5-111">De flesta stegen i det här dokumentet fungerar för både Windows - och Linux-baserade kluster.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-111">Most steps in this document work on both Windows- and Linux-based clusters.</span></span> <span data-ttu-id="f5eb5-112">Dock är de steg som används för att överföra den kompilerade UDF till klustret och köra den specifika för Linux-baserade kluster.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-112">However, the steps used to upload the compiled UDF to the cluster and run it are specific to Linux-based clusters.</span></span> <span data-ttu-id="f5eb5-113">Länkar finns information som kan användas med Windows-baserade kluster.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-113">Links are provided to information that can be used with Windows-based clusters.</span></span>

* <span data-ttu-id="f5eb5-114">[Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 8 eller senare (eller en motsvarande, till exempel OpenJDK)</span><span class="sxs-lookup"><span data-stu-id="f5eb5-114">[Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 8 or later (or an equivalent, such as OpenJDK)</span></span>

* [<span data-ttu-id="f5eb5-115">Apache Maven</span><span class="sxs-lookup"><span data-stu-id="f5eb5-115">Apache Maven</span></span>](http://maven.apache.org/)

* <span data-ttu-id="f5eb5-116">En textredigerare eller IDE för Java</span><span class="sxs-lookup"><span data-stu-id="f5eb5-116">A text editor or Java IDE</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="f5eb5-117">Om du skapar Python-filer på en Windows-klient, måste du använda ett redigeringsprogram som använder LF som en rad avslutades.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-117">If you create the Python files on a Windows client, you must use an editor that uses LF as a line ending.</span></span> <span data-ttu-id="f5eb5-118">Om du inte är säker på om redigeringsprogram använder LF eller CRLF finns i [felsökning](#troubleshooting) avsnittet stegvisa instruktioner för att ta bort CR-tecken.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-118">If you are not sure whether your editor uses LF or CRLF, see the [Troubleshooting](#troubleshooting) section for steps on removing the CR character.</span></span>

## <a name="create-an-example-java-udf"></a><span data-ttu-id="f5eb5-119">Skapa ett exempel Java UDF</span><span class="sxs-lookup"><span data-stu-id="f5eb5-119">Create an example Java UDF</span></span> 

1. <span data-ttu-id="f5eb5-120">Använd följande för att skapa ett nytt Maven-projekt från en kommandorad:</span><span class="sxs-lookup"><span data-stu-id="f5eb5-120">From a command line, use the following to create a new Maven project:</span></span>

    ```bash
    mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=ExampleUDF -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

   > [!NOTE]
   > <span data-ttu-id="f5eb5-121">Om du använder PowerShell, måste du placera citattecken runt parametrarna.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-121">If you are using PowerShell, you must put quotes around the parameters.</span></span> <span data-ttu-id="f5eb5-122">Till exempel `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-122">For example, `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`.</span></span>

    <span data-ttu-id="f5eb5-123">Det här kommandot skapar en katalog med namnet **exampleudf**, som innehåller Maven-projekt.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-123">This command creates a directory named **exampleudf**, which contains the Maven project.</span></span>

2. <span data-ttu-id="f5eb5-124">När projektet har skapats kan du ta bort den **exampleudf/src/test** katalogen som har skapats som en del av projektet.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-124">Once the project has been created, delete the **exampleudf/src/test** directory that was created as part of the project.</span></span>

3. <span data-ttu-id="f5eb5-125">Öppna den **exampleudf/pom.xml**, och Ersätt den befintliga `<dependencies>` post med följande XML-filen:</span><span class="sxs-lookup"><span data-stu-id="f5eb5-125">Open the **exampleudf/pom.xml**, and replace the existing `<dependencies>` entry with the following XML:</span></span>

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

    <span data-ttu-id="f5eb5-126">De här posterna ange version för Hadoop och Hive som ingår i HDInsight 3.5.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-126">These entries specify the version of Hadoop and Hive included with HDInsight 3.5.</span></span> <span data-ttu-id="f5eb5-127">Du kan hitta information om versioner av Hadoop och Hive med HDInsight från den [HDInsight component-versioning](hdinsight-component-versioning.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-127">You can find information on the versions of Hadoop and Hive provided with HDInsight from the [HDInsight component versioning](hdinsight-component-versioning.md) document.</span></span>

    <span data-ttu-id="f5eb5-128">Lägg till en `<build>` avsnittet före den `</project>` rad i slutet av filen.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-128">Add a `<build>` section before the `</project>` line at the end of the file.</span></span> <span data-ttu-id="f5eb5-129">Det här avsnittet bör innehålla följande XML-filen:</span><span class="sxs-lookup"><span data-stu-id="f5eb5-129">This section should contain the following XML:</span></span>

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

    <span data-ttu-id="f5eb5-130">Dessa poster definierar hur att skapa projektet.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-130">These entries define how to build the project.</span></span> <span data-ttu-id="f5eb5-131">Mer specifikt versionen av Java som projektet använder och hur du skapar en uberjar för distribution till klustret.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-131">Specifically, the version of Java that the project uses and how to build an uberjar for deployment to the cluster.</span></span>

    <span data-ttu-id="f5eb5-132">Spara filen när de har ändrats.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-132">Save the file once the changes have been made.</span></span>

4. <span data-ttu-id="f5eb5-133">Byt namn på **exampleudf/src/main/java/com/microsoft/examples/App.java** till **ExampleUDF.java**, och sedan öppna filen i redigeringsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-133">Rename **exampleudf/src/main/java/com/microsoft/examples/App.java** to **ExampleUDF.java**, and then open the file in your editor.</span></span>

5. <span data-ttu-id="f5eb5-134">Ersätt innehållet i den **ExampleUDF.java** med följande och spara filen.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-134">Replace the contents of the **ExampleUDF.java** file with the following, then save the file.</span></span>

    ```java
    package com.microsoft.examples;

    import org.apache.hadoop.hive.ql.exec.Description;
    import org.apache.hadoop.hive.ql.exec.UDF;
    import org.apache.hadoop.io.*;

    // Description of the UDF
    @Description(
        name="ExampleUDF",
        value="returns a lower case version of the input string.",
        extended="select ExampleUDF(deviceplatform) from hivesampletable limit 10;"
    )
    public class ExampleUDF extends UDF {
        // Accept a string input
        public String evaluate(String input) {
            // If the value is null, return a null
            if(input == null)
                return null;
            // Lowercase the input string and return it
            return input.toLowerCase();
        }
    }
    ```

    <span data-ttu-id="f5eb5-135">Den här koden implementerar en UDF som accepterar ett strängvärde och returnerar en gemen version av strängen.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-135">This code implements a UDF that accepts a string value, and returns a lowercase version of the string.</span></span>

## <a name="build-and-install-the-udf"></a><span data-ttu-id="f5eb5-136">Skapa och installera en användardefinierad funktion</span><span class="sxs-lookup"><span data-stu-id="f5eb5-136">Build and install the UDF</span></span>

1. <span data-ttu-id="f5eb5-137">Använd följande kommando för att kompilera och paketera UDF-filen:</span><span class="sxs-lookup"><span data-stu-id="f5eb5-137">Use the following command to compile and package the UDF:</span></span>

    ```bash
    mvn compile package
    ```

    <span data-ttu-id="f5eb5-138">Det här kommandot skapar och paketerar UDF till den `exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar` filen.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-138">This command builds and packages the UDF into the `exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar` file.</span></span>

2. <span data-ttu-id="f5eb5-139">Använd den `scp` kommando för att kopiera filen till HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-139">Use the `scp` command to copy the file to the HDInsight cluster.</span></span>

    ```bash
    scp ./target/ExampleUDF-1.0-SNAPSHOT.jar myuser@mycluster-ssh.azurehdinsight
    ```

    <span data-ttu-id="f5eb5-140">Ersätt `myuser` med SSH-användarkonto för klustret.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-140">Replace `myuser` with the SSH user account for your cluster.</span></span> <span data-ttu-id="f5eb5-141">Ersätt `mycluster` med klustrets namn.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-141">Replace `mycluster` with the cluster name.</span></span> <span data-ttu-id="f5eb5-142">Om du har använt ett lösenord för att skydda det SSH-kontot uppmanas du att ange lösenordet.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-142">If you used a password to secure the SSH account, you are prompted to enter the password.</span></span> <span data-ttu-id="f5eb5-143">Om du använder ett certifikat, kan du behöva använda de `-i` parametern för att ange fil för privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-143">If you used a certificate, you may need to use the `-i` parameter to specify the private key file.</span></span>

3. <span data-ttu-id="f5eb5-144">Ansluta till klustret med SSH.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-144">Connect to the cluster using SSH.</span></span>

    ```bash
    ssh myuser@mycluster-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="f5eb5-145">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="f5eb5-145">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

4. <span data-ttu-id="f5eb5-146">Kopiera jar-filen i HDInsight-lagringen från SSH-session.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-146">From the SSH session, copy the jar file to HDInsight storage.</span></span>

    ```bash
    hdfs dfs -put ExampleUDF-1.0-SNAPSHOT.jar /example/jars
    ```

## <a name="use-the-udf-from-hive"></a><span data-ttu-id="f5eb5-147">Använda en användardefinierad funktion från Hive</span><span class="sxs-lookup"><span data-stu-id="f5eb5-147">Use the UDF from Hive</span></span>

1. <span data-ttu-id="f5eb5-148">Använd följande för att starta klienten Beeline från SSH-session.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-148">Use the following to start the Beeline client from the SSH session.</span></span>

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin
    ```

    <span data-ttu-id="f5eb5-149">Det här kommandot förutsätter att du använde standardvärdet **admin** för inloggningskontot för klustret.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-149">This command assumes that you used the default of **admin** for the login account for your cluster.</span></span>

2. <span data-ttu-id="f5eb5-150">När du kommer till den `jdbc:hive2://localhost:10001/>` uppmanar, ange följande om du vill lägga till en användardefinierad funktion Hive och exponera som en funktion.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-150">Once you arrive at the `jdbc:hive2://localhost:10001/>` prompt, enter the following to add the UDF to Hive and expose it as a function.</span></span>

    ```hiveql
    ADD JAR wasb:///example/jars/ExampleUDF-1.0-SNAPSHOT.jar;
    CREATE TEMPORARY FUNCTION tolower as 'com.microsoft.examples.ExampleUDF';
    ```

    > [!NOTE]
    > <span data-ttu-id="f5eb5-151">Det här exemplet förutsätter att lagringen Azure standardlagring för klustret.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-151">This example assumes that Azure Storage is default storage for the cluster.</span></span> <span data-ttu-id="f5eb5-152">Om klustret använder Data Lake Store i stället ändrar den `wasb:///` värde till `adl:///`.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-152">If your cluster uses Data Lake Store instead, change the `wasb:///` value to `adl:///`.</span></span>

3. <span data-ttu-id="f5eb5-153">Använda en användardefinierad funktion för att konvertera värden som hämtats från en tabell till gemen strängar.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-153">Use the UDF to convert values retrieved from a table to lower case strings.</span></span>

    ```hiveql
    SELECT tolower(deviceplatform) FROM hivesampletable LIMIT 10;
    ```

    <span data-ttu-id="f5eb5-154">Den här frågan väljer plattform för enheter (Android, Windows, iOS, etc.) från tabellen, konvertera strängen till lägre fall och visa dem sedan.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-154">This query selects the device platform (Android, Windows, iOS, etc.) from the table, convert the string to lower case, and then display them.</span></span> <span data-ttu-id="f5eb5-155">Utdata liknar följande:</span><span class="sxs-lookup"><span data-stu-id="f5eb5-155">The output appears similar to the following text:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="f5eb5-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f5eb5-156">Next steps</span></span>

<span data-ttu-id="f5eb5-157">Andra sätt att arbeta med Hive finns [använda Hive med HDInsight](hdinsight-use-hive.md).</span><span class="sxs-lookup"><span data-stu-id="f5eb5-157">For other ways to work with Hive, see [Use Hive with HDInsight](hdinsight-use-hive.md).</span></span>

<span data-ttu-id="f5eb5-158">Mer information om Hive User-Defined funktioner finns [Hive operatorer och användardefinierade funktioner](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) avsnitt i Hive-wiki på apache.org.</span><span class="sxs-lookup"><span data-stu-id="f5eb5-158">For more information on Hive User-Defined Functions, see [Hive Operators and User-Defined Functions](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) section of the Hive wiki at apache.org.</span></span>
