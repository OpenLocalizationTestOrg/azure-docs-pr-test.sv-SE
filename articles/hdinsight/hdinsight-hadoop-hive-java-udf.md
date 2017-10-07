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
# <a name="use-a-java-udf-with-hive-in-hdinsight"></a>Använda en Java UDF med Hive i HDInsight

Lär dig hur toocreate en Java-baserad användardefinierad funktion (UDF) som fungerar med Hive. i det här exemplet Hello Java UDF konverterar en tabell på strängar tooall gemena tecken.

## <a name="requirements"></a>Krav

* Ett HDInsight-kluster 

    > [!IMPORTANT]
    > Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

    De flesta stegen i det här dokumentet fungerar för både Windows - och Linux-baserade kluster. Dock kompileras hello steg som används för tooupload hello UDF toohello kluster och kör det finns specifika tooLinux-baserade kluster. Länkar finns tooinformation som kan användas med Windows-baserade kluster.

* [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 8 eller senare (eller en motsvarande, till exempel OpenJDK)

* [Apache Maven](http://maven.apache.org/)

* En textredigerare eller IDE för Java

    > [!IMPORTANT]
    > Om du skapar hello Python-filer på en Windows-klient, måste du använda ett redigeringsprogram som använder LF som en rad avslutades. Om du inte är säker på om redigeringsprogram använder LF eller CRLF finns hello [felsökning](#troubleshooting) avsnittet för instruktioner för att ta bort hello CR-tecken.

## <a name="create-an-example-java-udf"></a>Skapa ett exempel Java UDF 

1. Använd hello följande toocreate ett nytt Maven-projekt från en kommandorad:

    ```bash
    mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=ExampleUDF -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

   > [!NOTE]
   > Om du använder PowerShell, måste du placera citattecken runt hello parametrar. Till exempel `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`.

    Det här kommandot skapar en katalog med namnet **exampleudf**, som innehåller hello Maven-projekt.

2. När hello projektet har skapats kan du ta bort hello **exampleudf/src/test** katalogen som har skapats som en del av hello-projekt.

3. Öppna hello **exampleudf/pom.xml**, och ersätta befintliga hello `<dependencies>` post med följande XML hello:

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

    De här posterna ange hello version av Hadoop och Hive som ingår i HDInsight 3.5. Du kan hitta information om hello versioner av Hadoop och Hive som tillhandahålls med HDInsight från hello [HDInsight component-versioning](hdinsight-component-versioning.md) dokumentet.

    Lägg till en `<build>` avsnittet före hello `</project>` rad hello slutet av hello-filen. Det här avsnittet bör innehålla följande XML hello:

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

    Dessa poster definierar hur toobuild hello projektet. Mer specifikt hello version av Java som hello projektet använder och hur toobuild en uberjar för distribution toohello kluster.

    Spara hello när hello ändringar har gjorts.

4. Byt namn på **exampleudf/src/main/java/com/microsoft/examples/App.java** för**ExampleUDF.java**, och sedan öppna hello-filen i redigeringsprogrammet.

5. Ersätt hello innehållet i hello **ExampleUDF.java** med följande hello och spara hello-filen.

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

    Den här koden implementerar en UDF som accepterar ett strängvärde och returnerar en gemen version av hello-sträng.

## <a name="build-and-install-hello-udf"></a>Skapa och installera hello UDF

1. Använd följande kommando toocompile hello och paketet hello UDF:

    ```bash
    mvn compile package
    ```

    Det här kommandot skapar och paket hello UDF i hello `exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar` fil.

2. Använd hello `scp` kommandot toocopy hello filen toohello HDInsight-kluster.

    ```bash
    scp ./target/ExampleUDF-1.0-SNAPSHOT.jar myuser@mycluster-ssh.azurehdinsight
    ```

    Ersätt `myuser` med hello SSH-användarkontot för klustret. Ersätt `mycluster` med hello klusternamnet. Om du har använt ett lösenord toosecure hello SSH-konto kan du ange tooenter hello lösenord. Om du använder ett certifikat, måste du kanske toouse hello `-i` parametern toospecify hello fil för privat nyckel.

3. Ansluta toohello kluster med SSH.

    ```bash
    ssh myuser@mycluster-ssh.azurehdinsight.net
    ```

    Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

4. Kopiera hello jar fillagring tooHDInsight från hello SSH-sessionen.

    ```bash
    hdfs dfs -put ExampleUDF-1.0-SNAPSHOT.jar /example/jars
    ```

## <a name="use-hello-udf-from-hive"></a>Använda hello UDF från Hive

1. Använd hello följande toostart hello Beeline klienten från hello SSH-session.

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin
    ```

    Det här kommandot förutsätter att du använde hello standardvärdet **admin** för hello inloggningskonto för klustret.

2. När du kommer till hello `jdbc:hive2://localhost:10001/>` fråga, ange hello följande tooadd hello UDF tooHive och exponera som en funktion.

    ```hiveql
    ADD JAR wasb:///example/jars/ExampleUDF-1.0-SNAPSHOT.jar;
    CREATE TEMPORARY FUNCTION tolower as 'com.microsoft.examples.ExampleUDF';
    ```

    > [!NOTE]
    > Det här exemplet förutsätter att lagringen Azure standardlagring för hello-kluster. Om klustret använder Data Lake Store i stället ändrar hello `wasb:///` värdet för`adl:///`.

3. Använd hello UDF tooconvert värdena som hämtas från en tabell toolower case strängar.

    ```hiveql
    SELECT tolower(deviceplatform) FROM hivesampletable LIMIT 10;
    ```

    Den här frågan väljer hello plattform för enheter (Android, Windows, iOS, etc.) från hello tabell, konvertera hello sträng toolower fallet och visa dem sedan. hello utdata visas liknande toohello följande text:

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

## <a name="next-steps"></a>Nästa steg

Andra sätt toowork med Hive finns [använda Hive med HDInsight](hdinsight-use-hive.md).

Mer information om Hive User-Defined funktioner finns [Hive operatorer och användardefinierade funktioner](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) avsnitt i hello Hive wiki på apache.org.
