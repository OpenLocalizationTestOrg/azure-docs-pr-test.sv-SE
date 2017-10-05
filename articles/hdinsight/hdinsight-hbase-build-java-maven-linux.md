---
title: Java-HBase-client - Azure HDInsight | Microsoft Docs
description: "Lär dig hur du använder Apache Maven för att skapa ett Java-baserad Apache HBase-program och sedan distribuera den till HBase på Azure HDInsight."
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
ms.openlocfilehash: 03c88397e36c0fc7f19410e49f6b6f1a607659f8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="build-java-applications-for-apache-hbase"></a><span data-ttu-id="feed3-103">Skapa Java-program för Apache HBase</span><span class="sxs-lookup"><span data-stu-id="feed3-103">Build Java applications for Apache HBase</span></span>

<span data-ttu-id="feed3-104">Lär dig hur du skapar en [Apache HBase](http://hbase.apache.org/) program i Java.</span><span class="sxs-lookup"><span data-stu-id="feed3-104">Learn how to create an [Apache HBase](http://hbase.apache.org/) application in Java.</span></span> <span data-ttu-id="feed3-105">Använd sedan programmet med HBase på Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="feed3-105">Then use the application with HBase on Azure HDInsight.</span></span>

<span data-ttu-id="feed3-106">Stegen i det här dokumentet används [Maven](http://maven.apache.org/) att skapa och skapa projektet.</span><span class="sxs-lookup"><span data-stu-id="feed3-106">The steps in this document use [Maven](http://maven.apache.org/) to create and build the project.</span></span> <span data-ttu-id="feed3-107">Maven är en programvara projekthantering och förståelse verktyg som du kan skapa programvara, dokumentation och rapporter för Java-projekt.</span><span class="sxs-lookup"><span data-stu-id="feed3-107">Maven is a software project management and comprehension tool that allows you to build software, documentation, and reports for Java projects.</span></span>

> [!NOTE]
> <span data-ttu-id="feed3-108">Stegen i det här dokumentet har senast har testats med HDInsight 3,6.</span><span class="sxs-lookup"><span data-stu-id="feed3-108">The steps in this document were most recently tested with HDInsight 3.6.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="feed3-109">Stegen i det här dokumentet kräver ett HDInsight-kluster som använder Linux.</span><span class="sxs-lookup"><span data-stu-id="feed3-109">The steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="feed3-110">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="feed3-110">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="feed3-111">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="feed3-111">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="requirements"></a><span data-ttu-id="feed3-112">Krav</span><span class="sxs-lookup"><span data-stu-id="feed3-112">Requirements</span></span>

* <span data-ttu-id="feed3-113">[Java platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 8 eller senare.</span><span class="sxs-lookup"><span data-stu-id="feed3-113">[Java platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 8 or later.</span></span>

    > [!NOTE]
    > <span data-ttu-id="feed3-114">HDInsight 3.5 och större kräver Java 8.</span><span class="sxs-lookup"><span data-stu-id="feed3-114">HDInsight 3.5 and greater requires Java 8.</span></span> <span data-ttu-id="feed3-115">Tidigare versioner av HDInsight kräver Java 7.</span><span class="sxs-lookup"><span data-stu-id="feed3-115">Earlier versions of HDInsight require Java 7.</span></span>

* [<span data-ttu-id="feed3-116">Maven 3.</span><span class="sxs-lookup"><span data-stu-id="feed3-116">Maven</span></span>](http://maven.apache.org/)

* [<span data-ttu-id="feed3-117">Ett Linux-baserade Azure HDInsight-kluster med HBase</span><span class="sxs-lookup"><span data-stu-id="feed3-117">A Linux-based Azure HDInsight cluster with HBase</span></span>](hdinsight-hbase-tutorial-get-started-linux.md#create-hbase-cluster)

  > [!NOTE]
  > <span data-ttu-id="feed3-118">Stegen i det här dokumentet har testats med HDInsight-kluster version 3.4 och 3.5.</span><span class="sxs-lookup"><span data-stu-id="feed3-118">The steps in this document have been tested with HDInsight cluster versions 3.4 and 3.5.</span></span> <span data-ttu-id="feed3-119">De standardvärden som i exemplen är för ett kluster i HDInsight 3.5.</span><span class="sxs-lookup"><span data-stu-id="feed3-119">The default values provided in examples are for a HDInsight 3.5 cluster.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="feed3-120">Skapa projektet</span><span class="sxs-lookup"><span data-stu-id="feed3-120">Create the project</span></span>

1. <span data-ttu-id="feed3-121">Från kommandoraden i din utvecklingsmiljö, ändra kataloger till platsen där du vill skapa projektet, till exempel `cd code\hbase`.</span><span class="sxs-lookup"><span data-stu-id="feed3-121">From the command line in your development environment, change directories to the location where you want to create the project, for example, `cd code\hbase`.</span></span>

2. <span data-ttu-id="feed3-122">Använd den **mvn** kommandot, som installeras med Maven för att generera scaffold-teknik för projektet.</span><span class="sxs-lookup"><span data-stu-id="feed3-122">Use the **mvn** command, which is installed with Maven, to generate the scaffolding for the project.</span></span>

    ```bash
    mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

    > [!NOTE]
    > <span data-ttu-id="feed3-123">Om du använder PowerShell kan du ange den `-D` parametrar inom dubbla citattecken.</span><span class="sxs-lookup"><span data-stu-id="feed3-123">If you are using PowerShell, you must enclose the `-D` parameters in double quotes.</span></span>
    >
    > `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=hbaseapp" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`

    <span data-ttu-id="feed3-124">Det här kommandot skapar en katalog med samma namn som den **artefakt-ID** parameter (**hbaseapp** i det här exemplet.) Den här katalogen innehåller följande:</span><span class="sxs-lookup"><span data-stu-id="feed3-124">This command creates a directory with the same name as the **artifactID** parameter (**hbaseapp** in this example.) This directory contains the following items:</span></span>

   * <span data-ttu-id="feed3-125">**pom.XML**: I projektet Object Model ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) innehåller information och konfiguration information som används för att skapa projektet.</span><span class="sxs-lookup"><span data-stu-id="feed3-125">**pom.xml**:  The Project Object Model ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) contains information and configuration details used to build the project.</span></span>
   * <span data-ttu-id="feed3-126">**SRC**: den katalog som innehåller den **main/java/com/microsoft/exempel** directory, där du skapar programmet.</span><span class="sxs-lookup"><span data-stu-id="feed3-126">**src**: The directory that contains the **main/java/com/microsoft/examples** directory, where you author the application.</span></span>

3. <span data-ttu-id="feed3-127">Ta bort den `src/test/java/com/microsoft/examples/apptest.java` filen.</span><span class="sxs-lookup"><span data-stu-id="feed3-127">Delete the `src/test/java/com/microsoft/examples/apptest.java` file.</span></span> <span data-ttu-id="feed3-128">Det är inte användas i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="feed3-128">It is not be used in this example.</span></span>

## <a name="update-the-project-object-model"></a><span data-ttu-id="feed3-129">Uppdatera projekt-objektmodell</span><span class="sxs-lookup"><span data-stu-id="feed3-129">Update the Project Object Model</span></span>

1. <span data-ttu-id="feed3-130">Redigera den `pom.xml` och Lägg till följande kod inne i `<dependencies>` avsnitt:</span><span class="sxs-lookup"><span data-stu-id="feed3-130">Edit the `pom.xml` file and add the following code inside the `<dependencies>` section:</span></span>

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

    <span data-ttu-id="feed3-131">Det här avsnittet visar att projektet behöver **hbase-klient** och **phoenix kärnor** komponenter.</span><span class="sxs-lookup"><span data-stu-id="feed3-131">This section indicates that the project needs **hbase-client** and **phoenix-core** components.</span></span> <span data-ttu-id="feed3-132">Dessa beroenden hämtas vid kompileringen, från Maven standarddatabas.</span><span class="sxs-lookup"><span data-stu-id="feed3-132">At compile time, these dependencies are downloaded from the default Maven repository.</span></span> <span data-ttu-id="feed3-133">Du kan använda den [Maven centrala lagringsplatsen Sök](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) lära dig mer om detta beroende.</span><span class="sxs-lookup"><span data-stu-id="feed3-133">You can use the [Maven Central Repository Search](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) to learn more about this dependency.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="feed3-134">Versionsnumret för hbase-klient måste matcha versionen av HBase som ingår i ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="feed3-134">The version number of the hbase-client must match the version of HBase that is provided with your HDInsight cluster.</span></span> <span data-ttu-id="feed3-135">Använd följande tabell för att hitta rätt versionsnumret.</span><span class="sxs-lookup"><span data-stu-id="feed3-135">Use the following table to find the correct version number.</span></span>

   | <span data-ttu-id="feed3-136">HDInsight-kluster av version</span><span class="sxs-lookup"><span data-stu-id="feed3-136">HDInsight cluster version</span></span> | <span data-ttu-id="feed3-137">HBase-version som ska användas</span><span class="sxs-lookup"><span data-stu-id="feed3-137">HBase version to use</span></span> |
   | --- | --- |
   | <span data-ttu-id="feed3-138">3.2</span><span class="sxs-lookup"><span data-stu-id="feed3-138">3.2</span></span> |<span data-ttu-id="feed3-139">0.98.4-hadoop2</span><span class="sxs-lookup"><span data-stu-id="feed3-139">0.98.4-hadoop2</span></span> |
   | <span data-ttu-id="feed3-140">3.3, 3.4, 3.5 och 3,6</span><span class="sxs-lookup"><span data-stu-id="feed3-140">3.3, 3.4, 3.5, and 3.6</span></span> |<span data-ttu-id="feed3-141">1.1.2</span><span class="sxs-lookup"><span data-stu-id="feed3-141">1.1.2</span></span> |

    <span data-ttu-id="feed3-142">Mer information om HDInsight-versioner och komponenter finns [vilka är de olika Hadoop-komponenterna med HDInsight](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="feed3-142">For more information on HDInsight versions and components, see [What are the different Hadoop components available with HDInsight](hdinsight-component-versioning.md).</span></span>

3. <span data-ttu-id="feed3-143">Lägg till följande kod i den **pom.xml** fil.</span><span class="sxs-lookup"><span data-stu-id="feed3-143">Add the following code to the **pom.xml** file.</span></span> <span data-ttu-id="feed3-144">Den här texten måste finnas i den `<project>...</project>` taggar i filen, till exempel mellan `</dependencies>` och `</project>`.</span><span class="sxs-lookup"><span data-stu-id="feed3-144">This text must be inside the `<project>...</project>` tags in the file, for example, between `</dependencies>` and `</project>`.</span></span>

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

    <span data-ttu-id="feed3-145">Det här avsnittet konfigureras en resurs (`conf/hbase-site.xml`) som innehåller konfigurationsinformation för HBase.</span><span class="sxs-lookup"><span data-stu-id="feed3-145">This section configures a resource (`conf/hbase-site.xml`) that contains configuration information for HBase.</span></span>

   > [!NOTE]
   > <span data-ttu-id="feed3-146">Du kan också ange konfigurationsvärden via kod.</span><span class="sxs-lookup"><span data-stu-id="feed3-146">You can also set configuration values via code.</span></span> <span data-ttu-id="feed3-147">Visa kommentarerna i den `CreateTable` exempel.</span><span class="sxs-lookup"><span data-stu-id="feed3-147">See the comments in the `CreateTable` example.</span></span>

    <span data-ttu-id="feed3-148">Det här avsnittet konfigureras också på [Maven-kompilatorn Plugin](http://maven.apache.org/plugins/maven-compiler-plugin/) och [Maven skugga Plugin](http://maven.apache.org/plugins/maven-shade-plugin/).</span><span class="sxs-lookup"><span data-stu-id="feed3-148">This section also configures the [Maven Compiler Plugin](http://maven.apache.org/plugins/maven-compiler-plugin/) and [Maven Shade Plugin](http://maven.apache.org/plugins/maven-shade-plugin/).</span></span> <span data-ttu-id="feed3-149">Kompilatorn plugin-programmet används för att kompilera topologin.</span><span class="sxs-lookup"><span data-stu-id="feed3-149">The compiler plug-in is used to compile the topology.</span></span> <span data-ttu-id="feed3-150">Skugga plugin-programmet används för att förhindra licens dubbletter av JAR-paketet som skapas genom Maven.</span><span class="sxs-lookup"><span data-stu-id="feed3-150">The shade plug-in is used to prevent license duplication in the JAR package that is built by Maven.</span></span> <span data-ttu-id="feed3-151">Det här plugin-programmet används för att förhindra att ett ”dubbla licensfiler”-fel vid körning i HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="feed3-151">This plugin is used to prevent a "duplicate license files" error at run time on the HDInsight cluster.</span></span> <span data-ttu-id="feed3-152">Med hjälp av maven-skugga-plugin-programmet med den `ApacheLicenseResourceTransformer` implementering förhindrar felet.</span><span class="sxs-lookup"><span data-stu-id="feed3-152">Using maven-shade-plugin with the `ApacheLicenseResourceTransformer` implementation prevents the error.</span></span>

    <span data-ttu-id="feed3-153">Maven-skugga-plugin-programmet ger också en uber jar som innehåller alla beroenden som krävs för programmet.</span><span class="sxs-lookup"><span data-stu-id="feed3-153">The maven-shade-plugin also produces an uber jar that contains all the dependencies required by the application.</span></span>

4. <span data-ttu-id="feed3-154">Spara den `pom.xml` filen.</span><span class="sxs-lookup"><span data-stu-id="feed3-154">Save the `pom.xml` file.</span></span>

5. <span data-ttu-id="feed3-155">Skapa en katalog med namnet `conf` i den `hbaseapp` directory.</span><span class="sxs-lookup"><span data-stu-id="feed3-155">Create a directory named `conf` in the `hbaseapp` directory.</span></span> <span data-ttu-id="feed3-156">Den här katalogen används för lagring av konfigurationsinformation för att ansluta till HBase.</span><span class="sxs-lookup"><span data-stu-id="feed3-156">This directory is used to hold configuration information for connecting to HBase.</span></span>

6. <span data-ttu-id="feed3-157">Använd följande kommando för att kopiera HBase-konfigurationen från HBase-kluster till det `conf` directory.</span><span class="sxs-lookup"><span data-stu-id="feed3-157">Use the following command to copy the HBase configuration from the HBase cluster to the `conf` directory.</span></span> <span data-ttu-id="feed3-158">Ersätt `USERNAME` med namnet på SSH-inloggning.</span><span class="sxs-lookup"><span data-stu-id="feed3-158">Replace `USERNAME` with the name of your SSH login.</span></span> <span data-ttu-id="feed3-159">Ersätt `CLUSTERNAME` med ditt HDInsight-klustrets namn:</span><span class="sxs-lookup"><span data-stu-id="feed3-159">Replace `CLUSTERNAME` with your HDInsight cluster name:</span></span>

    ```bash
    scp USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml
    ```

   <span data-ttu-id="feed3-160">Mer information om hur du använder `ssh` och `scp`, se [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="feed3-160">For more information on using `ssh` and `scp`, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <a name="create-the-application"></a><span data-ttu-id="feed3-161">Skapa programmet</span><span class="sxs-lookup"><span data-stu-id="feed3-161">Create the application</span></span>

1. <span data-ttu-id="feed3-162">Gå till den `hbaseapp/src/main/java/com/microsoft/examples` katalog och Byt namn på app.java filen till `CreateTable.java`.</span><span class="sxs-lookup"><span data-stu-id="feed3-162">Go to the `hbaseapp/src/main/java/com/microsoft/examples` directory and rename the app.java file to `CreateTable.java`.</span></span>

2. <span data-ttu-id="feed3-163">Öppna den `CreateTable.java` filen och ersätter det befintliga innehållet med följande text:</span><span class="sxs-lookup"><span data-stu-id="feed3-163">Open the `CreateTable.java` file and replace the existing contents with the following text:</span></span>

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

        //Linux-based HDInsight clusters use /hbase-unsecure as the znode parent
        config.set("zookeeper.znode.parent","/hbase-unsecure");

        // create an admin object using the config
        HBaseAdmin admin = new HBaseAdmin(config);

        // create the table...
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

        // Add each person to the table
        //   Use the `name` column family for the name
        //   Use the `contactinfo` column family for the email
        for (int i = 0; i< people.length; i++) {
            Put person = new Put(Bytes.toBytes(people[i][0]));
            person.add(Bytes.toBytes("name"), Bytes.toBytes("first"), Bytes.toBytes(people[i][1]));
            person.add(Bytes.toBytes("name"), Bytes.toBytes("last"), Bytes.toBytes(people[i][2]));
            person.add(Bytes.toBytes("contactinfo"), Bytes.toBytes("email"), Bytes.toBytes(people[i][3]));
            table.put(person);
        }
        // flush commits and close the table
        table.flushCommits();
        table.close();
        }
    }
   ```

    <span data-ttu-id="feed3-164">Den här koden är den **CreateTable** -klassen, som skapar en tabell med namnet **personer** och fylla det med vissa fördefinierade användare.</span><span class="sxs-lookup"><span data-stu-id="feed3-164">This code is the **CreateTable** class, which creates a table named **people** and populate it with some predefined users.</span></span>

3. <span data-ttu-id="feed3-165">Spara den `CreateTable.java` filen.</span><span class="sxs-lookup"><span data-stu-id="feed3-165">Save the `CreateTable.java` file.</span></span>

4. <span data-ttu-id="feed3-166">I den `hbaseapp/src/main/java/com/microsoft/examples` directory, skapa en fil med namnet `SearchByEmail.java`.</span><span class="sxs-lookup"><span data-stu-id="feed3-166">In the `hbaseapp/src/main/java/com/microsoft/examples` directory, create a file named `SearchByEmail.java`.</span></span> <span data-ttu-id="feed3-167">Använd följande text som innehållet i den här filen:</span><span class="sxs-lookup"><span data-stu-id="feed3-167">Use the following text as the contents of this file:</span></span>

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

        // Use GenericOptionsParser to get only the parameters to the class
        // and not all the parameters passed (when using WebHCat for example)
        String[] otherArgs = new GenericOptionsParser(config, args).getRemainingArgs();
        if (otherArgs.length != 1) {
            System.out.println("usage: [regular expression]");
            System.exit(-1);
        }

        // Open the table
        HTable table = new HTable(config, "people");

        // Define the family and qualifiers to be used
        byte[] contactFamily = Bytes.toBytes("contactinfo");
        byte[] emailQualifier = Bytes.toBytes("email");
        byte[] nameFamily = Bytes.toBytes("name");
        byte[] firstNameQualifier = Bytes.toBytes("first");
        byte[] lastNameQualifier = Bytes.toBytes("last");

        // Create a regex filter
        RegexStringComparator emailFilter = new RegexStringComparator(otherArgs[0]);
        // Attach the regex filter to a filter
        //   for the email column
        SingleColumnValueFilter filter = new SingleColumnValueFilter(
            contactFamily,
            emailQualifier,
            CompareOp.EQUAL,
            emailFilter
        );

        // Create a scan and set the filter
        Scan scan = new Scan();
        scan.setFilter(filter);

        // Get the results
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

    <span data-ttu-id="feed3-168">Den **SearchByEmail** klassen kan användas för att söka efter rader av e-postadress.</span><span class="sxs-lookup"><span data-stu-id="feed3-168">The **SearchByEmail** class can be used to query for rows by email address.</span></span> <span data-ttu-id="feed3-169">Eftersom den använder ett reguljärt uttryck för filter kan du ange en sträng eller ett reguljärt uttryck när du använder klassen.</span><span class="sxs-lookup"><span data-stu-id="feed3-169">Because it uses a regular expression filter, you can provide either a string or a regular expression when using the class.</span></span>

5. <span data-ttu-id="feed3-170">Spara den `SearchByEmail.java` filen.</span><span class="sxs-lookup"><span data-stu-id="feed3-170">Save the `SearchByEmail.java` file.</span></span>

6. <span data-ttu-id="feed3-171">I den `hbaseapp/src/main/hava/com/microsoft/examples` directory, skapa en fil med namnet `DeleteTable.java`.</span><span class="sxs-lookup"><span data-stu-id="feed3-171">In the `hbaseapp/src/main/hava/com/microsoft/examples` directory, create a file named `DeleteTable.java`.</span></span> <span data-ttu-id="feed3-172">Använd följande text som innehållet i den här filen:</span><span class="sxs-lookup"><span data-stu-id="feed3-172">Use the following text as the contents of this file:</span></span>

   ```java
    package com.microsoft.examples;
    import java.io.IOException;

    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.hbase.HBaseConfiguration;
    import org.apache.hadoop.hbase.client.HBaseAdmin;

    public class DeleteTable {
        public static void main(String[] args) throws IOException {
        Configuration config = HBaseConfiguration.create();

        // Create an admin object using the config
        HBaseAdmin admin = new HBaseAdmin(config);

        // Disable, and then delete the table
        admin.disableTable("people");
        admin.deleteTable("people");
        }
    }
   ```

    <span data-ttu-id="feed3-173">Den här klassen rensar HBase-tabeller som skapats i det här exemplet genom att inaktivera och släppa tabellen som skapats av den `CreateTable` klass.</span><span class="sxs-lookup"><span data-stu-id="feed3-173">This class cleans up the HBase tables created in this example by disabling and dropping the table created by the `CreateTable` class.</span></span>

7. <span data-ttu-id="feed3-174">Spara den `DeleteTable.java` filen.</span><span class="sxs-lookup"><span data-stu-id="feed3-174">Save the `DeleteTable.java` file.</span></span>

## <a name="build-and-package-the-application"></a><span data-ttu-id="feed3-175">Skapa och paketera programmet</span><span class="sxs-lookup"><span data-stu-id="feed3-175">Build and package the application</span></span>

1. <span data-ttu-id="feed3-176">Från den `hbaseapp` directory, använder du följande kommando för att skapa en JAR-fil som innehåller programmet:</span><span class="sxs-lookup"><span data-stu-id="feed3-176">From the `hbaseapp` directory, use the following command to build a JAR file that contains the application:</span></span>

    ```bash
    mvn clean package
    ```

    <span data-ttu-id="feed3-177">Det här kommandot skapar och paketerar programmet till en .jar-fil.</span><span class="sxs-lookup"><span data-stu-id="feed3-177">This command builds and packages the application into a .jar file.</span></span>

2. <span data-ttu-id="feed3-178">När kommandot har slutförts i `hbaseapp/target` katalogen innehåller en fil med namnet `hbaseapp-1.0-SNAPSHOT.jar`.</span><span class="sxs-lookup"><span data-stu-id="feed3-178">When the command completes, the `hbaseapp/target` directory contains a file named `hbaseapp-1.0-SNAPSHOT.jar`.</span></span>

   > [!NOTE]
   > <span data-ttu-id="feed3-179">Den `hbaseapp-1.0-SNAPSHOT.jar` filen är en uber jar.</span><span class="sxs-lookup"><span data-stu-id="feed3-179">The `hbaseapp-1.0-SNAPSHOT.jar` file is an uber jar.</span></span> <span data-ttu-id="feed3-180">Den innehåller alla beroenden som krävs för att köra programmet.</span><span class="sxs-lookup"><span data-stu-id="feed3-180">It contains all the dependencies required to run the application.</span></span>


## <a name="upload-the-jar-and-run-jobs-ssh"></a><span data-ttu-id="feed3-181">Överför JAR och köra jobb (SSH)</span><span class="sxs-lookup"><span data-stu-id="feed3-181">Upload the JAR and run jobs (SSH)</span></span>

<span data-ttu-id="feed3-182">Följande steg används `scp` att kopiera JAR till primära huvudnod din HBase på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="feed3-182">The following steps use `scp` to copy the JAR to the primary head node of your HBase on HDInsight cluster.</span></span> <span data-ttu-id="feed3-183">Den `ssh` kommandot används sedan för att ansluta till klustret och köra exemplet direkt på huvudnoden.</span><span class="sxs-lookup"><span data-stu-id="feed3-183">The `ssh` command is then used to connect to the cluster and run the example directly on the head node.</span></span>

1. <span data-ttu-id="feed3-184">Om du vill överföra jar klustret, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="feed3-184">To upload the jar to the cluster, use the following command:</span></span>

    ```bash
    scp ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:hbaseapp-1.0-SNAPSHOT.jar
    ```

    <span data-ttu-id="feed3-185">Ersätt `USERNAME` med namnet på SSH-inloggning.</span><span class="sxs-lookup"><span data-stu-id="feed3-185">Replace `USERNAME` with the name of your SSH login.</span></span> <span data-ttu-id="feed3-186">Ersätt `CLUSTERNAME` med ditt HDInsight-klustrets namn.</span><span class="sxs-lookup"><span data-stu-id="feed3-186">Replace `CLUSTERNAME` with your HDInsight cluster name.</span></span>

2. <span data-ttu-id="feed3-187">För att ansluta till HBase-kluster använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="feed3-187">To connect to the HBase cluster, use the following command:</span></span>

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="feed3-188">Ersätt `USERNAME` namnet på SSH-inloggning.</span><span class="sxs-lookup"><span data-stu-id="feed3-188">Replace `USERNAME` the name of your SSH login.</span></span> <span data-ttu-id="feed3-189">Ersätt `CLUSTERNAME` med ditt HDInsight-klustrets namn.</span><span class="sxs-lookup"><span data-stu-id="feed3-189">Replace `CLUSTERNAME` with your HDInsight cluster name.</span></span>

3. <span data-ttu-id="feed3-190">Om du vill skapa en HBase-tabell med Java-program, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="feed3-190">To create an HBase table using the Java application, use the following command:</span></span>

    ```bash
    yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.CreateTable
    ```

    <span data-ttu-id="feed3-191">Det här kommandot skapar en HBase-tabell med namnet **personer**, och fylls med data.</span><span class="sxs-lookup"><span data-stu-id="feed3-191">This command creates a HBase table named **people**, and populates it with data.</span></span>

4. <span data-ttu-id="feed3-192">Om du vill söka efter e-postadresser som lagras i tabellen, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="feed3-192">To search for email addresses stored in the table, use the following command:</span></span>

    ```bash
    yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.SearchByEmail contoso.com
    ```

    <span data-ttu-id="feed3-193">Du får följande resultat:</span><span class="sxs-lookup"><span data-stu-id="feed3-193">You receive the following results:</span></span>

        Franklin Holtz - ID: 2
        Franklin Holtz - franklin@contoso.com - ID: 2
        Rae Schroeder - ID: 4
        Rae Schroeder - rae@contoso.com - ID: 4
        Gabriela Ingram - ID: 6
        Gabriela Ingram - gabriela@contoso.com - ID: 6

5. <span data-ttu-id="feed3-194">Om du vill ta bort tabellen använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="feed3-194">To delete the table, use the following command:</span></span>

    

## <a name="upload-the-jar-and-run-jobs-powershell"></a><span data-ttu-id="feed3-195">Överför JAR och köra jobb (PowerShell)</span><span class="sxs-lookup"><span data-stu-id="feed3-195">Upload the JAR and run jobs (PowerShell)</span></span>

<span data-ttu-id="feed3-196">Följande steg använda Azure PowerShell för att överföra JAR till standardlagring för HBase-kluster.</span><span class="sxs-lookup"><span data-stu-id="feed3-196">The following steps use Azure PowerShell to upload the JAR to the default storage for your HBase cluster.</span></span> <span data-ttu-id="feed3-197">HDInsight-cmdletarna används sedan för att köra exemplen via fjärranslutning.</span><span class="sxs-lookup"><span data-stu-id="feed3-197">HDInsight cmdlets are then used to run the examples remotely.</span></span>

1. <span data-ttu-id="feed3-198">Efter att installera och konfigurera Azure PowerShell kan du skapa en fil med namnet `hbase-runner.psm1`.</span><span class="sxs-lookup"><span data-stu-id="feed3-198">After installing and configuring Azure PowerShell, create a file named `hbase-runner.psm1`.</span></span> <span data-ttu-id="feed3-199">Använd följande text som innehållet i den här filen:</span><span class="sxs-lookup"><span data-stu-id="feed3-199">Use the following text as the contents of this file:</span></span>

   ```powershell
    <#
    .SYNOPSIS
    Copies a file to the primary storage of an HDInsight cluster.
    .DESCRIPTION
    Copies a file from a local directory to the blob container for
    the HDInsight cluster.
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
    #The class to run
    [Parameter(Mandatory = $true)]
    [String]$className,

    #The name of the HDInsight cluster
    [Parameter(Mandatory = $true)]
    [String]$clusterName,

    #Only used when using SearchByEmail
    [Parameter(Mandatory = $false)]
    [String]$emailRegex,

    #Use if you want to see stderr output
    [Parameter(Mandatory = $false)]
    [Switch]$showErr
    )

    Set-StrictMode -Version 3

    # Is the Azure module installed?
    FindAzure

    # Get the login for the HDInsight cluster
    $creds=Get-Credential -Message "Enter the login for the cluster" -UserName "admin"

    # The JAR
    $jarFile = "wasb:///example/jars/hbaseapp-1.0-SNAPSHOT.jar"

    # The job definition
    $jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
        -JarFile $jarFile `
        -ClassName $className `
        -Arguments $emailRegex

    # Get the job output
    $job = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds
    Write-Host "Wait for the job to complete ..." -ForegroundColor Green
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
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
                -Clustername $clusterName `
                -JobId $job.JobId `
                -HttpCredential $creds
    }

    <#
    .SYNOPSIS
    Copies a file to the primary storage of an HDInsight cluster.
    .DESCRIPTION
    Copies a file from a local directory to the blob container for
    the HDInsight cluster.
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
            #The path to the local file.
            [Parameter(Mandatory = $true)]
            [String]$localPath,

            #The destination path and file name, relative to the root of the container.
            [Parameter(Mandatory = $true)]
            [String]$destinationPath,

            #The name of the HDInsight cluster
            [Parameter(Mandatory = $true)]
            [String]$clusterName,

            #If specified, overwrites existing files without prompting
            [Parameter(Mandatory = $false)]
            [Switch]$force
        )

        Set-StrictMode -Version 3

        # Is the Azure module installed?
        FindAzure

        # Get authentication for the cluster
        $creds=Get-Credential

        # Does the local path exist?
        if (-not (Test-Path $localPath))
        {
            throw "Source path '$localPath' does not exist."
        }

        # Get the primary storage container
        $storage = GetStorage -clusterName $clusterName

        # Upload file to storage, overwriting existing files if -force was used.
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
            throw "No active Azure subscription found! If you have a subscription, use the Login-AzureRmAccount cmdlet to login to your subscription."
        }
    }

    function GetStorage {
        param(
            [Parameter(Mandatory = $true)]
            [String]$clusterName
        )
        $hdi = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        # Does the cluster exist?
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
        # Get the resource group, in case we need that
        $return.resourceGroup = $resourceGroup
        # Get the storage context, as we can't depend
        # on using the default storage context
        $return.context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        # Get the container, so we know where to
        # find/store blobs
        $return.container = $container
        # Return storage accounts to support finding all accounts for
        # a cluster
        $return.storageAccount = $storageAccountName
        $return.storageAccountKey = $storageAccountKey

        return $return
    }
    # Only export the verb-phrase things
    export-modulemember *-*
   ```

    <span data-ttu-id="feed3-200">Denna fil innehåller två moduler:</span><span class="sxs-lookup"><span data-stu-id="feed3-200">This file contains two modules:</span></span>

   * <span data-ttu-id="feed3-201">**Lägg till HDInsightFile** – används för att överföra filer till klustret</span><span class="sxs-lookup"><span data-stu-id="feed3-201">**Add-HDInsightFile** - used to upload files to the cluster</span></span>
   * <span data-ttu-id="feed3-202">**Start-HBaseExample** – används för att köra de klasser som skapades tidigare</span><span class="sxs-lookup"><span data-stu-id="feed3-202">**Start-HBaseExample** - used to run the classes created earlier</span></span>

2. <span data-ttu-id="feed3-203">Spara den `hbase-runner.psm1` filen.</span><span class="sxs-lookup"><span data-stu-id="feed3-203">Save the `hbase-runner.psm1` file.</span></span>

3. <span data-ttu-id="feed3-204">Öppna ett nytt Azure PowerShell-fönster, ändra kataloger till den `hbaseapp` katalog och kör sedan följande kommando:</span><span class="sxs-lookup"><span data-stu-id="feed3-204">Open a new Azure PowerShell window, change directories to the `hbaseapp` directory, and then run the following command:</span></span>

    ```powershell
    PS C:\ Import-Module c:\path\to\hbase-runner.psm1
    ```

    <span data-ttu-id="feed3-205">Ändra sökvägen till platsen för den `hbase-runner.psm1` filen som skapades tidigare.</span><span class="sxs-lookup"><span data-stu-id="feed3-205">Change the path to the location of the `hbase-runner.psm1` file created earlier.</span></span> <span data-ttu-id="feed3-206">Detta kommando registrerar modulen med Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="feed3-206">This command registers the module with Azure PowerShell.</span></span>

4. <span data-ttu-id="feed3-207">Använd följande kommando för att överföra den `hbaseapp-1.0-SNAPSHOT.jar` i klustret.</span><span class="sxs-lookup"><span data-stu-id="feed3-207">Use the following command to upload the `hbaseapp-1.0-SNAPSHOT.jar` to your cluster.</span></span>

    ```powershell
    Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername
    ```

    <span data-ttu-id="feed3-208">Ersätt `hdinsightclustername` med namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="feed3-208">Replace `hdinsightclustername` with the name of your cluster.</span></span> <span data-ttu-id="feed3-209">Kommandot laddar upp den `hbaseapp-1.0-SNAPSHOT.jar` till den `example/jars` plats i den primära lagringsplatsen för klustret.</span><span class="sxs-lookup"><span data-stu-id="feed3-209">The command uploads the `hbaseapp-1.0-SNAPSHOT.jar` to the `example/jars` location in the primary storage for your cluster.</span></span>

5. <span data-ttu-id="feed3-210">Skapa en tabell med hjälp av den `hbaseapp`, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="feed3-210">To create a table using the `hbaseapp`, use the following command:</span></span>

    ```powershell
    Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername
    ```

    <span data-ttu-id="feed3-211">Ersätt `hdinsightclustername` med namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="feed3-211">Replace `hdinsightclustername` with the name of your cluster.</span></span>

    <span data-ttu-id="feed3-212">Det här kommandot skapar en tabell med namnet **personer** i HBase på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="feed3-212">This command creates a table named **people** in HBase on your HDInsight cluster.</span></span> <span data-ttu-id="feed3-213">Det här kommandot visar inte några utdata i konsolfönstret.</span><span class="sxs-lookup"><span data-stu-id="feed3-213">This command does not show any output in the console window.</span></span>

6. <span data-ttu-id="feed3-214">Om du vill söka efter i tabellen, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="feed3-214">To search for entries in the table, use the following command:</span></span>

    ```powershell
    Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com
    ```

    <span data-ttu-id="feed3-215">Ersätt `hdinsightclustername` med namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="feed3-215">Replace `hdinsightclustername` with the name of your cluster.</span></span>

    <span data-ttu-id="feed3-216">Detta kommando använder den `SearchByEmail` klassen för att söka efter alla rader där den `contactinformation` kolumnfamilj och `email` kolumnen innehåller strängen `contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="feed3-216">This command uses the `SearchByEmail` class to search for any rows where the `contactinformation` column family and the `email` column, contains the string `contoso.com`.</span></span> <span data-ttu-id="feed3-217">Du bör få följande resultat:</span><span class="sxs-lookup"><span data-stu-id="feed3-217">You should receive the following results:</span></span>

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    <span data-ttu-id="feed3-218">Med hjälp av **fabrikam.com** för den `-emailRegex` värde returnerar de användare som har **fabrikam.com** i fältet e-post.</span><span class="sxs-lookup"><span data-stu-id="feed3-218">Using **fabrikam.com** for the `-emailRegex` value returns the users that have **fabrikam.com** in the email field.</span></span> <span data-ttu-id="feed3-219">Du kan också använda reguljära uttryck som sökvillkor.</span><span class="sxs-lookup"><span data-stu-id="feed3-219">You can also use regular expressions as the search term.</span></span> <span data-ttu-id="feed3-220">Till exempel **^ r** Returnerar e-postadresser som börjar med bokstaven ”r”.</span><span class="sxs-lookup"><span data-stu-id="feed3-220">For example, **^r** returns email addresses that begin with the letter 'r'.</span></span>

