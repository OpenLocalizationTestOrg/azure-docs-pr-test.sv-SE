---
title: aaaJava HBase klient - Azure HDInsight | Microsoft Docs
description: "Lär dig hur toouse Apache Maven toobuild en Java-baserad Apache HBase programmet sedan distribuera den tooHBase på Azure HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: 
ms.assetid: 1d1ed180-e0f4-4d1c-b5ea-72e0eda643bc
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: larryfr
ms.openlocfilehash: 41ef92b2900280dd59089c4fa40686c44133b337
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="build-java-applications-for-apache-hbase"></a><span data-ttu-id="4af06-103">Skapa Java-program för Apache HBase</span><span class="sxs-lookup"><span data-stu-id="4af06-103">Build Java applications for Apache HBase</span></span>

<span data-ttu-id="4af06-104">Lär dig hur toocreate en [Apache HBase](http://hbase.apache.org/) program i Java.</span><span class="sxs-lookup"><span data-stu-id="4af06-104">Learn how toocreate an [Apache HBase](http://hbase.apache.org/) application in Java.</span></span> <span data-ttu-id="4af06-105">Sedan använda hello programmet med HBase på Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="4af06-105">Then use hello application with HBase on Azure HDInsight.</span></span>

<span data-ttu-id="4af06-106">hello stegen i det här dokumentet används [Maven](http://maven.apache.org/) toocreate och bygga hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="4af06-106">hello steps in this document use [Maven](http://maven.apache.org/) toocreate and build hello project.</span></span> <span data-ttu-id="4af06-107">Maven är en programvara projekthantering och förståelse verktyg som gör att toobuild programvara, dokumentation och rapporter för Java-projekt.</span><span class="sxs-lookup"><span data-stu-id="4af06-107">Maven is a software project management and comprehension tool that allows you toobuild software, documentation, and reports for Java projects.</span></span>

> [!NOTE]
> <span data-ttu-id="4af06-108">hello har stegen i det här dokumentet senast har testats med HDInsight 3,6.</span><span class="sxs-lookup"><span data-stu-id="4af06-108">hello steps in this document were most recently tested with HDInsight 3.6.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4af06-109">hello stegen i det här dokumentet kräver ett HDInsight-kluster som använder Linux.</span><span class="sxs-lookup"><span data-stu-id="4af06-109">hello steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="4af06-110">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="4af06-110">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="4af06-111">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="4af06-111">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="requirements"></a><span data-ttu-id="4af06-112">Krav</span><span class="sxs-lookup"><span data-stu-id="4af06-112">Requirements</span></span>

* <span data-ttu-id="4af06-113">[Java platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 8 eller senare.</span><span class="sxs-lookup"><span data-stu-id="4af06-113">[Java platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 8 or later.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4af06-114">HDInsight 3.5 och större kräver Java 8.</span><span class="sxs-lookup"><span data-stu-id="4af06-114">HDInsight 3.5 and greater requires Java 8.</span></span> <span data-ttu-id="4af06-115">Tidigare versioner av HDInsight kräver Java 7.</span><span class="sxs-lookup"><span data-stu-id="4af06-115">Earlier versions of HDInsight require Java 7.</span></span>

* [<span data-ttu-id="4af06-116">Maven 3.</span><span class="sxs-lookup"><span data-stu-id="4af06-116">Maven</span></span>](http://maven.apache.org/)

* [<span data-ttu-id="4af06-117">Ett Linux-baserade Azure HDInsight-kluster med HBase</span><span class="sxs-lookup"><span data-stu-id="4af06-117">A Linux-based Azure HDInsight cluster with HBase</span></span>](hdinsight-hbase-tutorial-get-started-linux.md#create-hbase-cluster)

  > [!NOTE]
  > <span data-ttu-id="4af06-118">hello stegen i det här dokumentet har testats med HDInsight-kluster version 3.4 och 3.5.</span><span class="sxs-lookup"><span data-stu-id="4af06-118">hello steps in this document have been tested with HDInsight cluster versions 3.4 and 3.5.</span></span> <span data-ttu-id="4af06-119">hello standardvärden som i exemplen är för ett kluster i HDInsight 3.5.</span><span class="sxs-lookup"><span data-stu-id="4af06-119">hello default values provided in examples are for a HDInsight 3.5 cluster.</span></span>

## <a name="create-hello-project"></a><span data-ttu-id="4af06-120">Skapa hello-projekt</span><span class="sxs-lookup"><span data-stu-id="4af06-120">Create hello project</span></span>

1. <span data-ttu-id="4af06-121">Ändra kataloger toohello plats där du vill toocreate hello projekt, till exempel från kommandoraden i din utvecklingsmiljö hello `cd code\hbase`.</span><span class="sxs-lookup"><span data-stu-id="4af06-121">From hello command line in your development environment, change directories toohello location where you want toocreate hello project, for example, `cd code\hbase`.</span></span>

2. <span data-ttu-id="4af06-122">Använd hello **mvn** kommandot, som installeras med Maven toogenerate hello scaffold-teknik för hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="4af06-122">Use hello **mvn** command, which is installed with Maven, toogenerate hello scaffolding for hello project.</span></span>

    ```bash
    mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

    > [!NOTE]
    > <span data-ttu-id="4af06-123">Om du använder PowerShell kan du ange hello `-D` parametrar inom dubbla citattecken.</span><span class="sxs-lookup"><span data-stu-id="4af06-123">If you are using PowerShell, you must enclose hello `-D` parameters in double quotes.</span></span>
    >
    > `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=hbaseapp" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`

    <span data-ttu-id="4af06-124">Det här kommandot skapar en katalog med samma namn som hello hello **artefakt-ID** parameter (**hbaseapp** i det här exemplet.) Den här katalogen innehåller hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="4af06-124">This command creates a directory with hello same name as hello **artifactID** parameter (**hbaseapp** in this example.) This directory contains hello following items:</span></span>

   * <span data-ttu-id="4af06-125">**pom.XML**: hello projektet Object Model ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) innehåller information och konfiguration information används toobuild hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="4af06-125">**pom.xml**:  hello Project Object Model ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) contains information and configuration details used toobuild hello project.</span></span>
   * <span data-ttu-id="4af06-126">**SRC**: hello-katalog som innehåller hello **main/java/com/microsoft/exempel** directory, där du skapar hello program.</span><span class="sxs-lookup"><span data-stu-id="4af06-126">**src**: hello directory that contains hello **main/java/com/microsoft/examples** directory, where you author hello application.</span></span>

3. <span data-ttu-id="4af06-127">Ta bort hello `src/test/java/com/microsoft/examples/apptest.java` fil.</span><span class="sxs-lookup"><span data-stu-id="4af06-127">Delete hello `src/test/java/com/microsoft/examples/apptest.java` file.</span></span> <span data-ttu-id="4af06-128">Det är inte användas i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="4af06-128">It is not be used in this example.</span></span>

## <a name="update-hello-project-object-model"></a><span data-ttu-id="4af06-129">Uppdatera hello projekt-objektmodell</span><span class="sxs-lookup"><span data-stu-id="4af06-129">Update hello Project Object Model</span></span>

1. <span data-ttu-id="4af06-130">Redigera hello `pom.xml` och Lägg till följande kod i hello hello `<dependencies>` avsnitt:</span><span class="sxs-lookup"><span data-stu-id="4af06-130">Edit hello `pom.xml` file and add hello following code inside hello `<dependencies>` section:</span></span>

   ```xml
    <dependency>
        <groupId>org.apache.hbase</groupId>
        <artifactId>hbase-client</artifactId>
        <version>1.1.2</version>
    </dependency>
    <dependency>
        <groupId>org.apache.phoenix</groupId>
        <artifactId>phoenix-core</artifactId>
        <version>4.4.0-HBase-1.1</version>
    </dependency>
   ```

    <span data-ttu-id="4af06-131">Det här avsnittet visar hello projektet behöver **hbase-klient** och **phoenix kärnor** komponenter.</span><span class="sxs-lookup"><span data-stu-id="4af06-131">This section indicates that hello project needs **hbase-client** and **phoenix-core** components.</span></span> <span data-ttu-id="4af06-132">Dessa beroenden hämtas vid kompileringen, från hello standarddatabas Maven.</span><span class="sxs-lookup"><span data-stu-id="4af06-132">At compile time, these dependencies are downloaded from hello default Maven repository.</span></span> <span data-ttu-id="4af06-133">Du kan använda hello [Maven centrala lagringsplatsen Sök](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) toolearn mer om detta beroende.</span><span class="sxs-lookup"><span data-stu-id="4af06-133">You can use hello [Maven Central Repository Search](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) toolearn more about this dependency.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="4af06-134">hello versionsnumret för hello hbase-klienten måste matcha hello version av HBase som ingår i ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="4af06-134">hello version number of hello hbase-client must match hello version of HBase that is provided with your HDInsight cluster.</span></span> <span data-ttu-id="4af06-135">Använd följande tabell toofind hello rätt versionsnummer hello.</span><span class="sxs-lookup"><span data-stu-id="4af06-135">Use hello following table toofind hello correct version number.</span></span>

   | <span data-ttu-id="4af06-136">HDInsight-kluster av version</span><span class="sxs-lookup"><span data-stu-id="4af06-136">HDInsight cluster version</span></span> | <span data-ttu-id="4af06-137">HBase version toouse</span><span class="sxs-lookup"><span data-stu-id="4af06-137">HBase version toouse</span></span> |
   | --- | --- |
   | <span data-ttu-id="4af06-138">3.2</span><span class="sxs-lookup"><span data-stu-id="4af06-138">3.2</span></span> |<span data-ttu-id="4af06-139">0.98.4-hadoop2</span><span class="sxs-lookup"><span data-stu-id="4af06-139">0.98.4-hadoop2</span></span> |
   | <span data-ttu-id="4af06-140">3.3, 3.4, 3.5 och 3,6</span><span class="sxs-lookup"><span data-stu-id="4af06-140">3.3, 3.4, 3.5, and 3.6</span></span> |<span data-ttu-id="4af06-141">1.1.2</span><span class="sxs-lookup"><span data-stu-id="4af06-141">1.1.2</span></span> |

    <span data-ttu-id="4af06-142">Mer information om HDInsight-versioner och komponenter finns [vad är hello olika Hadoop-komponenter tillgänglig med HDInsight](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="4af06-142">For more information on HDInsight versions and components, see [What are hello different Hadoop components available with HDInsight](hdinsight-component-versioning.md).</span></span>

3. <span data-ttu-id="4af06-143">Lägg till följande kod toohello hello **pom.xml** fil.</span><span class="sxs-lookup"><span data-stu-id="4af06-143">Add hello following code toohello **pom.xml** file.</span></span> <span data-ttu-id="4af06-144">Den här texten måste finnas i hello `<project>...</project>` taggar i hello-fil, till exempel mellan `</dependencies>` och `</project>`.</span><span class="sxs-lookup"><span data-stu-id="4af06-144">This text must be inside hello `<project>...</project>` tags in hello file, for example, between `</dependencies>` and `</project>`.</span></span>

   ```xml
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
                <source>1.8</source>
                <target>1.8</target>
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
   ```

    <span data-ttu-id="4af06-145">Det här avsnittet konfigureras en resurs (`conf/hbase-site.xml`) som innehåller konfigurationsinformation för HBase.</span><span class="sxs-lookup"><span data-stu-id="4af06-145">This section configures a resource (`conf/hbase-site.xml`) that contains configuration information for HBase.</span></span>

   > [!NOTE]
   > <span data-ttu-id="4af06-146">Du kan också ange konfigurationsvärden via kod.</span><span class="sxs-lookup"><span data-stu-id="4af06-146">You can also set configuration values via code.</span></span> <span data-ttu-id="4af06-147">Hello kommentarer i hello `CreateTable` exempel.</span><span class="sxs-lookup"><span data-stu-id="4af06-147">See hello comments in hello `CreateTable` example.</span></span>

    <span data-ttu-id="4af06-148">Det här avsnittet konfigureras också hello [Maven-kompilatorn Plugin](http://maven.apache.org/plugins/maven-compiler-plugin/) och [Maven skugga Plugin](http://maven.apache.org/plugins/maven-shade-plugin/).</span><span class="sxs-lookup"><span data-stu-id="4af06-148">This section also configures hello [Maven Compiler Plugin](http://maven.apache.org/plugins/maven-compiler-plugin/) and [Maven Shade Plugin](http://maven.apache.org/plugins/maven-shade-plugin/).</span></span> <span data-ttu-id="4af06-149">hello kompilatorn plugin-programmet är används toocompile hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="4af06-149">hello compiler plug-in is used toocompile hello topology.</span></span> <span data-ttu-id="4af06-150">hello skugga plugin-programmet är dubbletter av används tooprevent licens hello JAR-paketet som skapas genom Maven.</span><span class="sxs-lookup"><span data-stu-id="4af06-150">hello shade plug-in is used tooprevent license duplication in hello JAR package that is built by Maven.</span></span> <span data-ttu-id="4af06-151">Det här plugin-programmet är används tooprevent ett ”duplicera licensfiler” fel vid körning på hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="4af06-151">This plugin is used tooprevent a "duplicate license files" error at run time on hello HDInsight cluster.</span></span> <span data-ttu-id="4af06-152">Med hjälp av maven-skugga-plugin-program med hello `ApacheLicenseResourceTransformer` implementering förhindrar hello-fel.</span><span class="sxs-lookup"><span data-stu-id="4af06-152">Using maven-shade-plugin with hello `ApacheLicenseResourceTransformer` implementation prevents hello error.</span></span>

    <span data-ttu-id="4af06-153">Hej maven-skugga-plugin-programmet ger också en uber jar som innehåller alla hello beroenden som krävs av hello program.</span><span class="sxs-lookup"><span data-stu-id="4af06-153">hello maven-shade-plugin also produces an uber jar that contains all hello dependencies required by hello application.</span></span>

4. <span data-ttu-id="4af06-154">Spara hello `pom.xml` fil.</span><span class="sxs-lookup"><span data-stu-id="4af06-154">Save hello `pom.xml` file.</span></span>

5. <span data-ttu-id="4af06-155">Skapa en katalog med namnet `conf` i hello `hbaseapp` directory.</span><span class="sxs-lookup"><span data-stu-id="4af06-155">Create a directory named `conf` in hello `hbaseapp` directory.</span></span> <span data-ttu-id="4af06-156">Den här katalogen finns används toohold konfigurationsinformation för att ansluta tooHBase.</span><span class="sxs-lookup"><span data-stu-id="4af06-156">This directory is used toohold configuration information for connecting tooHBase.</span></span>

6. <span data-ttu-id="4af06-157">Använd hello följande kommando toocopy hello HBase konfigurationen från hello HBase-kluster toohello `conf` directory.</span><span class="sxs-lookup"><span data-stu-id="4af06-157">Use hello following command toocopy hello HBase configuration from hello HBase cluster toohello `conf` directory.</span></span> <span data-ttu-id="4af06-158">Ersätt `USERNAME` med hello namnet för SSH-inloggning.</span><span class="sxs-lookup"><span data-stu-id="4af06-158">Replace `USERNAME` with hello name of your SSH login.</span></span> <span data-ttu-id="4af06-159">Ersätt `CLUSTERNAME` med ditt HDInsight-klustrets namn:</span><span class="sxs-lookup"><span data-stu-id="4af06-159">Replace `CLUSTERNAME` with your HDInsight cluster name:</span></span>

    ```bash
    scp USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml
    ```

   <span data-ttu-id="4af06-160">Mer information om hur du använder `ssh` och `scp`, se [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="4af06-160">For more information on using `ssh` and `scp`, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <a name="create-hello-application"></a><span data-ttu-id="4af06-161">Skapa hello program</span><span class="sxs-lookup"><span data-stu-id="4af06-161">Create hello application</span></span>

1. <span data-ttu-id="4af06-162">Gå toohello `hbaseapp/src/main/java/com/microsoft/examples` katalog och Byt namn på hello app.java filen för`CreateTable.java`.</span><span class="sxs-lookup"><span data-stu-id="4af06-162">Go toohello `hbaseapp/src/main/java/com/microsoft/examples` directory and rename hello app.java file too`CreateTable.java`.</span></span>

2. <span data-ttu-id="4af06-163">Öppna hello `CreateTable.java` filen och ersätter hello befintligt innehåll med hello följande text:</span><span class="sxs-lookup"><span data-stu-id="4af06-163">Open hello `CreateTable.java` file and replace hello existing contents with hello following text:</span></span>

   ```java
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
        //
        //NOTE: Actual zookeeper host names can be found using Ambari:
        //curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts"

        //Linux-based HDInsight clusters use /hbase-unsecure as hello znode parent
        config.set("zookeeper.znode.parent","/hbase-unsecure");

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
   ```

    <span data-ttu-id="4af06-164">Den här koden är hello **CreateTable** -klassen, som skapar en tabell med namnet **personer** och fylla det med vissa fördefinierade användare.</span><span class="sxs-lookup"><span data-stu-id="4af06-164">This code is hello **CreateTable** class, which creates a table named **people** and populate it with some predefined users.</span></span>

3. <span data-ttu-id="4af06-165">Spara hello `CreateTable.java` fil.</span><span class="sxs-lookup"><span data-stu-id="4af06-165">Save hello `CreateTable.java` file.</span></span>

4. <span data-ttu-id="4af06-166">I hello `hbaseapp/src/main/java/com/microsoft/examples` directory, skapa en fil med namnet `SearchByEmail.java`.</span><span class="sxs-lookup"><span data-stu-id="4af06-166">In hello `hbaseapp/src/main/java/com/microsoft/examples` directory, create a file named `SearchByEmail.java`.</span></span> <span data-ttu-id="4af06-167">Använd hello följande text som hello innehållet i den här filen:</span><span class="sxs-lookup"><span data-stu-id="4af06-167">Use hello following text as hello contents of this file:</span></span>

   ```java
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

        // Create a regex filter
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
   ```

    <span data-ttu-id="4af06-168">Hej **SearchByEmail** klassen kan vara används tooquery för rader av e-postadress.</span><span class="sxs-lookup"><span data-stu-id="4af06-168">hello **SearchByEmail** class can be used tooquery for rows by email address.</span></span> <span data-ttu-id="4af06-169">Eftersom den använder ett reguljärt uttryck för filter kan du ange en sträng eller ett reguljärt uttryck när hello-klassen.</span><span class="sxs-lookup"><span data-stu-id="4af06-169">Because it uses a regular expression filter, you can provide either a string or a regular expression when using hello class.</span></span>

5. <span data-ttu-id="4af06-170">Spara hello `SearchByEmail.java` fil.</span><span class="sxs-lookup"><span data-stu-id="4af06-170">Save hello `SearchByEmail.java` file.</span></span>

6. <span data-ttu-id="4af06-171">I hello `hbaseapp/src/main/hava/com/microsoft/examples` directory, skapa en fil med namnet `DeleteTable.java`.</span><span class="sxs-lookup"><span data-stu-id="4af06-171">In hello `hbaseapp/src/main/hava/com/microsoft/examples` directory, create a file named `DeleteTable.java`.</span></span> <span data-ttu-id="4af06-172">Använd hello följande text som hello innehållet i den här filen:</span><span class="sxs-lookup"><span data-stu-id="4af06-172">Use hello following text as hello contents of this file:</span></span>

   ```java
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
   ```

    <span data-ttu-id="4af06-173">Den här klassen som rensar hello HBase-tabeller som skapats i det här exemplet genom att inaktivera och släppa hello tabell skapas av hello `CreateTable` klass.</span><span class="sxs-lookup"><span data-stu-id="4af06-173">This class cleans up hello HBase tables created in this example by disabling and dropping hello table created by hello `CreateTable` class.</span></span>

7. <span data-ttu-id="4af06-174">Spara hello `DeleteTable.java` fil.</span><span class="sxs-lookup"><span data-stu-id="4af06-174">Save hello `DeleteTable.java` file.</span></span>

## <a name="build-and-package-hello-application"></a><span data-ttu-id="4af06-175">Bygg- och paketet hello-program</span><span class="sxs-lookup"><span data-stu-id="4af06-175">Build and package hello application</span></span>

1. <span data-ttu-id="4af06-176">Från hello `hbaseapp` katalog, Använd hello följande kommando toobuild JAR-filen som innehåller programmet hello:</span><span class="sxs-lookup"><span data-stu-id="4af06-176">From hello `hbaseapp` directory, use hello following command toobuild a JAR file that contains hello application:</span></span>

    ```bash
    mvn clean package
    ```

    <span data-ttu-id="4af06-177">Det här kommandot skapar och paket hello program till en .jar-fil.</span><span class="sxs-lookup"><span data-stu-id="4af06-177">This command builds and packages hello application into a .jar file.</span></span>

2. <span data-ttu-id="4af06-178">När hello-kommandot har slutförts hello `hbaseapp/target` katalogen innehåller en fil med namnet `hbaseapp-1.0-SNAPSHOT.jar`.</span><span class="sxs-lookup"><span data-stu-id="4af06-178">When hello command completes, hello `hbaseapp/target` directory contains a file named `hbaseapp-1.0-SNAPSHOT.jar`.</span></span>

   > [!NOTE]
   > <span data-ttu-id="4af06-179">Hej `hbaseapp-1.0-SNAPSHOT.jar` filen är en uber jar.</span><span class="sxs-lookup"><span data-stu-id="4af06-179">hello `hbaseapp-1.0-SNAPSHOT.jar` file is an uber jar.</span></span> <span data-ttu-id="4af06-180">Den innehåller alla hello beroenden krävs toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="4af06-180">It contains all hello dependencies required toorun hello application.</span></span>


## <a name="upload-hello-jar-and-run-jobs-ssh"></a><span data-ttu-id="4af06-181">Överför hello JAR och köra jobb (SSH)</span><span class="sxs-lookup"><span data-stu-id="4af06-181">Upload hello JAR and run jobs (SSH)</span></span>

<span data-ttu-id="4af06-182">Hej följande steg används `scp` toocopy hello JAR toohello primära huvudnod i din HBase på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="4af06-182">hello following steps use `scp` toocopy hello JAR toohello primary head node of your HBase on HDInsight cluster.</span></span> <span data-ttu-id="4af06-183">Hej `ssh` kommandot är används tooconnect toohello klustret och köra hello exempel direkt på hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="4af06-183">hello `ssh` command is then used tooconnect toohello cluster and run hello example directly on hello head node.</span></span>

1. <span data-ttu-id="4af06-184">tooupload hello jar toohello kluster, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4af06-184">tooupload hello jar toohello cluster, use hello following command:</span></span>

    ```bash
    scp ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:hbaseapp-1.0-SNAPSHOT.jar
    ```

    <span data-ttu-id="4af06-185">Ersätt `USERNAME` med hello namnet för SSH-inloggning.</span><span class="sxs-lookup"><span data-stu-id="4af06-185">Replace `USERNAME` with hello name of your SSH login.</span></span> <span data-ttu-id="4af06-186">Ersätt `CLUSTERNAME` med ditt HDInsight-klustrets namn.</span><span class="sxs-lookup"><span data-stu-id="4af06-186">Replace `CLUSTERNAME` with your HDInsight cluster name.</span></span>

2. <span data-ttu-id="4af06-187">tooconnect toohello HBase-kluster använder hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4af06-187">tooconnect toohello HBase cluster, use hello following command:</span></span>

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="4af06-188">Ersätt `USERNAME` hello namnet på SSH-inloggning.</span><span class="sxs-lookup"><span data-stu-id="4af06-188">Replace `USERNAME` hello name of your SSH login.</span></span> <span data-ttu-id="4af06-189">Ersätt `CLUSTERNAME` med ditt HDInsight-klustrets namn.</span><span class="sxs-lookup"><span data-stu-id="4af06-189">Replace `CLUSTERNAME` with your HDInsight cluster name.</span></span>

3. <span data-ttu-id="4af06-190">en HBase-tabell med toocreate hello Java-program, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4af06-190">toocreate an HBase table using hello Java application, use hello following command:</span></span>

    ```bash
    yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.CreateTable
    ```

    <span data-ttu-id="4af06-191">Det här kommandot skapar en HBase-tabell med namnet **personer**, och fylls med data.</span><span class="sxs-lookup"><span data-stu-id="4af06-191">This command creates a HBase table named **people**, and populates it with data.</span></span>

4. <span data-ttu-id="4af06-192">toosearch för e-postadresser i hello tabell, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4af06-192">toosearch for email addresses stored in hello table, use hello following command:</span></span>

    ```bash
    yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.SearchByEmail contoso.com
    ```

    <span data-ttu-id="4af06-193">Du får hello följande resultat:</span><span class="sxs-lookup"><span data-stu-id="4af06-193">You receive hello following results:</span></span>

        Franklin Holtz - ID: 2
        Franklin Holtz - franklin@contoso.com - ID: 2
        Rae Schroeder - ID: 4
        Rae Schroeder - rae@contoso.com - ID: 4
        Gabriela Ingram - ID: 6
        Gabriela Ingram - gabriela@contoso.com - ID: 6

5. <span data-ttu-id="4af06-194">toodelete hello tabell, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4af06-194">toodelete hello table, use hello following command:</span></span>

    

## <a name="upload-hello-jar-and-run-jobs-powershell"></a><span data-ttu-id="4af06-195">Överför hello JAR och köra jobb (PowerShell)</span><span class="sxs-lookup"><span data-stu-id="4af06-195">Upload hello JAR and run jobs (PowerShell)</span></span>

<span data-ttu-id="4af06-196">hello följande steg kan du använda Azure PowerShell tooupload hello JAR toohello standardlagring för HBase-kluster.</span><span class="sxs-lookup"><span data-stu-id="4af06-196">hello following steps use Azure PowerShell tooupload hello JAR toohello default storage for your HBase cluster.</span></span> <span data-ttu-id="4af06-197">HDInsight-cmdlets är och sedan använda toorun hello exempel via fjärranslutning.</span><span class="sxs-lookup"><span data-stu-id="4af06-197">HDInsight cmdlets are then used toorun hello examples remotely.</span></span>

1. <span data-ttu-id="4af06-198">Efter att installera och konfigurera Azure PowerShell kan du skapa en fil med namnet `hbase-runner.psm1`.</span><span class="sxs-lookup"><span data-stu-id="4af06-198">After installing and configuring Azure PowerShell, create a file named `hbase-runner.psm1`.</span></span> <span data-ttu-id="4af06-199">Använd hello följande text som hello innehållet i den här filen:</span><span class="sxs-lookup"><span data-stu-id="4af06-199">Use hello following text as hello contents of this file:</span></span>

   ```powershell
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
   ```

    <span data-ttu-id="4af06-200">Denna fil innehåller två moduler:</span><span class="sxs-lookup"><span data-stu-id="4af06-200">This file contains two modules:</span></span>

   * <span data-ttu-id="4af06-201">**Lägg till HDInsightFile** – används tooupload filer toohello kluster</span><span class="sxs-lookup"><span data-stu-id="4af06-201">**Add-HDInsightFile** - used tooupload files toohello cluster</span></span>
   * <span data-ttu-id="4af06-202">**Start-HBaseExample** -används toorun hello klasser som skapats tidigare</span><span class="sxs-lookup"><span data-stu-id="4af06-202">**Start-HBaseExample** - used toorun hello classes created earlier</span></span>

2. <span data-ttu-id="4af06-203">Spara hello `hbase-runner.psm1` fil.</span><span class="sxs-lookup"><span data-stu-id="4af06-203">Save hello `hbase-runner.psm1` file.</span></span>

3. <span data-ttu-id="4af06-204">Öppna ett nytt Azure PowerShell-fönster, ändra kataloger toohello `hbaseapp` directory, och sedan kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4af06-204">Open a new Azure PowerShell window, change directories toohello `hbaseapp` directory, and then run hello following command:</span></span>

    ```powershell
    PS C:\ Import-Module c:\path\to\hbase-runner.psm1
    ```

    <span data-ttu-id="4af06-205">Ändra hello toohello sökvägen för hello `hbase-runner.psm1` filen som skapades tidigare.</span><span class="sxs-lookup"><span data-stu-id="4af06-205">Change hello path toohello location of hello `hbase-runner.psm1` file created earlier.</span></span> <span data-ttu-id="4af06-206">Detta kommando registrerar hello modulen med Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4af06-206">This command registers hello module with Azure PowerShell.</span></span>

4. <span data-ttu-id="4af06-207">Använd hello följande kommando tooupload hello `hbaseapp-1.0-SNAPSHOT.jar` tooyour klustret.</span><span class="sxs-lookup"><span data-stu-id="4af06-207">Use hello following command tooupload hello `hbaseapp-1.0-SNAPSHOT.jar` tooyour cluster.</span></span>

    ```powershell
    Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername
    ```

    <span data-ttu-id="4af06-208">Ersätt `hdinsightclustername` med hello namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="4af06-208">Replace `hdinsightclustername` with hello name of your cluster.</span></span> <span data-ttu-id="4af06-209">hello kommandot överför hello `hbaseapp-1.0-SNAPSHOT.jar` toohello `example/jars` plats i hello primära lagring för klustret.</span><span class="sxs-lookup"><span data-stu-id="4af06-209">hello command uploads hello `hbaseapp-1.0-SNAPSHOT.jar` toohello `example/jars` location in hello primary storage for your cluster.</span></span>

5. <span data-ttu-id="4af06-210">en tabell med hjälp av toocreate hello `hbaseapp`, använda hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4af06-210">toocreate a table using hello `hbaseapp`, use hello following command:</span></span>

    ```powershell
    Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername
    ```

    <span data-ttu-id="4af06-211">Ersätt `hdinsightclustername` med hello namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="4af06-211">Replace `hdinsightclustername` with hello name of your cluster.</span></span>

    <span data-ttu-id="4af06-212">Det här kommandot skapar en tabell med namnet **personer** i HBase på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="4af06-212">This command creates a table named **people** in HBase on your HDInsight cluster.</span></span> <span data-ttu-id="4af06-213">Det här kommandot visar inte några utdata i hello konsolfönstret.</span><span class="sxs-lookup"><span data-stu-id="4af06-213">This command does not show any output in hello console window.</span></span>

6. <span data-ttu-id="4af06-214">toosearch för poster i hello tabell, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4af06-214">toosearch for entries in hello table, use hello following command:</span></span>

    ```powershell
    Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com
    ```

    <span data-ttu-id="4af06-215">Ersätt `hdinsightclustername` med hello namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="4af06-215">Replace `hdinsightclustername` with hello name of your cluster.</span></span>

    <span data-ttu-id="4af06-216">Detta kommando använder hello `SearchByEmail` klassen toosearch för alla rader där hello `contactinformation` kolumnfamilj och hello `email` kolumnen innehåller hello sträng `contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="4af06-216">This command uses hello `SearchByEmail` class toosearch for any rows where hello `contactinformation` column family and hello `email` column, contains hello string `contoso.com`.</span></span> <span data-ttu-id="4af06-217">Du bör få hello följande resultat:</span><span class="sxs-lookup"><span data-stu-id="4af06-217">You should receive hello following results:</span></span>

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    <span data-ttu-id="4af06-218">Med hjälp av **fabrikam.com** för hello `-emailRegex` värde returnerar hello-användare som har **fabrikam.com** hello e-fältet.</span><span class="sxs-lookup"><span data-stu-id="4af06-218">Using **fabrikam.com** for hello `-emailRegex` value returns hello users that have **fabrikam.com** in hello email field.</span></span> <span data-ttu-id="4af06-219">Du kan också använda reguljära uttryck som hello sökterm.</span><span class="sxs-lookup"><span data-stu-id="4af06-219">You can also use regular expressions as hello search term.</span></span> <span data-ttu-id="4af06-220">Till exempel **^ r** Returnerar e-postadresser som börjar med bokstaven hello ”r”.</span><span class="sxs-lookup"><span data-stu-id="4af06-220">For example, **^r** returns email addresses that begin with hello letter 'r'.</span></span>

### <a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a><span data-ttu-id="4af06-221">Inga resultat eller oväntade resultat när du använder Start HBaseExample</span><span class="sxs-lookup"><span data-stu-id="4af06-221">No results or unexpected results when using Start-HBaseExample</span></span>

<span data-ttu-id="4af06-222">Använd hello `-showErr` parametern tooview hello standardfel (STDERR) som skapas när hello jobb som körs.</span><span class="sxs-lookup"><span data-stu-id="4af06-222">Use hello `-showErr` parameter tooview hello standard error (STDERR) that is produced while running hello job.</span></span>

## <a name="delete-hello-table"></a><span data-ttu-id="4af06-223">Ta bort hello tabell</span><span class="sxs-lookup"><span data-stu-id="4af06-223">Delete hello table</span></span>

<span data-ttu-id="4af06-224">När du är klar med hello exempel använder följande toodelete hello hello **personer** tabell som används i det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="4af06-224">When you are done with hello example, use hello following toodelete hello **people** table used in this example:</span></span>

<span data-ttu-id="4af06-225">__Från en `ssh` session__:</span><span class="sxs-lookup"><span data-stu-id="4af06-225">__From an `ssh` session__:</span></span>

`yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.DeleteTable`

<span data-ttu-id="4af06-226">__Från Azure PowerShell__:</span><span class="sxs-lookup"><span data-stu-id="4af06-226">__From Azure PowerShell__:</span></span>

`Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername`

## <a name="next-steps"></a><span data-ttu-id="4af06-227">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4af06-227">Next steps</span></span>

[<span data-ttu-id="4af06-228">Lär dig hur toouse SQL SQuirreL med HBase</span><span class="sxs-lookup"><span data-stu-id="4af06-228">Learn how toouse SQuirreL SQL with HBase</span></span>](hdinsight-hbase-phoenix-squirrel-linux.md)
