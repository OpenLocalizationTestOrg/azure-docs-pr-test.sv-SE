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
# <a name="use-maven-toobuild-java-applications-that-use-hbase-with-windows-based-hdinsight-hadoop"></a>Använd Maven toobuild Java-program som använder HBase med Windows-baserade HDInsight (Hadoop)
Lär dig hur toocreate och skapa en [Apache HBase](http://hbase.apache.org/) program i Java med hjälp av Apache Maven. Använd sedan hello program med Azure HDInsight (Hadoop).

[Maven](http://maven.apache.org/) är ett projekt hantering och förståelse programverktyg som gör att du toobuild programvara, dokumentation och rapporter för Java-projekt. I den här artikeln får du lära dig hur toouse den toocreate en grundläggande Java-program som som skapar, frågor och tar bort en HBase tabell i ett Azure HDInsight-kluster.

> [!IMPORTANT]
> hello stegen i det här dokumentet kräver ett HDInsight-kluster som använder Windows. Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="requirements"></a>Krav
* [Java platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 eller senare
* [Maven 3.](http://maven.apache.org/)
* Ett Windows-baserade HDInsight-kluster med HBase

    > [!NOTE]
    > hello stegen i det här dokumentet har testats med HDInsight-kluster version 3.2 och 3.3. hello standardvärden som i exemplen är för ett 3.3 HDInsight-kluster.

## <a name="create-hello-project"></a>Skapa hello-projekt
1. Ändra kataloger toohello plats där du vill toocreate hello projekt, till exempel från kommandoraden i din utvecklingsmiljö hello `cd code\hdinsight`.
2. Använd hello **mvn** kommandot, som installeras med Maven toogenerate hello scaffold-teknik för hello-projekt.

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Det här kommandot skapar en katalog på hello aktuella plats, med hello namn anges av hello **artefakt-ID** parameter (**hbaseapp** i det här exemplet.) Den här katalogen innehåller hello följande objekt:

   * **pom.XML**: hello projektet Object Model ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) innehåller information och konfiguration information används toobuild hello-projekt.
   * **SRC**: hello-katalog som innehåller hello **main\java\com\microsoft\examples** directory, där du ska skapa hello program.
3. Ta bort hello **src\test\java\com\microsoft\examples\apptest.java** filen eftersom den inte används i det här exemplet.

## <a name="update-hello-project-object-model"></a>Uppdatera hello projekt-objektmodell
1. Redigera hello **pom.xml** och Lägg till följande kod i hello hello `<dependencies>` avsnitt:

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>

    Det här avsnittet visar Maven som hello projektet kräver **hbase-klient** version **1.1.2**. Vid kompileringen hämtas detta beroende från hello standarddatabas Maven. Du kan använda hello [Maven centrala lagringsplatsen Sök](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) toolearn mer om detta beroende.

   > [!IMPORTANT]
   > hello versionsnumret måste matcha hello version av HBase som ingår i ditt HDInsight-kluster. Använd följande tabell toofind hello rätt versionsnummer hello.
   >
   >

   | HDInsight-kluster av version | HBase version toouse |
   | --- | --- |
   | 3.2 |0.98.4-hadoop2 |
   | 3.3 |1.1.2 |

    Mer information om HDInsight-versioner och komponenter finns [vad är hello olika Hadoop-komponenter tillgänglig med HDInsight](hdinsight-component-versioning.md).
2. Om du använder ett 3.3 HDInsight-kluster måste du också lägga till följande toohello hello `<dependencies>` avsnitt:

        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>

    Detta beroende läses hello phoenix-grundläggande komponenter som används av Hbase version 1.1.x.
3. Lägg till följande kod toohello hello **pom.xml** fil. Det här avsnittet måste finnas i hello `<project>...</project>` taggar i hello-fil, till exempel mellan `</dependencies>` och `</project>`.

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

    Hej `<resources>` avsnittet konfigureras en resurs (**conf\hbase site.xml**) som innehåller konfigurationsinformation för HBase.

   > [!NOTE]
   > Du kan också ange konfigurationsvärden via kod. Hello kommentarer i hello **CreateTable** exemplet som följer hur toodo detta.
   >
   >

    Detta `<plugins>` avsnittet konfigureras hello [Maven-kompilatorn Plugin](http://maven.apache.org/plugins/maven-compiler-plugin/) och [Maven skugga Plugin](http://maven.apache.org/plugins/maven-shade-plugin/). hello kompilatorn plugin-programmet är används toocompile hello-topologi. hello skugga plugin-programmet är dubbletter av används tooprevent licens hello JAR-paketet som skapas genom Maven. hello beror används på att hello dubbla licensfiler orsakar ett fel vid körning på hello HDInsight-kluster. Med hjälp av maven-skugga-plugin-program med hello `ApacheLicenseResourceTransformer` implementering förhindrar det här felet.

    Hej maven-skugga-plugin-programmet ger också en uber jar (eller fat jar) som innehåller alla hello beroenden som krävs av hello program.
4. Spara hello **pom.xml** fil.
5. Skapa en ny katalog med namnet **conf** i hello **hbaseapp** directory. I hello **conf** directory, skapa en fil med namnet **hbase-site.xml**. Använd följande hello som hello innehållet i hello-fil:

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

    Den här filen kommer att använda tooload hello HBase konfiguration för ett HDInsight-kluster.

   > [!NOTE]
   > Detta är en minimal hbase-site.xml-fil och den innehåller hello utan minimikrav för hello HDInsight-kluster.

6. Spara hello **hbase-site.xml** fil.

## <a name="create-hello-application"></a>Skapa hello program
1. Gå toohello **hbaseapp\src\main\java\com\microsoft\examples** katalog och Byt namn på hello app.java filen för**CreateTable.java**.
2. Öppna hello **CreateTable.java** filen och ersätter hello befintligt innehåll med hello följande kod:

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

    Detta är hello **CreateTable** -klassen, som skapar en tabell med namnet **personer** och fylla det med vissa fördefinierade användare.
3. Spara hello **CreateTable.java** fil.
4. I hello **hbaseapp\src\main\java\com\microsoft\examples** directory, skapa en ny fil med namnet **SearchByEmail.java**. Använd hello efter koden som hello innehållet i den här filen:

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

    Hej **SearchByEmail** klassen kan vara används tooquery för rader av e-postadress. Eftersom den använder ett reguljärt uttryck för filter kan du ange en sträng eller ett reguljärt uttryck när hello-klassen.
5. Spara hello **SearchByEmail.java** fil.
6. I hello **hbaseapp\src\main\hava\com\microsoft\examples** directory, skapa en ny fil med namnet **DeleteTable.java**. Använd hello efter koden som hello innehållet i den här filen:

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

    Den här klassen är för att rensa det här exemplet genom att inaktivera och släppa hello tabell skapas av hello **CreateTable** klass.
7. Spara hello **DeleteTable.java** fil.

## <a name="build-and-package-hello-application"></a>Bygg- och paketet hello-program
1. Öppna en kommandotolk och ändra kataloger toohello **hbaseapp** directory.
2. Använd följande kommando toobuild JAR-filen som innehåller programmet hello hello:

        mvn clean package

    Detta rensar alla tidigare build-artefakter, hämtas eventuella beroenden som inte redan har installerats, sedan bygger och programmet hello-paket.
3. När hello-kommandot har slutförts hello **hbaseapp\target** katalogen innehåller en fil med namnet **hbaseapp-1.0-SNAPSHOT.jar**.

   > [!NOTE]
   > Hej **hbaseapp-1.0-SNAPSHOT.jar** filen är en uber jar (kallas ibland för en fat jar) som innehåller alla hello beroenden krävs toorun hello program.

## <a name="upload-hello-jar-file-and-start-a-job"></a>Överför hello JAR-filen och starta ett jobb
Det finns många sätt tooupload ett filen tooyour HDInsight-kluster, enligt beskrivningen i [överföra data för Hadoop-jobb i HDInsight](hdinsight-upload-data.md). hello använda följande Azure PowerShell.

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

1. Efter att installera och konfigurera Azure PowerShell, skapa en ny fil med namnet **hbase runner.psm1**. Använd följande hello som hello innehållet i den här filen:

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

    Denna fil innehåller två moduler:

   * **Lägg till HDInsightFile** -används tooupload filer tooHDInsight
   * **Start-HBaseExample** -används toorun hello klasser som skapats tidigare
2. Spara hello **hbase runner.psm1** fil.
3. Öppna ett nytt Azure PowerShell-fönster, ändra kataloger toohello **hbaseapp** directory, och sedan kör hello följande kommando.

        PS C:\ Import-Module c:\path\to\hbase-runner.psm1

    Ändra hello toohello sökvägen för hello **hbase runner.psm1** filen som skapades tidigare. Detta registrerar hello-modulen för den här Azure PowerShell-sessionen.
4. Använd hello följande kommando tooupload hello **hbaseapp-1.0-SNAPSHOT.jar** tooyour HDInsight-kluster.

        Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername

    Ersätt **hdinsightclustername** med hello namnet på ditt HDInsight-kluster. hello kommandot överför hello **hbaseapp-1.0-SNAPSHOT.jar** toohello **exempel/burkar** plats i hello primära lagringsplatsen för HDInsight-kluster.
5. När hello filer överförs används hello följande kod toocreate en tabell med hello **hbaseapp**:

        Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername

    Ersätt **hdinsightclustername** med hello namnet på ditt HDInsight-kluster.

    Det här kommandot skapar en ny tabell med namnet **personer** i ditt HDInsight-kluster. Det här kommandot visar inte några utdata i hello konsolfönstret.
6. toosearch för poster i hello tabell, Använd hello följande kommando:

        Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com

    Ersätt **hdinsightclustername** med hello namnet på ditt HDInsight-kluster.

    Detta kommando använder hello **SearchByEmail** klassen toosearch för alla rader där hello **contactinformation** kolumnfamilj och hello **e-post** kolumnen innehåller hello sträng **contoso.com**. Du bör få hello följande resultat:

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    Med hjälp av **fabrikam.com** för hello `-emailRegex` värde returnerar hello-användare som har **fabrikam.com** hello e-fältet. Eftersom den här sökningen implementeras med hjälp av ett reguljärt uttryck baserat filter, du kan också ange reguljära uttryck som **^ r**, vilka returnerar poster där hello e-post som börjar med hello bokstaven ”r”.

## <a name="delete-hello-table"></a>Ta bort hello tabell
När du är klar med hello exemplet, Använd hello följande kommando från hello Azure PowerShell-session toodelete hello **personer** tabell som används i det här exemplet:

    Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername

Ersätt **hdinsightclustername** med hello namnet på ditt HDInsight-kluster.

## <a name="troubleshooting"></a>Felsökning
### <a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a>Inga resultat eller oväntade resultat när du använder Start HBaseExample
Använd hello `-showErr` parametern tooview hello standardfel (STDERR) som skapas när hello jobb som körs.
