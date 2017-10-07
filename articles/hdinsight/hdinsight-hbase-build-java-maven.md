---
title: "aaaBuild ett HBase Java-program för Windows-baserade Azure HDInsight | Microsoft Docs"
description: "Lär dig hur toouse Apache Maven toobuild en Java-baserad Apache HBase programmet sedan distribuera den tooa Windows-baserade Azure HDInsight-kluster."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7f4a4e02-45ab-40dd-842b-3ec034f256c9
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 33c2f3d12cb6a17b5406817e8bcd3accff239517
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-maven-toobuild-java-applications-that-use-hbase-with-windows-based-hdinsight-hadoop"></a><span data-ttu-id="e1a2b-103">Använd Maven toobuild Java-program som använder HBase med Windows-baserade HDInsight (Hadoop)</span><span class="sxs-lookup"><span data-stu-id="e1a2b-103">Use Maven toobuild Java applications that use HBase with Windows-based HDInsight (Hadoop)</span></span>
<span data-ttu-id="e1a2b-104">Lär dig hur toocreate och skapa en [Apache HBase](http://hbase.apache.org/) program i Java med hjälp av Apache Maven.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-104">Learn how toocreate and build an [Apache HBase](http://hbase.apache.org/) application in Java by using Apache Maven.</span></span> <span data-ttu-id="e1a2b-105">Använd sedan hello program med Azure HDInsight (Hadoop).</span><span class="sxs-lookup"><span data-stu-id="e1a2b-105">Then use hello application with Azure HDInsight (Hadoop).</span></span>

<span data-ttu-id="e1a2b-106">[Maven](http://maven.apache.org/) är ett projekt hantering och förståelse programverktyg som gör att du toobuild programvara, dokumentation och rapporter för Java-projekt.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-106">[Maven](http://maven.apache.org/) is a software project management and comprehension tool that allows you toobuild software, documentation, and reports for Java projects.</span></span> <span data-ttu-id="e1a2b-107">I den här artikeln får du lära dig hur toouse den toocreate en grundläggande Java-program som som skapar, frågor och tar bort en HBase tabell i ett Azure HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-107">In this article, you learn how toouse it toocreate a basic Java application that that creates, queries, and deletes an HBase table on an Azure HDInsight cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e1a2b-108">hello stegen i det här dokumentet kräver ett HDInsight-kluster som använder Windows.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-108">hello steps in this document require an HDInsight cluster that uses Windows.</span></span> <span data-ttu-id="e1a2b-109">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="e1a2b-110">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="e1a2b-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="requirements"></a><span data-ttu-id="e1a2b-111">Krav</span><span class="sxs-lookup"><span data-stu-id="e1a2b-111">Requirements</span></span>
* <span data-ttu-id="e1a2b-112">[Java platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 eller senare</span><span class="sxs-lookup"><span data-stu-id="e1a2b-112">[Java platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 or later</span></span>
* [<span data-ttu-id="e1a2b-113">Maven 3.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-113">Maven</span></span>](http://maven.apache.org/)
* <span data-ttu-id="e1a2b-114">Ett Windows-baserade HDInsight-kluster med HBase</span><span class="sxs-lookup"><span data-stu-id="e1a2b-114">A Windows-based HDInsight cluster with HBase</span></span>

    > [!NOTE]
    > <span data-ttu-id="e1a2b-115">hello stegen i det här dokumentet har testats med HDInsight-kluster version 3.2 och 3.3.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-115">hello steps in this document have been tested with HDInsight cluster versions 3.2 and 3.3.</span></span> <span data-ttu-id="e1a2b-116">hello standardvärden som i exemplen är för ett 3.3 HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-116">hello default values provided in examples are for a HDInsight 3.3 cluster.</span></span>

## <a name="create-hello-project"></a><span data-ttu-id="e1a2b-117">Skapa hello-projekt</span><span class="sxs-lookup"><span data-stu-id="e1a2b-117">Create hello project</span></span>
1. <span data-ttu-id="e1a2b-118">Ändra kataloger toohello plats där du vill toocreate hello projekt, till exempel från kommandoraden i din utvecklingsmiljö hello `cd code\hdinsight`.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-118">From hello command line in your development environment, change directories toohello location where you want toocreate hello project, for example, `cd code\hdinsight`.</span></span>
2. <span data-ttu-id="e1a2b-119">Använd hello **mvn** kommandot, som installeras med Maven toogenerate hello scaffold-teknik för hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-119">Use hello **mvn** command, which is installed with Maven, toogenerate hello scaffolding for hello project.</span></span>

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    <span data-ttu-id="e1a2b-120">Det här kommandot skapar en katalog på hello aktuella plats, med hello namn anges av hello **artefakt-ID** parameter (**hbaseapp** i det här exemplet.) Den här katalogen innehåller hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="e1a2b-120">This command creates a directory in hello current location, with hello name specified by hello **artifactID** parameter (**hbaseapp** in this example.) This directory contains hello following items:</span></span>

   * <span data-ttu-id="e1a2b-121">**pom.XML**: hello projektet Object Model ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) innehåller information och konfiguration information används toobuild hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-121">**pom.xml**:  hello Project Object Model ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) contains information and configuration details used toobuild hello project.</span></span>
   * <span data-ttu-id="e1a2b-122">**SRC**: hello-katalog som innehåller hello **main\java\com\microsoft\examples** directory, där du ska skapa hello program.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-122">**src**: hello directory that contains hello **main\java\com\microsoft\examples** directory, where you will author hello application.</span></span>
3. <span data-ttu-id="e1a2b-123">Ta bort hello **src\test\java\com\microsoft\examples\apptest.java** filen eftersom den inte används i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-123">Delete hello **src\test\java\com\microsoft\examples\apptest.java** file because it is not used in this example.</span></span>

## <a name="update-hello-project-object-model"></a><span data-ttu-id="e1a2b-124">Uppdatera hello projekt-objektmodell</span><span class="sxs-lookup"><span data-stu-id="e1a2b-124">Update hello Project Object Model</span></span>
1. <span data-ttu-id="e1a2b-125">Redigera hello **pom.xml** och Lägg till följande kod i hello hello `<dependencies>` avsnitt:</span><span class="sxs-lookup"><span data-stu-id="e1a2b-125">Edit hello **pom.xml** file and add hello following code inside hello `<dependencies>` section:</span></span>

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>

    <span data-ttu-id="e1a2b-126">Det här avsnittet visar Maven som hello projektet kräver **hbase-klient** version **1.1.2**.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-126">This section tells Maven that hello project requires **hbase-client** version **1.1.2**.</span></span> <span data-ttu-id="e1a2b-127">Vid kompileringen hämtas detta beroende från hello standarddatabas Maven.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-127">At compile time, this dependency is downloaded from hello default Maven repository.</span></span> <span data-ttu-id="e1a2b-128">Du kan använda hello [Maven centrala lagringsplatsen Sök](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) toolearn mer om detta beroende.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-128">You can use hello [Maven Central Repository Search](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) toolearn more about this dependency.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="e1a2b-129">hello versionsnumret måste matcha hello version av HBase som ingår i ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-129">hello version number must match hello version of HBase that is provided with your HDInsight cluster.</span></span> <span data-ttu-id="e1a2b-130">Använd följande tabell toofind hello rätt versionsnummer hello.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-130">Use hello following table toofind hello correct version number.</span></span>
   >
   >

   | <span data-ttu-id="e1a2b-131">HDInsight-kluster av version</span><span class="sxs-lookup"><span data-stu-id="e1a2b-131">HDInsight cluster version</span></span> | <span data-ttu-id="e1a2b-132">HBase version toouse</span><span class="sxs-lookup"><span data-stu-id="e1a2b-132">HBase version toouse</span></span> |
   | --- | --- |
   | <span data-ttu-id="e1a2b-133">3.2</span><span class="sxs-lookup"><span data-stu-id="e1a2b-133">3.2</span></span> |<span data-ttu-id="e1a2b-134">0.98.4-hadoop2</span><span class="sxs-lookup"><span data-stu-id="e1a2b-134">0.98.4-hadoop2</span></span> |
   | <span data-ttu-id="e1a2b-135">3.3</span><span class="sxs-lookup"><span data-stu-id="e1a2b-135">3.3</span></span> |<span data-ttu-id="e1a2b-136">1.1.2</span><span class="sxs-lookup"><span data-stu-id="e1a2b-136">1.1.2</span></span> |

    <span data-ttu-id="e1a2b-137">Mer information om HDInsight-versioner och komponenter finns [vad är hello olika Hadoop-komponenter tillgänglig med HDInsight](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="e1a2b-137">For more information on HDInsight versions and components, see [What are hello different Hadoop components available with HDInsight](hdinsight-component-versioning.md).</span></span>
2. <span data-ttu-id="e1a2b-138">Om du använder ett 3.3 HDInsight-kluster måste du också lägga till följande toohello hello `<dependencies>` avsnitt:</span><span class="sxs-lookup"><span data-stu-id="e1a2b-138">If you are using an HDInsight 3.3 cluster, you must also add hello following toohello `<dependencies>` section:</span></span>

        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>

    <span data-ttu-id="e1a2b-139">Detta beroende läses hello phoenix-grundläggande komponenter som används av Hbase version 1.1.x.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-139">This dependency will load hello phoenix-core components, which are used by Hbase version 1.1.x.</span></span>
3. <span data-ttu-id="e1a2b-140">Lägg till följande kod toohello hello **pom.xml** fil.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-140">Add hello following code toohello **pom.xml** file.</span></span> <span data-ttu-id="e1a2b-141">Det här avsnittet måste finnas i hello `<project>...</project>` taggar i hello-fil, till exempel mellan `</dependencies>` och `</project>`.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-141">This section must be inside hello `<project>...</project>` tags in hello file, for example, between `</dependencies>` and `</project>`.</span></span>

        <build>
          <sourceDirectory>src</sourceDirectory>
          <resources>
              <resource>
                <directory>${basedir}/conf</directory>
                <filtering>false</filtering>
                <includes>
                  <include>hbase-site.xml</include>
                </includes>
              </resource>
            </resources>
          <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.3</version>
                <configuration>
                    <source>1.7</source>
                    <target>1.7</target>
                </configuration>
              </plugin>
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
          </plugins>
        </build>

    <span data-ttu-id="e1a2b-142">Hej `<resources>` avsnittet konfigureras en resurs (**conf\hbase site.xml**) som innehåller konfigurationsinformation för HBase.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-142">hello `<resources>` section configures a resource (**conf\hbase-site.xml**) that contains configuration information for HBase.</span></span>

   > [!NOTE]
   > <span data-ttu-id="e1a2b-143">Du kan också ange konfigurationsvärden via kod.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-143">You can also set configuration values via code.</span></span> <span data-ttu-id="e1a2b-144">Hello kommentarer i hello **CreateTable** exemplet som följer hur toodo detta.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-144">See hello comments in hello **CreateTable** example that follows for how toodo this.</span></span>
   >
   >

    <span data-ttu-id="e1a2b-145">Detta `<plugins>` avsnittet konfigureras hello [Maven-kompilatorn Plugin](http://maven.apache.org/plugins/maven-compiler-plugin/) och [Maven skugga Plugin](http://maven.apache.org/plugins/maven-shade-plugin/).</span><span class="sxs-lookup"><span data-stu-id="e1a2b-145">This `<plugins>` section configures hello [Maven Compiler Plugin](http://maven.apache.org/plugins/maven-compiler-plugin/) and [Maven Shade Plugin](http://maven.apache.org/plugins/maven-shade-plugin/).</span></span> <span data-ttu-id="e1a2b-146">hello kompilatorn plugin-programmet är används toocompile hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-146">hello compiler plug-in is used toocompile hello topology.</span></span> <span data-ttu-id="e1a2b-147">hello skugga plugin-programmet är dubbletter av används tooprevent licens hello JAR-paketet som skapas genom Maven.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-147">hello shade plug-in is used tooprevent license duplication in hello JAR package that is built by Maven.</span></span> <span data-ttu-id="e1a2b-148">hello beror används på att hello dubbla licensfiler orsakar ett fel vid körning på hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-148">hello reason this is used is that hello duplicate license files cause an error at run time on hello HDInsight cluster.</span></span> <span data-ttu-id="e1a2b-149">Med hjälp av maven-skugga-plugin-program med hello `ApacheLicenseResourceTransformer` implementering förhindrar det här felet.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-149">Using maven-shade-plugin with hello `ApacheLicenseResourceTransformer` implementation prevents this error.</span></span>

    <span data-ttu-id="e1a2b-150">Hej maven-skugga-plugin-programmet ger också en uber jar (eller fat jar) som innehåller alla hello beroenden som krävs av hello program.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-150">hello maven-shade-plugin also produces an uber jar (or fat jar) that contains all hello dependencies required by hello application.</span></span>
4. <span data-ttu-id="e1a2b-151">Spara hello **pom.xml** fil.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-151">Save hello **pom.xml** file.</span></span>
5. <span data-ttu-id="e1a2b-152">Skapa en ny katalog med namnet **conf** i hello **hbaseapp** directory.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-152">Create a new directory named **conf** in hello **hbaseapp** directory.</span></span> <span data-ttu-id="e1a2b-153">I hello **conf** directory, skapa en fil med namnet **hbase-site.xml**.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-153">In hello **conf** directory, create a file named **hbase-site.xml**.</span></span> <span data-ttu-id="e1a2b-154">Använd följande hello som hello innehållet i hello-fil:</span><span class="sxs-lookup"><span data-stu-id="e1a2b-154">Use hello following as hello contents of hello file:</span></span>

        <?xml version="1.0"?>
        <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
        <!--
        /**
          * Copyright 2010 hello Apache Software Foundation
          *
          * Licensed toohello Apache Software Foundation (ASF) under one
          * or more contributor license agreements.  See hello NOTICE file
          * distributed with this work for additional information
          * regarding copyright ownership.  hello ASF licenses this file
          * tooyou under hello Apache License, Version 2.0 (the
          * "License"); you may not use this file except in compliance
          * with hello License.  You may obtain a copy of hello License at
          *
          *     http://www.apache.org/licenses/LICENSE-2.0
          *
          * Unless required by applicable law or agreed tooin writing, software
          * distributed under hello License is distributed on an "AS IS" BASIS,
          * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
          * See hello License for hello specific language governing permissions and
          * limitations under hello License.
          */
        -->
        <configuration>
          <property>
            <name>hbase.cluster.distributed</name>
            <value>true</value>
          </property>
          <property>
            <name>hbase.zookeeper.quorum</name>
            <value>zookeeper0,zookeeper1,zookeeper2</value>
          </property>
          <property>
            <name>hbase.zookeeper.property.clientPort</name>
            <value>2181</value>
          </property>
        </configuration>

    <span data-ttu-id="e1a2b-155">Den här filen kommer att använda tooload hello HBase konfiguration för ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-155">This file will be used tooload hello HBase configuration for an HDInsight cluster.</span></span>

   > [!NOTE]
   > <span data-ttu-id="e1a2b-156">Detta är en minimal hbase-site.xml-fil och den innehåller hello utan minimikrav för hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-156">This is a minimal hbase-site.xml file, and it contains hello bare minimum settings for hello HDInsight cluster.</span></span>

6. <span data-ttu-id="e1a2b-157">Spara hello **hbase-site.xml** fil.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-157">Save hello **hbase-site.xml** file.</span></span>

## <a name="create-hello-application"></a><span data-ttu-id="e1a2b-158">Skapa hello program</span><span class="sxs-lookup"><span data-stu-id="e1a2b-158">Create hello application</span></span>
1. <span data-ttu-id="e1a2b-159">Gå toohello **hbaseapp\src\main\java\com\microsoft\examples** katalog och Byt namn på hello app.java filen för**CreateTable.java**.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-159">Go toohello **hbaseapp\src\main\java\com\microsoft\examples** directory and rename hello app.java file too**CreateTable.java**.</span></span>
2. <span data-ttu-id="e1a2b-160">Öppna hello **CreateTable.java** filen och ersätter hello befintligt innehåll med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="e1a2b-160">Open hello **CreateTable.java** file and replace hello existing contents with hello following code:</span></span>

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HBaseAdmin;
        import org.apache.hadoop.hbase.HTableDescriptor;
        import org.apache.hadoop.hbase.TableName;
        import org.apache.hadoop.hbase.HColumnDescriptor;
        import org.apache.hadoop.hbase.client.HTable;
        import org.apache.hadoop.hbase.client.Put;
        import org.apache.hadoop.hbase.util.Bytes;

        public class CreateTable {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Example of setting zookeeper values for HDInsight
            // in code instead of an hbase-site.xml file
            //
            // config.set("hbase.zookeeper.quorum",
            //            "zookeepernode0,zookeepernode1,zookeepernode2");
            //config.set("hbase.zookeeper.property.clientPort", "2181");
            //config.set("hbase.cluster.distributed", "true");
            // hello following sets hello znode root for Linux-based HDInsight
            //config.set("zookeeper.znode.parent","/hbase-unsecure");

            // create an admin object using hello config
            HBaseAdmin admin = new HBaseAdmin(config);

            // create hello table...
            HTableDescriptor tableDescriptor = new HTableDescriptor(TableName.valueOf("people"));
            // ... with two column families
            tableDescriptor.addFamily(new HColumnDescriptor("name"));
            tableDescriptor.addFamily(new HColumnDescriptor("contactinfo"));
            admin.createTable(tableDescriptor);

            // define some people
            String[][] people = {
                { "1", "Marcel", "Haddad", "marcel@fabrikam.com"},
                { "2", "Franklin", "Holtz", "franklin@contoso.com" },
                { "3", "Dwayne", "McKee", "dwayne@fabrikam.com" },
                { "4", "Rae", "Schroeder", "rae@contoso.com" },
                { "5", "Rosalie", "burton", "rosalie@fabrikam.com"},
                { "6", "Gabriela", "Ingram", "gabriela@contoso.com"} };

            HTable table = new HTable(config, "people");

            // Add each person toohello table
            //   Use hello `name` column family for hello name
            //   Use hello `contactinfo` column family for hello email
            for (int i = 0; i< people.length; i++) {
              Put person = new Put(Bytes.toBytes(people[i][0]));
              person.add(Bytes.toBytes("name"), Bytes.toBytes("first"), Bytes.toBytes(people[i][1]));
              person.add(Bytes.toBytes("name"), Bytes.toBytes("last"), Bytes.toBytes(people[i][2]));
              person.add(Bytes.toBytes("contactinfo"), Bytes.toBytes("email"), Bytes.toBytes(people[i][3]));
              table.put(person);
            }
            // flush commits and close hello table
            table.flushCommits();
            table.close();
          }
        }

    <span data-ttu-id="e1a2b-161">Detta är hello **CreateTable** -klassen, som skapar en tabell med namnet **personer** och fylla det med vissa fördefinierade användare.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-161">This is hello **CreateTable** class, which will create a table named **people** and populate it with some predefined users.</span></span>
3. <span data-ttu-id="e1a2b-162">Spara hello **CreateTable.java** fil.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-162">Save hello **CreateTable.java** file.</span></span>
4. <span data-ttu-id="e1a2b-163">I hello **hbaseapp\src\main\java\com\microsoft\examples** directory, skapa en ny fil med namnet **SearchByEmail.java**.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-163">In hello **hbaseapp\src\main\java\com\microsoft\examples** directory, create a new file named **SearchByEmail.java**.</span></span> <span data-ttu-id="e1a2b-164">Använd hello efter koden som hello innehållet i den här filen:</span><span class="sxs-lookup"><span data-stu-id="e1a2b-164">Use hello following code as hello contents of this file:</span></span>

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HTable;
        import org.apache.hadoop.hbase.client.Scan;
        import org.apache.hadoop.hbase.client.ResultScanner;
        import org.apache.hadoop.hbase.client.Result;
        import org.apache.hadoop.hbase.filter.RegexStringComparator;
        import org.apache.hadoop.hbase.filter.SingleColumnValueFilter;
        import org.apache.hadoop.hbase.filter.CompareFilter.CompareOp;
        import org.apache.hadoop.hbase.util.Bytes;
        import org.apache.hadoop.util.GenericOptionsParser;

        public class SearchByEmail {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Use GenericOptionsParser tooget only hello parameters toohello class
            // and not all hello parameters passed (when using WebHCat for example)
            String[] otherArgs = new GenericOptionsParser(config, args).getRemainingArgs();
            if (otherArgs.length != 1) {
              System.out.println("usage: [regular expression]");
              System.exit(-1);
            }

            // Open hello table
            HTable table = new HTable(config, "people");

            // Define hello family and qualifiers toobe used
            byte[] contactFamily = Bytes.toBytes("contactinfo");
            byte[] emailQualifier = Bytes.toBytes("email");
            byte[] nameFamily = Bytes.toBytes("name");
            byte[] firstNameQualifier = Bytes.toBytes("first");
            byte[] lastNameQualifier = Bytes.toBytes("last");

            // Create a new regex filter
            RegexStringComparator emailFilter = new RegexStringComparator(otherArgs[0]);
            // Attach hello regex filter tooa filter
            //   for hello email column
            SingleColumnValueFilter filter = new SingleColumnValueFilter(
              contactFamily,
              emailQualifier,
              CompareOp.EQUAL,
              emailFilter
            );

            // Create a scan and set hello filter
            Scan scan = new Scan();
            scan.setFilter(filter);

            // Get hello results
            ResultScanner results = table.getScanner(scan);
            // Iterate over results and print  values
            for (Result result : results ) {
              String id = new String(result.getRow());
              byte[] firstNameObj = result.getValue(nameFamily, firstNameQualifier);
              String firstName = new String(firstNameObj);
              byte[] lastNameObj = result.getValue(nameFamily, lastNameQualifier);
              String lastName = new String(lastNameObj);
              System.out.println(firstName + " " + lastName + " - ID: " + id);
              byte[] emailObj = result.getValue(contactFamily, emailQualifier);
              String email = new String(emailObj);
              System.out.println(firstName + " " + lastName + " - " + email + " - ID: " + id);
            }
            results.close();
            table.close();
          }
        }

    <span data-ttu-id="e1a2b-165">Hej **SearchByEmail** klassen kan vara används tooquery för rader av e-postadress.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-165">hello **SearchByEmail** class can be used tooquery for rows by email address.</span></span> <span data-ttu-id="e1a2b-166">Eftersom den använder ett reguljärt uttryck för filter kan du ange en sträng eller ett reguljärt uttryck när hello-klassen.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-166">Because it uses a regular expression filter, you can provide either a string or a regular expression when using hello class.</span></span>
5. <span data-ttu-id="e1a2b-167">Spara hello **SearchByEmail.java** fil.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-167">Save hello **SearchByEmail.java** file.</span></span>
6. <span data-ttu-id="e1a2b-168">I hello **hbaseapp\src\main\hava\com\microsoft\examples** directory, skapa en ny fil med namnet **DeleteTable.java**.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-168">In hello **hbaseapp\src\main\hava\com\microsoft\examples** directory, create a new file named **DeleteTable.java**.</span></span> <span data-ttu-id="e1a2b-169">Använd hello efter koden som hello innehållet i den här filen:</span><span class="sxs-lookup"><span data-stu-id="e1a2b-169">Use hello following code as hello contents of this file:</span></span>

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HBaseAdmin;

        public class DeleteTable {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Create an admin object using hello config
            HBaseAdmin admin = new HBaseAdmin(config);

            // Disable, and then delete hello table
            admin.disableTable("people");
            admin.deleteTable("people");
          }
        }

    <span data-ttu-id="e1a2b-170">Den här klassen är för att rensa det här exemplet genom att inaktivera och släppa hello tabell skapas av hello **CreateTable** klass.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-170">This class is for cleaning up this example by disabling and dropping hello table created by hello **CreateTable** class.</span></span>
7. <span data-ttu-id="e1a2b-171">Spara hello **DeleteTable.java** fil.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-171">Save hello **DeleteTable.java** file.</span></span>

## <a name="build-and-package-hello-application"></a><span data-ttu-id="e1a2b-172">Bygg- och paketet hello-program</span><span class="sxs-lookup"><span data-stu-id="e1a2b-172">Build and package hello application</span></span>
1. <span data-ttu-id="e1a2b-173">Öppna en kommandotolk och ändra kataloger toohello **hbaseapp** directory.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-173">Open a command prompt and change directories toohello **hbaseapp** directory.</span></span>
2. <span data-ttu-id="e1a2b-174">Använd följande kommando toobuild JAR-filen som innehåller programmet hello hello:</span><span class="sxs-lookup"><span data-stu-id="e1a2b-174">Use hello following command toobuild a JAR file that contains hello application:</span></span>

        mvn clean package

    <span data-ttu-id="e1a2b-175">Detta rensar alla tidigare build-artefakter, hämtas eventuella beroenden som inte redan har installerats, sedan bygger och programmet hello-paket.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-175">This cleans any previous build artifacts, downloads any dependencies that have not already been installed, then builds and packages hello application.</span></span>
3. <span data-ttu-id="e1a2b-176">När hello-kommandot har slutförts hello **hbaseapp\target** katalogen innehåller en fil med namnet **hbaseapp-1.0-SNAPSHOT.jar**.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-176">When hello command completes, hello **hbaseapp\target** directory contains a file named **hbaseapp-1.0-SNAPSHOT.jar**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="e1a2b-177">Hej **hbaseapp-1.0-SNAPSHOT.jar** filen är en uber jar (kallas ibland för en fat jar) som innehåller alla hello beroenden krävs toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-177">hello **hbaseapp-1.0-SNAPSHOT.jar** file is an uber jar (sometimes called a fat jar,) which contains all hello dependencies required toorun hello application.</span></span>

## <a name="upload-hello-jar-file-and-start-a-job"></a><span data-ttu-id="e1a2b-178">Överför hello JAR-filen och starta ett jobb</span><span class="sxs-lookup"><span data-stu-id="e1a2b-178">Upload hello JAR file and start a job</span></span>
<span data-ttu-id="e1a2b-179">Det finns många sätt tooupload ett filen tooyour HDInsight-kluster, enligt beskrivningen i [överföra data för Hadoop-jobb i HDInsight](hdinsight-upload-data.md).</span><span class="sxs-lookup"><span data-stu-id="e1a2b-179">There are many ways tooupload a file tooyour HDInsight cluster, as described in [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md).</span></span> <span data-ttu-id="e1a2b-180">hello använda följande Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-180">hello following steps use Azure PowerShell.</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

1. <span data-ttu-id="e1a2b-181">Efter att installera och konfigurera Azure PowerShell, skapa en ny fil med namnet **hbase runner.psm1**.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-181">After installing and configuring Azure PowerShell, create a new file named **hbase-runner.psm1**.</span></span> <span data-ttu-id="e1a2b-182">Använd följande hello som hello innehållet i den här filen:</span><span class="sxs-lookup"><span data-stu-id="e1a2b-182">Use hello following as hello contents of this file:</span></span>

        <#
        .SYNOPSIS
        Copies a file toohello primary storage of an HDInsight cluster.
        .DESCRIPTION
        Copies a file from a local directory toohello blob container for
        hello HDInsight cluster.
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.CreateTable"
        -clusterName "MyHDInsightCluster"

        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
        -clusterName "MyHDInsightCluster"
        -emailRegex "contoso.com"

        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
        -clusterName "MyHDInsightCluster"
        -emailRegex "^r" -showErr
        #>

        function Start-HBaseExample {
        [CmdletBinding(SupportsShouldProcess = $true)]
        param(
        #hello class toorun
        [Parameter(Mandatory = $true)]
        [String]$className,

        #hello name of hello HDInsight cluster
        [Parameter(Mandatory = $true)]
        [String]$clusterName,

        #Only used when using SearchByEmail
        [Parameter(Mandatory = $false)]
        [String]$emailRegex,

        #Use if you want toosee stderr output
        [Parameter(Mandatory = $false)]
        [Switch]$showErr
        )

        Set-StrictMode -Version 3

        # Is hello Azure module installed?
        FindAzure

        # Get hello login for hello HDInsight cluster
        $creds=Get-Credential -Message "Enter hello login for hello cluster" -UserName "admin"

        # hello JAR
        $jarFile = "wasb:///example/jars/hbaseapp-1.0-SNAPSHOT.jar"

        # hello job definition
        $jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
            -JarFile $jarFile `
            -ClassName $className `
            -Arguments $emailRegex

        # Get hello job output
        $job = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $jobDefinition `
            -HttpCredential $creds
        Write-Host "Wait for hello job toocomplete ..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
        if($showErr)
        {
        Write-Host "STDERR"
        Get-AzureRmHDInsightJobOutput `
                    -Clustername $clusterName `
                    -JobId $job.JobId `
                    -HttpCredential $creds `
                    -DisplayOutputType StandardError
        }
        Write-Host "Display hello standard output ..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
                    -Clustername $clusterName `
                    -JobId $job.JobId `
                    -HttpCredential $creds
        }

        <#
        .SYNOPSIS
        Copies a file toohello primary storage of an HDInsight cluster.
        .DESCRIPTION
        Copies a file from a local directory toohello blob container for
        hello HDInsight cluster.
        .EXAMPLE
        Add-HDInsightFile -localPath "C:\temp\data.txt"
        -destinationPath "example/data/data.txt"
        -ClusterName "MyHDInsightCluster"
        .EXAMPLE
        Add-HDInsightFile -localPath "C:\temp\data.txt"
        -destinationPath "example/data/data.txt"
        -ClusterName "MyHDInsightCluster"
        -Container "MyContainer"
        #>

        function Add-HDInsightFile {
            [CmdletBinding(SupportsShouldProcess = $true)]
            param(
                #hello path toohello local file.
                [Parameter(Mandatory = $true)]
                [String]$localPath,

                #hello destination path and file name, relative toohello root of hello container.
                [Parameter(Mandatory = $true)]
                [String]$destinationPath,

                #hello name of hello HDInsight cluster
                [Parameter(Mandatory = $true)]
                [String]$clusterName,

                #If specified, overwrites existing files without prompting
                [Parameter(Mandatory = $false)]
                [Switch]$force
            )

            Set-StrictMode -Version 3

            # Is hello Azure module installed?
            FindAzure

            # Get authentication for hello cluster
            $creds=Get-Credential

            # Does hello local path exist?
            if (-not (Test-Path $localPath))
            {
                throw "Source path '$localPath' does not exist."
            }

            # Get hello primary storage container
            $storage = GetStorage -clusterName $clusterName

            # Upload file toostorage, overwriting existing files if -force was used.
            Set-AzureStorageBlobContent -File $localPath `
                -Blob $destinationPath `
                -force:$force `
                -Container $storage.container `
                -Context $storage.context
        }

        function FindAzure {
            # Is there an active Azure subscription?
            $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
            if(-not($sub))
            {
                throw "No active Azure subscription found! If you have a subscription, use hello Login-AzureRmAccount cmdlet toologin tooyour subscription."
            }
        }

        function GetStorage {
            param(
                [Parameter(Mandatory = $true)]
                [String]$clusterName
            )
            $hdi = Get-AzureRmHDInsightCluster -ClusterName $clusterName
            # Does hello cluster exist?
            if (!$hdi)
            {
                throw "HDInsight cluster '$clusterName' does not exist."
            }
            # Create a return object for context & container
            $return = @{}
            $storageAccounts = @{}

            # Get storage information
            $resourceGroup = $hdi.ResourceGroup
            $storageAccountName=$hdi.DefaultStorageAccount.split('.')[0]
            $container=$hdi.DefaultStorageContainer
            $storageAccountKey=(Get-AzureRmStorageAccountKey `
                -Name $storageAccountName `
            -ResourceGroupName $resourceGroup)[0].Value
            # Get hello resource group, in case we need that
            $return.resourceGroup = $resourceGroup
            # Get hello storage context, as we can't depend
            # on using hello default storage context
            $return.context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
            # Get hello container, so we know where to
            # find/store blobs
            $return.container = $container
            # Return storage accounts toosupport finding all accounts for
            # a cluster
            $return.storageAccount = $storageAccountName
            $return.storageAccountKey = $storageAccountKey

            return $return
        }
        # Only export hello verb-phrase things
        export-modulemember *-*

    <span data-ttu-id="e1a2b-183">Denna fil innehåller två moduler:</span><span class="sxs-lookup"><span data-stu-id="e1a2b-183">This file contains two modules:</span></span>

   * <span data-ttu-id="e1a2b-184">**Lägg till HDInsightFile** -används tooupload filer tooHDInsight</span><span class="sxs-lookup"><span data-stu-id="e1a2b-184">**Add-HDInsightFile** - used tooupload files tooHDInsight</span></span>
   * <span data-ttu-id="e1a2b-185">**Start-HBaseExample** -används toorun hello klasser som skapats tidigare</span><span class="sxs-lookup"><span data-stu-id="e1a2b-185">**Start-HBaseExample** - used toorun hello classes created earlier</span></span>
2. <span data-ttu-id="e1a2b-186">Spara hello **hbase runner.psm1** fil.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-186">Save hello **hbase-runner.psm1** file.</span></span>
3. <span data-ttu-id="e1a2b-187">Öppna ett nytt Azure PowerShell-fönster, ändra kataloger toohello **hbaseapp** directory, och sedan kör hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-187">Open a new Azure PowerShell window, change directories toohello **hbaseapp** directory, and then run hello following command.</span></span>

        PS C:\ Import-Module c:\path\to\hbase-runner.psm1

    <span data-ttu-id="e1a2b-188">Ändra hello toohello sökvägen för hello **hbase runner.psm1** filen som skapades tidigare.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-188">Change hello path toohello location of hello **hbase-runner.psm1** file created earlier.</span></span> <span data-ttu-id="e1a2b-189">Detta registrerar hello-modulen för den här Azure PowerShell-sessionen.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-189">This registers hello module for this Azure PowerShell session.</span></span>
4. <span data-ttu-id="e1a2b-190">Använd hello följande kommando tooupload hello **hbaseapp-1.0-SNAPSHOT.jar** tooyour HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-190">Use hello following command tooupload hello **hbaseapp-1.0-SNAPSHOT.jar** tooyour HDInsight cluster.</span></span>

        Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername

    <span data-ttu-id="e1a2b-191">Ersätt **hdinsightclustername** med hello namnet på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-191">Replace **hdinsightclustername** with hello name of your HDInsight cluster.</span></span> <span data-ttu-id="e1a2b-192">hello kommandot överför hello **hbaseapp-1.0-SNAPSHOT.jar** toohello **exempel/burkar** plats i hello primära lagringsplatsen för HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-192">hello command uploads hello **hbaseapp-1.0-SNAPSHOT.jar** toohello **example/jars** location in hello primary storage for your HDInsight cluster.</span></span>
5. <span data-ttu-id="e1a2b-193">När hello filer överförs används hello följande kod toocreate en tabell med hello **hbaseapp**:</span><span class="sxs-lookup"><span data-stu-id="e1a2b-193">After hello files are uploaded, use hello following code toocreate a table using hello **hbaseapp**:</span></span>

        Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername

    <span data-ttu-id="e1a2b-194">Ersätt **hdinsightclustername** med hello namnet på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-194">Replace **hdinsightclustername** with hello name of your HDInsight cluster.</span></span>

    <span data-ttu-id="e1a2b-195">Det här kommandot skapar en ny tabell med namnet **personer** i ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-195">This command creates a new table named **people** in your HDInsight cluster.</span></span> <span data-ttu-id="e1a2b-196">Det här kommandot visar inte några utdata i hello konsolfönstret.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-196">This command does not show any output in hello console window.</span></span>
6. <span data-ttu-id="e1a2b-197">toosearch för poster i hello tabell, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="e1a2b-197">toosearch for entries in hello table, use hello following command:</span></span>

        Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com

    <span data-ttu-id="e1a2b-198">Ersätt **hdinsightclustername** med hello namnet på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-198">Replace **hdinsightclustername** with hello name of your HDInsight cluster.</span></span>

    <span data-ttu-id="e1a2b-199">Detta kommando använder hello **SearchByEmail** klassen toosearch för alla rader där hello **contactinformation** kolumnfamilj och hello **e-post** kolumnen innehåller hello sträng **contoso.com**. Du bör få hello följande resultat:</span><span class="sxs-lookup"><span data-stu-id="e1a2b-199">This command uses hello **SearchByEmail** class toosearch for any rows where hello **contactinformation** column family and hello **email** column, contains hello string **contoso.com**. You should receive hello following results:</span></span>

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    <span data-ttu-id="e1a2b-200">Med hjälp av **fabrikam.com** för hello `-emailRegex` värde returnerar hello-användare som har **fabrikam.com** hello e-fältet.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-200">Using **fabrikam.com** for hello `-emailRegex` value returns hello users that have **fabrikam.com** in hello email field.</span></span> <span data-ttu-id="e1a2b-201">Eftersom den här sökningen implementeras med hjälp av ett reguljärt uttryck baserat filter, du kan också ange reguljära uttryck som **^ r**, vilka returnerar poster där hello e-post som börjar med hello bokstaven ”r”.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-201">Since this search is implemented by using a regular expression-based filter, you can also enter regular expressions, such as **^r**, which returns entries where hello email begins with hello letter 'r'.</span></span>

## <a name="delete-hello-table"></a><span data-ttu-id="e1a2b-202">Ta bort hello tabell</span><span class="sxs-lookup"><span data-stu-id="e1a2b-202">Delete hello table</span></span>
<span data-ttu-id="e1a2b-203">När du är klar med hello exemplet, Använd hello följande kommando från hello Azure PowerShell-session toodelete hello **personer** tabell som används i det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="e1a2b-203">When you are done with hello example, use hello following command from hello Azure PowerShell session toodelete hello **people** table used in this example:</span></span>

    Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername

<span data-ttu-id="e1a2b-204">Ersätt **hdinsightclustername** med hello namnet på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-204">Replace **hdinsightclustername** with hello name of your HDInsight cluster.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="e1a2b-205">Felsökning</span><span class="sxs-lookup"><span data-stu-id="e1a2b-205">Troubleshooting</span></span>
### <a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a><span data-ttu-id="e1a2b-206">Inga resultat eller oväntade resultat när du använder Start HBaseExample</span><span class="sxs-lookup"><span data-stu-id="e1a2b-206">No results or unexpected results when using Start-HBaseExample</span></span>
<span data-ttu-id="e1a2b-207">Använd hello `-showErr` parametern tooview hello standardfel (STDERR) som skapas när hello jobb som körs.</span><span class="sxs-lookup"><span data-stu-id="e1a2b-207">Use hello `-showErr` parameter tooview hello standard error (STDERR) that is produced while running hello job.</span></span>