### <a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a><span data-ttu-id="feed3-221">Inga resultat eller oväntade resultat när du använder Start HBaseExample</span><span class="sxs-lookup"><span data-stu-id="feed3-221">No results or unexpected results when using Start-HBaseExample</span></span>

<span data-ttu-id="feed3-222">Använd den `-showErr` parametern för att visa standardfel (STDERR) som skapas när du kör jobbet.</span><span class="sxs-lookup"><span data-stu-id="feed3-222">Use the `-showErr` parameter to view the standard error (STDERR) that is produced while running the job.</span></span>

## <a name="delete-the-table"></a><span data-ttu-id="feed3-223">Ta bort tabellen</span><span class="sxs-lookup"><span data-stu-id="feed3-223">Delete the table</span></span>

<span data-ttu-id="feed3-224">När du är klar med exemplet, Använd följande för att ta bort den **personer** tabell som används i det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="feed3-224">When you are done with the example, use the following to delete the **people** table used in this example:</span></span>

<span data-ttu-id="feed3-225">__Från en `ssh` session__:</span><span class="sxs-lookup"><span data-stu-id="feed3-225">__From an `ssh` session__:</span></span>

`yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.DeleteTable`

<span data-ttu-id="feed3-226">__Från Azure PowerShell__:</span><span class="sxs-lookup"><span data-stu-id="feed3-226">__From Azure PowerShell__:</span></span>

`Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername`

## <a name="next-steps"></a><span data-ttu-id="feed3-227">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="feed3-227">Next steps</span></span>

[<span data-ttu-id="feed3-228">Lär dig hur du använder SQL SQuirreL med HBase</span><span class="sxs-lookup"><span data-stu-id="feed3-228">Learn how to use SQuirreL SQL with HBase</span></span>](hdinsight-hbase-phoenix-squirrel-linux.md)
