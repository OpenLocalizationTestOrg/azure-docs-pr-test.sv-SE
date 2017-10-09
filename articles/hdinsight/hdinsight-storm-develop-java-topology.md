---
title: aaaApache Storm-exempel Java-topologi - Azure HDInsight | Microsoft Docs
description: "Lär dig hur toocreate Apache Storm-topologier i Java genom att skapa ett ord exempel räknar topologi."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
keywords: Apache storm, apache storm exempel storm java, storm-topologi exempel
ms.assetid: a8838f29-9c08-4fd9-99ef-26655d1bf6d7
ms.service: hdinsight
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/07/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive,hdiseo17may2017
ms.openlocfilehash: 54fa9dc3c93ddad83ac861f3101f50f80117d804
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-apache-storm-topology-in-java"></a><span data-ttu-id="a9221-104">Skapa en Apache Storm-topologi i Java</span><span class="sxs-lookup"><span data-stu-id="a9221-104">Create an Apache Storm topology in Java</span></span>

<span data-ttu-id="a9221-105">Lär dig hur toocreate en Java-baserad topologi för Apache Storm.</span><span class="sxs-lookup"><span data-stu-id="a9221-105">Learn how toocreate a Java-based topology for Apache Storm.</span></span> <span data-ttu-id="a9221-106">Du skapar en Storm-topologi som implementerar ett ordräkning program.</span><span class="sxs-lookup"><span data-stu-id="a9221-106">You create a Storm topology that implements a word-count application.</span></span> <span data-ttu-id="a9221-107">Du kan använda Maven toobuild och paketet hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="a9221-107">You use Maven toobuild and package hello project.</span></span> <span data-ttu-id="a9221-108">Sedan kan du lära dig hur toodefine hello topologi med hello som framework.</span><span class="sxs-lookup"><span data-stu-id="a9221-108">Then, you learn how toodefine hello topology using hello Flux framework.</span></span>

> [!NOTE]
> <span data-ttu-id="a9221-109">hello som framework är tillgänglig i Storm 0.10.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="a9221-109">hello Flux framework is available in Storm 0.10.0 or later.</span></span> <span data-ttu-id="a9221-110">Storm 0.10.0 är tillgänglig med HDInsight 3.3 och 3.4.</span><span class="sxs-lookup"><span data-stu-id="a9221-110">Storm 0.10.0 is available with HDInsight 3.3 and 3.4.</span></span>

<span data-ttu-id="a9221-111">När du har slutfört hello stegen i det här dokumentet kan du distribuera hello topologi tooApache Storm på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a9221-111">After completing hello steps in this document, you can deploy hello topology tooApache Storm on HDInsight.</span></span>

> [!NOTE]
> <span data-ttu-id="a9221-112">En fullständig version av hello Storm-topologi exempel som skapats i det här dokumentet finns på [https://github.com/Azure-Samples/hdinsight-java-storm-wordcount](https://github.com/Azure-Samples/hdinsight-java-storm-wordcount).</span><span class="sxs-lookup"><span data-stu-id="a9221-112">A completed version of hello Storm topology examples created in this document is available at [https://github.com/Azure-Samples/hdinsight-java-storm-wordcount](https://github.com/Azure-Samples/hdinsight-java-storm-wordcount).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a9221-113">Krav</span><span class="sxs-lookup"><span data-stu-id="a9221-113">Prerequisites</span></span>

* [<span data-ttu-id="a9221-114">Java Developer Kit (JDK) version 7</span><span class="sxs-lookup"><span data-stu-id="a9221-114">Java Developer Kit (JDK) version 7</span></span>](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)

* <span data-ttu-id="a9221-115">[Maven (https://maven.apache.org/download.cgi)](https://maven.apache.org/download.cgi): Maven är ett projekt build-system för Java-projekt.</span><span class="sxs-lookup"><span data-stu-id="a9221-115">[Maven (https://maven.apache.org/download.cgi)](https://maven.apache.org/download.cgi): Maven is a project build system for Java projects.</span></span>

* <span data-ttu-id="a9221-116">En textredigerare eller IDE.</span><span class="sxs-lookup"><span data-stu-id="a9221-116">A text editor or IDE.</span></span>

## <a name="configure-environment-variables"></a><span data-ttu-id="a9221-117">Konfigurera miljövariabler</span><span class="sxs-lookup"><span data-stu-id="a9221-117">Configure environment variables</span></span>

<span data-ttu-id="a9221-118">hello kan följande miljövariabler anges när du installerar Java och hello JDK.</span><span class="sxs-lookup"><span data-stu-id="a9221-118">hello following environment variables may be set when you install Java and hello JDK.</span></span> <span data-ttu-id="a9221-119">Dock bör du kontrollera att de finns och att de innehåller hello rätt värden för ditt system.</span><span class="sxs-lookup"><span data-stu-id="a9221-119">However, you should check that they exist and that they contain hello correct values for your system.</span></span>

* <span data-ttu-id="a9221-120">**JAVA_HOME** -ska peka toohello katalog där hello Java runtime environment (JRE) har installerats.</span><span class="sxs-lookup"><span data-stu-id="a9221-120">**JAVA_HOME** - should point toohello directory where hello Java runtime environment (JRE) is installed.</span></span> <span data-ttu-id="a9221-121">Till exempel i en Unix- eller Linux-distribution, den måste ha ett värde liknande för`/usr/lib/jvm/java-7-oracle`.</span><span class="sxs-lookup"><span data-stu-id="a9221-121">For example, in a Unix or Linux distribution, it should have a value similar too`/usr/lib/jvm/java-7-oracle`.</span></span> <span data-ttu-id="a9221-122">I Windows, skulle det ha ett värde som är liknande för`c:\Program Files (x86)\Java\jre1.7`</span><span class="sxs-lookup"><span data-stu-id="a9221-122">In Windows, it would have a value similar too`c:\Program Files (x86)\Java\jre1.7`</span></span>

* <span data-ttu-id="a9221-123">**SÖKVÄGEN** -bör innehålla hello följande sökvägar:</span><span class="sxs-lookup"><span data-stu-id="a9221-123">**PATH** - should contain hello following paths:</span></span>

  * <span data-ttu-id="a9221-124">**JAVA_HOME** (eller motsvarande hello-sökväg)</span><span class="sxs-lookup"><span data-stu-id="a9221-124">**JAVA_HOME** (or hello equivalent path)</span></span>

  * <span data-ttu-id="a9221-125">**JAVA_HOME\bin** (eller motsvarande hello-sökväg)</span><span class="sxs-lookup"><span data-stu-id="a9221-125">**JAVA_HOME\bin** (or hello equivalent path)</span></span>

  * <span data-ttu-id="a9221-126">hello katalog där Maven har installerats</span><span class="sxs-lookup"><span data-stu-id="a9221-126">hello directory where Maven is installed</span></span>

## <a name="create-a-maven-project"></a><span data-ttu-id="a9221-127">Skapa ett Maven-projekt</span><span class="sxs-lookup"><span data-stu-id="a9221-127">Create a Maven project</span></span>

<span data-ttu-id="a9221-128">Från kommandoraden hello Använd hello följande kommando toocreate ett Maven-projekt med namnet **WordCount**:</span><span class="sxs-lookup"><span data-stu-id="a9221-128">From hello command line, use hello following command toocreate a Maven project named **WordCount**:</span></span>

```bash
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DgroupId=com.microsoft.example -DartifactId=WordCount -DinteractiveMode=false
```

> [!NOTE]
> <span data-ttu-id="a9221-129">Om du använder PowerShell måste omges av`-D` parametrar med dubbla citattecken.</span><span class="sxs-lookup"><span data-stu-id="a9221-129">If you are using PowerShell, you must surround the`-D` parameters with double quotes.</span></span>
>
> `mvn archetype:generate "-DarchetypeArtifactId=maven-archetype-quickstart" "-DgroupId=com.microsoft.example" "-DartifactId=WordCount" "-DinteractiveMode=false"`

<span data-ttu-id="a9221-130">Det här kommandot skapar en katalog med namnet `WordCount` på hello aktuella plats, som innehåller en grundläggande Maven-projekt.</span><span class="sxs-lookup"><span data-stu-id="a9221-130">This command creates a directory named `WordCount` at hello current location, which contains a basic Maven project.</span></span> <span data-ttu-id="a9221-131">Hej `WordCount` katalogen innehåller hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="a9221-131">hello `WordCount` directory contains hello following items:</span></span>

* <span data-ttu-id="a9221-132">`pom.xml`: Innehåller inställningar för hello Maven-projekt.</span><span class="sxs-lookup"><span data-stu-id="a9221-132">`pom.xml`: Contains settings for hello Maven project.</span></span>
* <span data-ttu-id="a9221-133">`src\main\java\com\microsoft\example`: Innehåller programkoden.</span><span class="sxs-lookup"><span data-stu-id="a9221-133">`src\main\java\com\microsoft\example`: Contains your application code.</span></span>
* <span data-ttu-id="a9221-134">`src\test\java\com\microsoft\example`: Innehåller tester för ditt program.</span><span class="sxs-lookup"><span data-stu-id="a9221-134">`src\test\java\com\microsoft\example`: Contains tests for your application.</span></span> 

### <a name="remove-hello-generated-example-code"></a><span data-ttu-id="a9221-135">Ta bort hello genereras exempelkod</span><span class="sxs-lookup"><span data-stu-id="a9221-135">Remove hello generated example code</span></span>

<span data-ttu-id="a9221-136">Ta bort hello genereras test- och programfiler hello:</span><span class="sxs-lookup"><span data-stu-id="a9221-136">Delete hello generated test and hello application files:</span></span>

* <span data-ttu-id="a9221-137">**src\test\java\com\microsoft\example\AppTest.Java**</span><span class="sxs-lookup"><span data-stu-id="a9221-137">**src\test\java\com\microsoft\example\AppTest.java**</span></span>
* <span data-ttu-id="a9221-138">**src\main\java\com\microsoft\example\App.Java**</span><span class="sxs-lookup"><span data-stu-id="a9221-138">**src\main\java\com\microsoft\example\App.java**</span></span>

## <a name="add-maven-repositories"></a><span data-ttu-id="a9221-139">Lägg till Maven-databaser</span><span class="sxs-lookup"><span data-stu-id="a9221-139">Add Maven repositories</span></span>

<span data-ttu-id="a9221-140">HDInsight är baserad på hello Hortonworks Data Platform (HDP), så vi rekommenderar att du använder hello Hortonworks databasen toodownload beroenden för Apache Storm-projekt.</span><span class="sxs-lookup"><span data-stu-id="a9221-140">HDInsight is based on hello Hortonworks Data Platform (HDP), so we recommend using hello Hortonworks repository toodownload dependencies for your Apache Storm projects.</span></span> <span data-ttu-id="a9221-141">I hello __pom.xml__ lägger du till följande XML efter hello hello `<url>http://maven.apache.org</url>` rad:</span><span class="sxs-lookup"><span data-stu-id="a9221-141">In hello __pom.xml__ file, add hello following XML after hello `<url>http://maven.apache.org</url>` line:</span></span>

```xml
<repositories>
    <repository>
        <releases>
            <enabled>true</enabled>
            <updatePolicy>always</updatePolicy>
            <checksumPolicy>warn</checksumPolicy>
        </releases>
        <snapshots>
            <enabled>false</enabled>
            <updatePolicy>never</updatePolicy>
            <checksumPolicy>fail</checksumPolicy>
        </snapshots>
        <id>HDPReleases</id>
        <name>HDP Releases</name>
        <url>http://repo.hortonworks.com/content/repositories/releases/</url>
        <layout>default</layout>
    </repository>
    <repository>
        <releases>
            <enabled>true</enabled>
            <updatePolicy>always</updatePolicy>
            <checksumPolicy>warn</checksumPolicy>
        </releases>
        <snapshots>
            <enabled>false</enabled>
            <updatePolicy>never</updatePolicy>
            <checksumPolicy>fail</checksumPolicy>
        </snapshots>
        <id>HDPJetty</id>
        <name>Hadoop Jetty</name>
        <url>http://repo.hortonworks.com/content/repositories/jetty-hadoop/</url>
        <layout>default</layout>
    </repository>
</repositories>
```

## <a name="add-properties"></a><span data-ttu-id="a9221-142">Lägga till egenskaper</span><span class="sxs-lookup"><span data-stu-id="a9221-142">Add properties</span></span>

<span data-ttu-id="a9221-143">Maven kan toodefine projektnivå värden kallas egenskaper.</span><span class="sxs-lookup"><span data-stu-id="a9221-143">Maven allows you toodefine project-level values called properties.</span></span> <span data-ttu-id="a9221-144">I hello __pom.xml__, Lägg till följande text efter hello hello `</repositories>` rad:</span><span class="sxs-lookup"><span data-stu-id="a9221-144">In hello __pom.xml__, add hello following text after hello `</repositories>` line:</span></span>

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <!--
    This is a version of Storm from hello Hortonworks repository that is compatible with HDInsight.
    -->
    <storm.version>1.0.1.2.5.3.0-37</storm.version>
</properties>
```

<span data-ttu-id="a9221-145">Nu kan du använda det här värdet i andra avsnitt i hello `pom.xml`.</span><span class="sxs-lookup"><span data-stu-id="a9221-145">You can now use this value in other sections of hello `pom.xml`.</span></span> <span data-ttu-id="a9221-146">Till exempel när du anger hello version av Storm-komponenter, du kan använda `${storm.version}` i stället för hårda kodning ett värde.</span><span class="sxs-lookup"><span data-stu-id="a9221-146">For example, when specifying hello version of Storm components, you can use `${storm.version}` instead of hard coding a value.</span></span>

## <a name="add-dependencies"></a><span data-ttu-id="a9221-147">Lägga till beroenden</span><span class="sxs-lookup"><span data-stu-id="a9221-147">Add dependencies</span></span>

<span data-ttu-id="a9221-148">Lägg till ett beroende för Storm-komponenter.</span><span class="sxs-lookup"><span data-stu-id="a9221-148">Add a dependency for Storm components.</span></span> <span data-ttu-id="a9221-149">Öppna hello `pom.xml` och Lägg till följande kod i hello hello `<dependencies>` avsnitt:</span><span class="sxs-lookup"><span data-stu-id="a9221-149">Open hello `pom.xml` file and add hello following code in hello `<dependencies>` section:</span></span>

```xml
<dependency>
    <groupId>org.apache.storm</groupId>
    <artifactId>storm-core</artifactId>
    <version>${storm.version}</version>
    <!-- keep storm out of hello jar-with-dependencies -->
    <scope>provided</scope>
</dependency>
```

<span data-ttu-id="a9221-150">Vid kompileringen, Maven använder denna information toolook `storm-core` hello Maven på lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="a9221-150">At compile time, Maven uses this information toolook up `storm-core` in hello Maven repository.</span></span> <span data-ttu-id="a9221-151">Först genomsöks hello-databasen på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="a9221-151">It first looks in hello repository on your local computer.</span></span> <span data-ttu-id="a9221-152">Om hello filer finns inte Maven laddar ned dem från hello offentliga Maven-databasen och lagrar dem i hello lokal databas.</span><span class="sxs-lookup"><span data-stu-id="a9221-152">If hello files aren't there, Maven downloads them from hello public Maven repository and stores them in hello local repository.</span></span>

> [!NOTE]
> <span data-ttu-id="a9221-153">Meddelande hello `<scope>provided</scope>` raden i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="a9221-153">Notice hello `<scope>provided</scope>` line in this section.</span></span> <span data-ttu-id="a9221-154">Den här inställningen instruerar Maven tooexclude **storm-core** från alla JAR-filer som har skapats, eftersom det ges av hello system.</span><span class="sxs-lookup"><span data-stu-id="a9221-154">This setting tells Maven tooexclude **storm-core** from any JAR files that are created, because it is provided by hello system.</span></span>

## <a name="build-configuration"></a><span data-ttu-id="a9221-155">Skapa konfiguration</span><span class="sxs-lookup"><span data-stu-id="a9221-155">Build configuration</span></span>

<span data-ttu-id="a9221-156">Maven plugin-program kan du toocustomize hello build led i hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="a9221-156">Maven plug-ins allow you toocustomize hello build stages of hello project.</span></span> <span data-ttu-id="a9221-157">Till exempel hur hello projekt kompileras eller hur toopackage till JAR-filen.</span><span class="sxs-lookup"><span data-stu-id="a9221-157">For example, how hello project is compiled or how toopackage it into a JAR file.</span></span> <span data-ttu-id="a9221-158">Öppna hello `pom.xml` och Lägg till följande kod direkt ovanför hello hello `</project>` rad.</span><span class="sxs-lookup"><span data-stu-id="a9221-158">Open hello `pom.xml` file and add hello following code directly above hello `</project>` line.</span></span>

```xml
<build>
    <plugins>
    </plugins>
    <resources>
    </resources>
</build>
```

<span data-ttu-id="a9221-159">Det här avsnittet är används tooadd plugin-program, resurser och andra build-konfigurationsalternativ.</span><span class="sxs-lookup"><span data-stu-id="a9221-159">This section is used tooadd plug-ins, resources, and other build configuration options.</span></span> <span data-ttu-id="a9221-160">Fullständiga referenser för hello **pom.xml** fil, se [http://maven.apache.org/pom.html](http://maven.apache.org/pom.html).</span><span class="sxs-lookup"><span data-stu-id="a9221-160">For a full reference of hello **pom.xml** file, see [http://maven.apache.org/pom.html](http://maven.apache.org/pom.html).</span></span>

### <a name="add-plug-ins"></a><span data-ttu-id="a9221-161">Lägga till plugin-program</span><span class="sxs-lookup"><span data-stu-id="a9221-161">Add plug-ins</span></span>

<span data-ttu-id="a9221-162">Apache Storm-topologier som har implementerats i Java hello [Exec Maven Plugin](http://www.mojohaus.org/exec-maven-plugin/) är användbar eftersom den tillåter tooeasily kör hello topologi lokalt i din utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="a9221-162">For Apache Storm topologies implemented in Java, hello [Exec Maven Plugin](http://www.mojohaus.org/exec-maven-plugin/) is useful because it allows you tooeasily run hello topology locally in your development environment.</span></span> <span data-ttu-id="a9221-163">Lägg till följande toohello hello `<plugins>` avsnitt i hello `pom.xml` tooinclude hello Exec Maven plugin-filen:</span><span class="sxs-lookup"><span data-stu-id="a9221-163">Add hello following toohello `<plugins>` section of hello `pom.xml` file tooinclude hello Exec Maven plugin:</span></span>

```xml
<plugin>
    <groupId>org.codehaus.mojo</groupId>
    <artifactId>exec-maven-plugin</artifactId>
    <version>1.4.0</version>
    <executions>
    <execution>
    <goals>
        <goal>exec</goal>
    </goals>
    </execution>
    </executions>
    <configuration>
    <executable>java</executable>
    <includeProjectDependencies>true</includeProjectDependencies>
    <includePluginDependencies>false</includePluginDependencies>
    <classpathScope>compile</classpathScope>
    <mainClass>${storm.topology}</mainClass>
    <cleanupDaemonThreads>false</cleanupDaemonThreads> 
    </configuration>
</plugin>
```

<span data-ttu-id="a9221-164">En annan användbar plugin-program är hello [Apache Maven-kompilatorn Plugin](http://maven.apache.org/plugins/maven-compiler-plugin/), vilket är används toochange alternativ för kompilering.</span><span class="sxs-lookup"><span data-stu-id="a9221-164">Another useful plug-in is hello [Apache Maven Compiler Plugin](http://maven.apache.org/plugins/maven-compiler-plugin/), which is used toochange compilation options.</span></span> <span data-ttu-id="a9221-165">hello ändringar hello Java-version som Maven använder för hello källa och mål för ditt program.</span><span class="sxs-lookup"><span data-stu-id="a9221-165">hello changes hello Java version that Maven uses for hello source and target for your application.</span></span>

* <span data-ttu-id="a9221-166">För HDInsight __3,4 eller tidigare__, ange hello källa och mål Java version too__1.7__.</span><span class="sxs-lookup"><span data-stu-id="a9221-166">For HDInsight __3.4 or earlier__, set hello source and target Java version too__1.7__.</span></span>

* <span data-ttu-id="a9221-167">För HDInsight __3.5__, ange hello källa och mål Java version too__1.8__.</span><span class="sxs-lookup"><span data-stu-id="a9221-167">For HDInsight __3.5__, set hello source and target Java version too__1.8__.</span></span>

<span data-ttu-id="a9221-168">Lägg till följande text i hello hello `<plugins>` avsnitt i hello `pom.xml` tooinclude hello Apache Maven kompileraren plugin-filen.</span><span class="sxs-lookup"><span data-stu-id="a9221-168">Add hello following text in hello `<plugins>` section of hello `pom.xml` file tooinclude hello Apache Maven Compiler plugin.</span></span> <span data-ttu-id="a9221-169">Det här exemplet anger 1.8, så hello HDInsight målversionen är 3.5.</span><span class="sxs-lookup"><span data-stu-id="a9221-169">This example specifies 1.8, so hello target HDInsight version is 3.5.</span></span>

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.3</version>
    <configuration>
    <source>1.8</source>
    <target>1.8</target>
    </configuration>
</plugin>
```

### <a name="configure-resources"></a><span data-ttu-id="a9221-170">Konfigurera resurser</span><span class="sxs-lookup"><span data-stu-id="a9221-170">Configure resources</span></span>

<span data-ttu-id="a9221-171">hello resurser avsnittet kan du tooinclude-kod resurser, till exempel konfigurationsfiler som krävs av komponenter i hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="a9221-171">hello resources section allows you tooinclude non-code resources such as configuration files needed by components in hello topology.</span></span> <span data-ttu-id="a9221-172">I det här exemplet lägger du till följande text i hello hello `<resources>` avsnitt i hello-filen pom.xml.</span><span class="sxs-lookup"><span data-stu-id="a9221-172">For this example, add hello following text in hello `<resources>` section of hello \`pom.xml file.</span></span>

```xml
<resource>
    <directory>${basedir}/resources</directory>
    <filtering>false</filtering>
    <includes>
        <include>log4j2.xml</include>
    </includes>
</resource>
```

<span data-ttu-id="a9221-173">Det här exemplet lägger till hello resurser katalog i hello rot hello projekt (`${basedir}`) som en plats som innehåller resurser och hello-fil med namnet `log4j2.xml`.</span><span class="sxs-lookup"><span data-stu-id="a9221-173">This example adds hello resources directory in hello root of hello project (`${basedir}`) as a location that contains resources, and includes hello file named `log4j2.xml`.</span></span> <span data-ttu-id="a9221-174">Den här filen är används tooconfigure vilken information som loggas av hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="a9221-174">This file is used tooconfigure what information is logged by hello topology.</span></span>

## <a name="create-hello-topology"></a><span data-ttu-id="a9221-175">Skapa hello-topologi</span><span class="sxs-lookup"><span data-stu-id="a9221-175">Create hello topology</span></span>

<span data-ttu-id="a9221-176">En Java-baserad Apache Storm-topologi som består av tre komponenter som du måste skapa (eller referens) som ett beroende.</span><span class="sxs-lookup"><span data-stu-id="a9221-176">A Java-based Apache Storm topology consists of three components that you must author (or reference) as a dependency.</span></span>

* <span data-ttu-id="a9221-177">**Spouts**: läser data från externa datakällor och skickar dataströmmar till hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="a9221-177">**Spouts**: Reads data from external sources and emits streams of data into hello topology.</span></span>

* <span data-ttu-id="a9221-178">**Bolts**: utför bearbetning på dataströmmar som orsakat av kanaler eller andra bultar och genererar en eller flera strömmar.</span><span class="sxs-lookup"><span data-stu-id="a9221-178">**Bolts**: Performs processing on streams emitted by spouts or other bolts, and emits one or more streams.</span></span>

* <span data-ttu-id="a9221-179">**Topologi**: definierar hur hello spouts och bultar ordnas och tillhandahåller en startpunkt för hello för hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="a9221-179">**Topology**: Defines how hello spouts and bolts are arranged, and provides hello entry point for hello topology.</span></span>

### <a name="create-hello-spout"></a><span data-ttu-id="a9221-180">Skapa hello kanal</span><span class="sxs-lookup"><span data-stu-id="a9221-180">Create hello spout</span></span>

<span data-ttu-id="a9221-181">tooreduce krav för att installera externa datakällor hello följande kanal bara genererar slumpmässiga meningar.</span><span class="sxs-lookup"><span data-stu-id="a9221-181">tooreduce requirements for setting up external data sources, hello following spout simply emits random sentences.</span></span> <span data-ttu-id="a9221-182">Det är en modifierad version av en kanal som medföljer hello [Storm Starter-exempel](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter).</span><span class="sxs-lookup"><span data-stu-id="a9221-182">It is a modified version of a spout that is provided with hello [Storm-Starter examples](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter).</span></span>

> [!NOTE]
> <span data-ttu-id="a9221-183">Ett exempel på en kanal som läser från en extern datakälla finns i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="a9221-183">For an example of a spout that reads from an external data source, see one of hello following examples:</span></span>
>
> * <span data-ttu-id="a9221-184">[TwitterSampleSPout](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter/spout/TwitterSampleSpout.java): en exempel-kanal som läser från Twitter</span><span class="sxs-lookup"><span data-stu-id="a9221-184">[TwitterSampleSPout](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter/spout/TwitterSampleSpout.java): An example spout that reads from Twitter</span></span>
> * <span data-ttu-id="a9221-185">[Storm-Kafka](https://github.com/apache/storm/tree/0.10.x-branch/external/storm-kafka): en kanal som läser från Kafka</span><span class="sxs-lookup"><span data-stu-id="a9221-185">[Storm-Kafka](https://github.com/apache/storm/tree/0.10.x-branch/external/storm-kafka): A spout that reads from Kafka</span></span>

<span data-ttu-id="a9221-186">Skapa en fil med namnet för hello kanal `RandomSentenceSpout.java` i hello `src\main\java\com\microsoft\example` katalogen och Använd hello följande Java-kod som hello innehållet:</span><span class="sxs-lookup"><span data-stu-id="a9221-186">For hello spout, create a file named `RandomSentenceSpout.java` in hello `src\main\java\com\microsoft\example` directory and use hello following Java code as hello contents:</span></span>

```java
package com.microsoft.example;

import org.apache.storm.spout.SpoutOutputCollector;
import org.apache.storm.task.TopologyContext;
import org.apache.storm.topology.OutputFieldsDeclarer;
import org.apache.storm.topology.base.BaseRichSpout;
import org.apache.storm.tuple.Fields;
import org.apache.storm.tuple.Values;
import org.apache.storm.utils.Utils;

import java.util.Map;
import java.util.Random;

//This spout randomly emits sentences
public class RandomSentenceSpout extends BaseRichSpout {
  //Collector used tooemit output
  SpoutOutputCollector _collector;
  //Used toogenerate a random number
  Random _rand;

  //Open is called when an instance of hello class is created
  @Override
  public void open(Map conf, TopologyContext context, SpoutOutputCollector collector) {
  //Set hello instance collector toohello one passed in
    _collector = collector;
    //For randomness
    _rand = new Random();
  }

  //Emit data toohello stream
  @Override
  public void nextTuple() {
  //Sleep for a bit
    Utils.sleep(100);
    //hello sentences that are randomly emitted
    String[] sentences = new String[]{ "hello cow jumped over hello moon", "an apple a day keeps hello doctor away",
        "four score and seven years ago", "snow white and hello seven dwarfs", "i am at two with nature" };
    //Randomly pick a sentence
    String sentence = sentences[_rand.nextInt(sentences.length)];
    //Emit hello sentence
    _collector.emit(new Values(sentence));
  }

  //Ack is not implemented since this is a basic example
  @Override
  public void ack(Object id) {
  }

  //Fail is not implemented since this is a basic example
  @Override
  public void fail(Object id) {
  }

  //Declare hello output fields. In this case, an sentence
  @Override
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
    declarer.declare(new Fields("sentence"));
  }
}
```

> [!NOTE]
> <span data-ttu-id="a9221-187">Även om den här topologin använder endast en kanal kan har andra flera som går in data från olika källor i hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="a9221-187">Although this topology uses only one spout, others may have several that feed data from different sources into hello topology.</span></span>

### <a name="create-hello-bolts"></a><span data-ttu-id="a9221-188">Skapa hello bultar</span><span class="sxs-lookup"><span data-stu-id="a9221-188">Create hello bolts</span></span>

<span data-ttu-id="a9221-189">Bultar hantera hello databearbetning.</span><span class="sxs-lookup"><span data-stu-id="a9221-189">Bolts handle hello data processing.</span></span> <span data-ttu-id="a9221-190">Den här topologin används två bultar:</span><span class="sxs-lookup"><span data-stu-id="a9221-190">This topology uses two bolts:</span></span>

* <span data-ttu-id="a9221-191">**SplitSentence**: delar hello meningar sänds av **RandomSentenceSpout** till enskilda ord.</span><span class="sxs-lookup"><span data-stu-id="a9221-191">**SplitSentence**: Splits hello sentences emitted by **RandomSentenceSpout** into individual words.</span></span>

* <span data-ttu-id="a9221-192">**WordCount**: räknar hur många gånger varje ord har uppstått.</span><span class="sxs-lookup"><span data-stu-id="a9221-192">**WordCount**: Counts how many times each word has occurred.</span></span>

> [!NOTE]
> <span data-ttu-id="a9221-193">Bultar kan göra något, till exempel beräkning, beständiga eller pratar tooexternal komponenter.</span><span class="sxs-lookup"><span data-stu-id="a9221-193">Bolts can do anything, for example, computation, persistence, or talking tooexternal components.</span></span>

<span data-ttu-id="a9221-194">Skapa två nya filer `SplitSentence.java` och `WordCount.java` i hello `src\main\java\com\microsoft\example` directory.</span><span class="sxs-lookup"><span data-stu-id="a9221-194">Create two new files, `SplitSentence.java` and `WordCount.java` in hello `src\main\java\com\microsoft\example` directory.</span></span> <span data-ttu-id="a9221-195">Använd följande text som hello innehållet för hello filer hello:</span><span class="sxs-lookup"><span data-stu-id="a9221-195">Use hello following text as hello contents for hello files:</span></span>

#### <a name="splitsentence"></a><span data-ttu-id="a9221-196">SplitSentence</span><span class="sxs-lookup"><span data-stu-id="a9221-196">SplitSentence</span></span>

```java
package com.microsoft.example;

import java.text.BreakIterator;

import org.apache.storm.topology.BasicOutputCollector;
import org.apache.storm.topology.OutputFieldsDeclarer;
import org.apache.storm.topology.base.BaseBasicBolt;
import org.apache.storm.tuple.Fields;
import org.apache.storm.tuple.Tuple;
import org.apache.storm.tuple.Values;

//There are a variety of bolt types. In this case, use BaseBasicBolt
public class SplitSentence extends BaseBasicBolt {

  //Execute is called tooprocess tuples
  @Override
  public void execute(Tuple tuple, BasicOutputCollector collector) {
    //Get hello sentence content from hello tuple
    String sentence = tuple.getString(0);
    //An iterator tooget each word
    BreakIterator boundary=BreakIterator.getWordInstance();
    //Give hello iterator hello sentence
    boundary.setText(sentence);
    //Find hello beginning first word
    int start=boundary.first();
    //Iterate over each word and emit it toohello output stream
    for (int end=boundary.next(); end != BreakIterator.DONE; start=end, end=boundary.next()) {
      //get hello word
      String word=sentence.substring(start,end);
      //If a word is whitespace characters, replace it with empty
      word=word.replaceAll("\\s+","");
      //if it's an actual word, emit it
      if (!word.equals("")) {
        collector.emit(new Values(word));
      }
    }
  }

  //Declare that emitted tuples contain a word field
  @Override
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
    declarer.declare(new Fields("word"));
  }
}
```

#### <a name="wordcount"></a><span data-ttu-id="a9221-197">WordCount</span><span class="sxs-lookup"><span data-stu-id="a9221-197">WordCount</span></span>

```java
package com.microsoft.example;

import java.util.HashMap;
import java.util.Map;
import java.util.Iterator;

import org.apache.storm.Constants;
import org.apache.storm.topology.BasicOutputCollector;
import org.apache.storm.topology.OutputFieldsDeclarer;
import org.apache.storm.topology.base.BaseBasicBolt;
import org.apache.storm.tuple.Fields;
import org.apache.storm.tuple.Tuple;
import org.apache.storm.tuple.Values;
import org.apache.storm.Config;

// For logging
import org.apache.logging.log4j.Logger;
import org.apache.logging.log4j.LogManager;

//There are a variety of bolt types. In this case, use BaseBasicBolt
public class WordCount extends BaseBasicBolt {
  //Create logger for this class
  private static final Logger logger = LogManager.getLogger(WordCount.class);
  //For holding words and counts
  Map<String, Integer> counts = new HashMap<String, Integer>();
  //How often tooemit a count of words
  private Integer emitFrequency;

  // Default constructor
  public WordCount() {
      emitFrequency=5; // Default too60 seconds
  }

  // Constructor that sets emit frequency
  public WordCount(Integer frequency) {
      emitFrequency=frequency;
  }

  //Configure frequency of tick tuples for this bolt
  //This delivers a 'tick' tuple on a specific interval,
  //which is used tootrigger certain actions
  @Override
  public Map<String, Object> getComponentConfiguration() {
      Config conf = new Config();
      conf.put(Config.TOPOLOGY_TICK_TUPLE_FREQ_SECS, emitFrequency);
      return conf;
  }

  //execute is called tooprocess tuples
  @Override
  public void execute(Tuple tuple, BasicOutputCollector collector) {
    //If it's a tick tuple, emit all words and counts
    if(tuple.getSourceComponent().equals(Constants.SYSTEM_COMPONENT_ID)
            && tuple.getSourceStreamId().equals(Constants.SYSTEM_TICK_STREAM_ID)) {
      for(String word : counts.keySet()) {
        Integer count = counts.get(word);
        collector.emit(new Values(word, count));
        logger.info("Emitting a count of " + count + " for word " + word);
      }
    } else {
      //Get hello word contents from hello tuple
      String word = tuple.getString(0);
      //Have we counted any already?
      Integer count = counts.get(word);
      if (count == null)
        count = 0;
      //Increment hello count and store it
      count++;
      counts.put(word, count);
    }
  }

  //Declare that this emits a tuple containing two fields; word and count
  @Override
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
    declarer.declare(new Fields("word", "count"));
  }
}
```

### <a name="define-hello-topology"></a><span data-ttu-id="a9221-198">Definiera hello-topologi</span><span class="sxs-lookup"><span data-stu-id="a9221-198">Define hello topology</span></span>

<span data-ttu-id="a9221-199">hello topologi kopplar samman hello kanaler och bolts tillsammans i ett diagram som definierar hur data flödar mellan hello komponenter.</span><span class="sxs-lookup"><span data-stu-id="a9221-199">hello topology ties hello spouts and bolts together into a graph, which defines how data flows between hello components.</span></span> <span data-ttu-id="a9221-200">Det ger också parallellitet tips som Storm använder när du skapar instanser av hello komponenter i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="a9221-200">It also provides parallelism hints that Storm uses when creating instances of hello components within hello cluster.</span></span>

<span data-ttu-id="a9221-201">hello är följande bild ett grundläggande diagram hello diagrammet med komponenter för den här topologin.</span><span class="sxs-lookup"><span data-stu-id="a9221-201">hello following image is a basic diagram of hello graph of components for this topology.</span></span>

![diagram som visar hello spouts och bolts placering](./media/hdinsight-storm-develop-java-topology/wordcount-topology.png)

<span data-ttu-id="a9221-203">tooimplement Hej topologi, skapa en fil med namnet `WordCountTopology.java` i hello `src\main\java\com\microsoft\example` directory.</span><span class="sxs-lookup"><span data-stu-id="a9221-203">tooimplement hello topology, create a file named `WordCountTopology.java` in hello `src\main\java\com\microsoft\example` directory.</span></span> <span data-ttu-id="a9221-204">Använd hello följande Java-kod som hello innehållet i hello-fil:</span><span class="sxs-lookup"><span data-stu-id="a9221-204">Use hello following Java code as hello contents of hello file:</span></span>

```java
package com.microsoft.example;

import org.apache.storm.Config;
import org.apache.storm.LocalCluster;
import org.apache.storm.StormSubmitter;
import org.apache.storm.topology.TopologyBuilder;
import org.apache.storm.tuple.Fields;

import com.microsoft.example.RandomSentenceSpout;

public class WordCountTopology {

  //Entry point for hello topology
  public static void main(String[] args) throws Exception {
  //Used toobuild hello topology
    TopologyBuilder builder = new TopologyBuilder();
    //Add hello spout, with a name of 'spout'
    //and parallelism hint of 5 executors
    builder.setSpout("spout", new RandomSentenceSpout(), 5);
    //Add hello SplitSentence bolt, with a name of 'split'
    //and parallelism hint of 8 executors
    //shufflegrouping subscribes toohello spout, and equally distributes
    //tuples (sentences) across instances of hello SplitSentence bolt
    builder.setBolt("split", new SplitSentence(), 8).shuffleGrouping("spout");
    //Add hello counter, with a name of 'count'
    //and parallelism hint of 12 executors
    //fieldsgrouping subscribes toohello split bolt, and
    //ensures that hello same word is sent toohello same instance (group by field 'word')
    builder.setBolt("count", new WordCount(), 12).fieldsGrouping("split", new Fields("word"));

    //new configuration
    Config conf = new Config();
    //Set toofalse toodisable debug information when
    // running in production on a cluster
    conf.setDebug(false);

    //If there are arguments, we are running on a cluster
    if (args != null && args.length > 0) {
      //parallelism hint tooset hello number of workers
      conf.setNumWorkers(3);
      //submit hello topology
      StormSubmitter.submitTopology(args[0], conf, builder.createTopology());
    }
    //Otherwise, we are running locally
    else {
      //Cap hello maximum number of executors that can be spawned
      //for a component too3
      conf.setMaxTaskParallelism(3);
      //LocalCluster is used toorun locally
      LocalCluster cluster = new LocalCluster();
      //submit hello topology
      cluster.submitTopology("word-count", conf, builder.createTopology());
      //sleep
      Thread.sleep(10000);
      //shut down hello cluster
      cluster.shutdown();
    }
  }
}
```

### <a name="configure-logging"></a><span data-ttu-id="a9221-205">Konfigurera loggning</span><span class="sxs-lookup"><span data-stu-id="a9221-205">Configure logging</span></span>

<span data-ttu-id="a9221-206">Storm använder Apache Log4j toolog information.</span><span class="sxs-lookup"><span data-stu-id="a9221-206">Storm uses Apache Log4j toolog information.</span></span> <span data-ttu-id="a9221-207">Om du inte konfigurerar loggning avger hello topologi diagnostisk information.</span><span class="sxs-lookup"><span data-stu-id="a9221-207">If you do not configure logging, hello topology emits diagnostic information.</span></span> <span data-ttu-id="a9221-208">toocontrol vad är inloggad, skapa en fil med namnet `log4j2.xml` i hello `resources` directory.</span><span class="sxs-lookup"><span data-stu-id="a9221-208">toocontrol what is logged, create a file named `log4j2.xml` in hello `resources` directory.</span></span> <span data-ttu-id="a9221-209">Använd hello följande XML som hello innehållet i hello-fil.</span><span class="sxs-lookup"><span data-stu-id="a9221-209">Use hello following XML as hello contents of hello file.</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration>
<Appenders>
    <Console name="STDOUT" target="SYSTEM_OUT">
        <PatternLayout pattern="%d{HH:mm:ss} [%t] %-5level %logger{36} - %msg%n"/>
    </Console>
</Appenders>
<Loggers>
    <Logger name="com.microsoft.example" level="trace" additivity="false">
        <AppenderRef ref="STDOUT"/>
    </Logger>
    <Root level="error">
        <Appender-Ref ref="STDOUT"/>
    </Root>
</Loggers>
</Configuration>
```

<span data-ttu-id="a9221-210">Den här XML konfigurerar en ny loggare för hello `com.microsoft.example` -klassen, som innehåller hello komponenter i det här exemplet-topologi.</span><span class="sxs-lookup"><span data-stu-id="a9221-210">This XML configures a new logger for hello `com.microsoft.example` class, which includes hello components in this example topology.</span></span> <span data-ttu-id="a9221-211">hello anges tootrace för den här loggaren som samlar in alla loggningsinformation orsakat av komponenter i den här topologin.</span><span class="sxs-lookup"><span data-stu-id="a9221-211">hello level is set tootrace for this logger, which captures any logging information emitted by components in this topology.</span></span>

<span data-ttu-id="a9221-212">Hej `<Root level="error">` avsnittet konfigureras hello rotnivå loggning (allt inte i `com.microsoft.example`) tooonly logginformation för fel.</span><span class="sxs-lookup"><span data-stu-id="a9221-212">hello `<Root level="error">` section configures hello root level of logging (everything not in `com.microsoft.example`) tooonly log error information.</span></span>

<span data-ttu-id="a9221-213">Läs mer om hur du konfigurerar loggning för Log4j [http://logging.apache.org/log4j/2.x/manual/configuration.html](http://logging.apache.org/log4j/2.x/manual/configuration.html).</span><span class="sxs-lookup"><span data-stu-id="a9221-213">For more information on configuring logging for Log4j, see [http://logging.apache.org/log4j/2.x/manual/configuration.html](http://logging.apache.org/log4j/2.x/manual/configuration.html).</span></span>

> [!NOTE]
> <span data-ttu-id="a9221-214">Storm-version 0.10.0 och högre använder Log4j 2.x.</span><span class="sxs-lookup"><span data-stu-id="a9221-214">Storm version 0.10.0 and higher use Log4j 2.x.</span></span> <span data-ttu-id="a9221-215">Äldre versioner av storm används Log4j 1.x, som används av ett annat format för konfigurationen av loggen.</span><span class="sxs-lookup"><span data-stu-id="a9221-215">Older versions of storm used Log4j 1.x, which used a different format for log configuration.</span></span> <span data-ttu-id="a9221-216">Mer information om hello äldre konfiguration finns [http://wiki.apache.org/logging-log4j/Log4jXmlFormat](http://wiki.apache.org/logging-log4j/Log4jXmlFormat).</span><span class="sxs-lookup"><span data-stu-id="a9221-216">For information on hello older configuration, see [http://wiki.apache.org/logging-log4j/Log4jXmlFormat](http://wiki.apache.org/logging-log4j/Log4jXmlFormat).</span></span>

## <a name="test-hello-topology-locally"></a><span data-ttu-id="a9221-217">Testa hello lokalt topologi</span><span class="sxs-lookup"><span data-stu-id="a9221-217">Test hello topology locally</span></span>

<span data-ttu-id="a9221-218">När du har sparat hello filer, kommando Använd hello följande lokalt tootest hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="a9221-218">After you save hello files, use hello following command tootest hello topology locally.</span></span>

```bash
mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCountTopology
```

<span data-ttu-id="a9221-219">När den kör visar hello topologi startinformation.</span><span class="sxs-lookup"><span data-stu-id="a9221-219">As it runs, hello topology displays startup information.</span></span> <span data-ttu-id="a9221-220">hello är följande text ett exempel på hello word antal utdata:</span><span class="sxs-lookup"><span data-stu-id="a9221-220">hello following text is an example of hello word count output:</span></span>

    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
    17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word snow

<span data-ttu-id="a9221-221">Det här exemplet loggen visar hello ordet ”och” har släppts 113 gånger.</span><span class="sxs-lookup"><span data-stu-id="a9221-221">This example log indicates that hello word 'and' has been emitted 113 times.</span></span> <span data-ttu-id="a9221-222">hello antal fortsätter toogo upp så länge hello topologi körs eftersom hello kanal avger alltid hello samma meningar.</span><span class="sxs-lookup"><span data-stu-id="a9221-222">hello count continues toogo up as long as hello topology runs because hello spout continuously emits hello same sentences.</span></span>

<span data-ttu-id="a9221-223">Det finns ett 5 sekunder intervall mellan utsläpp av ord och antal.</span><span class="sxs-lookup"><span data-stu-id="a9221-223">There is a 5-second interval between emission of words and counts.</span></span> <span data-ttu-id="a9221-224">Hej **WordCount** -komponenten har konfigurerats tooonly generera information när en tidstuppel tas emot.</span><span class="sxs-lookup"><span data-stu-id="a9221-224">hello **WordCount** component is configured tooonly emit information when a tick tuple arrives.</span></span> <span data-ttu-id="a9221-225">Begär att skalstreck tupplar levereras endast var femte sekund.</span><span class="sxs-lookup"><span data-stu-id="a9221-225">It requests that tick tuples are only delivered every five seconds.</span></span>

## <a name="convert-hello-topology-tooflux"></a><span data-ttu-id="a9221-226">Konvertera hello topologi tooFlux</span><span class="sxs-lookup"><span data-stu-id="a9221-226">Convert hello topology tooFlux</span></span>

<span data-ttu-id="a9221-227">Som är ett nytt regelverk tillgänglig med Storm 0.10.0 och högre, vilket gör att du tooseparate konfigurationen från implementering.</span><span class="sxs-lookup"><span data-stu-id="a9221-227">Flux is a new framework available with Storm 0.10.0 and higher, which allows you tooseparate configuration from implementation.</span></span> <span data-ttu-id="a9221-228">Komponenterna definieras fortfarande i Java, men hello topologi definieras med hjälp av en YAML-fil.</span><span class="sxs-lookup"><span data-stu-id="a9221-228">Your components are still defined in Java, but hello topology is defined using a YAML file.</span></span> <span data-ttu-id="a9221-229">Du kan paketdefinition en standard-topologi med projektet eller använda en fristående fil när du skickar in hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="a9221-229">You can package a default topology definition with your project, or use a standalone file when submitting hello topology.</span></span> <span data-ttu-id="a9221-230">När hello topologi tooStorm, kan du använda miljövariabler eller konfigurationsvärden filer toopopulate i hello YAML topologi definition.</span><span class="sxs-lookup"><span data-stu-id="a9221-230">When submitting hello topology tooStorm, you can use environment variables or configuration files toopopulate values in hello YAML topology definition.</span></span>

<span data-ttu-id="a9221-231">Hej YAML filen definierar hello komponenter toouse för hello topologi och hello dataflödet mellan dem.</span><span class="sxs-lookup"><span data-stu-id="a9221-231">hello YAML file defines hello components toouse for hello topology and hello data flow between them.</span></span> <span data-ttu-id="a9221-232">Du kan inkludera en YAML-fil som en del av hello jar-filen eller så kan du använda en extern YAML-fil.</span><span class="sxs-lookup"><span data-stu-id="a9221-232">You can include a YAML file as part of hello jar file or you can use an external YAML file.</span></span>

<span data-ttu-id="a9221-233">Mer information om som finns [som framework (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).</span><span class="sxs-lookup"><span data-stu-id="a9221-233">For more information on Flux, see [Flux framework (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).</span></span>

> [!WARNING]
> <span data-ttu-id="a9221-234">På grund av tooa [programfel (https://issues.apache.org/jira/browse/STORM-2055)](https://issues.apache.org/jira/browse/STORM-2055) med Storm 1.0.1 behöva tooinstall en [Storm utvecklingsmiljö](https://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html) toorun som lokalt topologier.</span><span class="sxs-lookup"><span data-stu-id="a9221-234">Due tooa [bug (https://issues.apache.org/jira/browse/STORM-2055)](https://issues.apache.org/jira/browse/STORM-2055) with Storm 1.0.1, you may need tooinstall a [Storm development environment](https://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html) toorun Flux topologies locally.</span></span>

1. <span data-ttu-id="a9221-235">Flytta hello `WordCountTopology.java` filen utanför hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="a9221-235">Move hello `WordCountTopology.java` file out of hello project.</span></span> <span data-ttu-id="a9221-236">Hello-topologi har definierats tidigare den här filen, men behövs inte med som.</span><span class="sxs-lookup"><span data-stu-id="a9221-236">Previously, this file defined hello topology, but isn't needed with Flux.</span></span>

2. <span data-ttu-id="a9221-237">I hello `resources` directory, skapa en fil med namnet `topology.yaml`.</span><span class="sxs-lookup"><span data-stu-id="a9221-237">In hello `resources` directory, create a file named `topology.yaml`.</span></span> <span data-ttu-id="a9221-238">Använd hello följande text som hello innehållet i den här filen.</span><span class="sxs-lookup"><span data-stu-id="a9221-238">Use hello following text as hello contents of this file.</span></span>

        name: "wordcount"       # friendly name for hello topology
        
        config:                 # Topology configuration
        topology.workers: 1     # Hint for hello number of workers toocreate
        
        spouts:                 # Spout definitions
        - id: "sentence-spout"
            className: "com.microsoft.example.RandomSentenceSpout"
            parallelism: 1      # parallelism hint
        
        bolts:                  # Bolt definitions
        - id: "splitter-bolt"
            className: "com.microsoft.example.SplitSentence"
            parallelism: 1
         
        - id: "counter-bolt"
            className: "com.microsoft.example.WordCount"
            constructorArgs:
                - 10
            parallelism: 1
        
        streams:                # Stream definitions
            - name: "Spout --> Splitter" # name isn't used (placeholder for logging, UI, etc.)
            from: "sentence-spout"       # hello stream emitter
            to: "splitter-bolt"          # hello stream consumer
            grouping:                    # Grouping type
                type: SHUFFLE
          
            - name: "Splitter -> Counter"
            from: "splitter-bolt"
            to: "counter-bolt"
            grouping:
            type: FIELDS
                args: ["word"]           # field(s) toogroup on

3. <span data-ttu-id="a9221-239">Gör följande ändringar toohello hello `pom.xml` fil.</span><span class="sxs-lookup"><span data-stu-id="a9221-239">Make hello following changes toohello `pom.xml` file.</span></span>
   
   * <span data-ttu-id="a9221-240">Lägg till följande nya beroendet hello hello `<dependencies>` avsnitt:</span><span class="sxs-lookup"><span data-stu-id="a9221-240">Add hello following new dependency in hello `<dependencies>` section:</span></span>
     
        ```xml
        <!-- Add a dependency on hello Flux framework -->
        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>flux-core</artifactId>
            <version>${storm.version}</version>
        </dependency>
        ```
   * <span data-ttu-id="a9221-241">Lägg till följande plugin-programmet toohello hello `<plugins>` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a9221-241">Add hello following plugin toohello `<plugins>` section.</span></span> <span data-ttu-id="a9221-242">Det här plugin-programmet hanterar hello ett paket (jar-fil) skapas för hello projekt och gäller vissa specifika tooFlux transformationer när du skapar hello-paketet.</span><span class="sxs-lookup"><span data-stu-id="a9221-242">This plugin handles hello creation of a package (jar file) for hello project, and applies some transformations specific tooFlux when creating hello package.</span></span>
     
        ```xml
        <!-- build an uber jar -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-shade-plugin</artifactId>
            <version>2.3</version>
            <configuration>
                <transformers>
                    <!-- Keep us from getting a "can't overwrite file error" -->
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer" />
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer" />
                    <!-- We're using Flux, so refer tooit as main -->
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                        <mainClass>org.apache.storm.flux.Flux</mainClass>
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
        ```

   * <span data-ttu-id="a9221-243">I hello **exec-maven-plugin-programmet** `<configuration>` ändrar hello värde för `<mainClass>` för`org.apache.storm.flux.Flux`.</span><span class="sxs-lookup"><span data-stu-id="a9221-243">In hello **exec-maven-plugin** `<configuration>` section, change hello value for `<mainClass>` too`org.apache.storm.flux.Flux`.</span></span> <span data-ttu-id="a9221-244">Den här inställningen som toohandle körs lokalt hello topologi under utveckling.</span><span class="sxs-lookup"><span data-stu-id="a9221-244">This setting allows Flux toohandle running hello topology locally in development.</span></span>

   * <span data-ttu-id="a9221-245">I hello `<resources>` lägger du till följande toohello hello `<includes>`.</span><span class="sxs-lookup"><span data-stu-id="a9221-245">In hello `<resources>` section, add hello following toohello `<includes>`.</span></span> <span data-ttu-id="a9221-246">Den här XML innehåller hello YAML-fil som definierar hello topologi som en del av hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="a9221-246">This XML includes hello YAML file that defines hello topology as part of hello project.</span></span>

        ```xml
        <include>topology.yaml</include>
        ```

## <a name="test-hello-flux-topology-locally"></a><span data-ttu-id="a9221-247">Testa hello som lokalt topologi</span><span class="sxs-lookup"><span data-stu-id="a9221-247">Test hello flux topology locally</span></span>

1. <span data-ttu-id="a9221-248">Använd följande toocompile hello och kör hello som topologi med Maven:</span><span class="sxs-lookup"><span data-stu-id="a9221-248">Use hello following toocompile and execute hello Flux topology using Maven:</span></span>

    ```bash
    mvn compile exec:java -Dexec.args="--local -R /topology.yaml"
    ```

    <span data-ttu-id="a9221-249">Om du använder PowerShell använder du följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="a9221-249">If you are using PowerShell, use hello following command:</span></span>

    ```bash
    mvn compile exec:java "-Dexec.args=--local -R /topology.yaml"
    ```

    > [!WARNING]
    > <span data-ttu-id="a9221-250">Om din topologi använder Storm 1.0.1 bits, misslyckas kommandot.</span><span class="sxs-lookup"><span data-stu-id="a9221-250">If your topology uses Storm 1.0.1 bits, this command fails.</span></span> <span data-ttu-id="a9221-251">Felet orsakas av [https://issues.apache.org/jira/browse/STORM-2055](https://issues.apache.org/jira/browse/STORM-2055).</span><span class="sxs-lookup"><span data-stu-id="a9221-251">This failure is caused by [https://issues.apache.org/jira/browse/STORM-2055](https://issues.apache.org/jira/browse/STORM-2055).</span></span> <span data-ttu-id="a9221-252">I stället [installera Storm i din utvecklingsmiljö](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html) och Använd hello följande information.</span><span class="sxs-lookup"><span data-stu-id="a9221-252">Instead, [install Storm in your development environment](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html) and use hello following information.</span></span>

    <span data-ttu-id="a9221-253">Om du har [installerat Storm i din utvecklingsmiljö](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html), du kan använda följande kommandon i stället hello:</span><span class="sxs-lookup"><span data-stu-id="a9221-253">If you have [installed Storm in your development environment](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html), you can use hello following commands instead:</span></span>

    ```bash
    mvn compile package
    storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /topology.yaml
    ```

    <span data-ttu-id="a9221-254">Hej `--local` parametern körs hello topologi i lokalt läge på din utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="a9221-254">hello `--local` parameter runs hello topology in local mode on your development environment.</span></span> <span data-ttu-id="a9221-255">Hej `-R /topology.yaml` parametern använder hello `topology.yaml` filen resurs från hello jar-filen toodefine hello topologi.</span><span class="sxs-lookup"><span data-stu-id="a9221-255">hello `-R /topology.yaml` parameter uses hello `topology.yaml` file resource from hello jar file toodefine hello topology.</span></span>

    <span data-ttu-id="a9221-256">När den kör visar hello topologi startinformation.</span><span class="sxs-lookup"><span data-stu-id="a9221-256">As it runs, hello topology displays startup information.</span></span> <span data-ttu-id="a9221-257">hello efter texten är ett exempel på utdata hello:</span><span class="sxs-lookup"><span data-stu-id="a9221-257">hello following text is an example of hello output:</span></span>

        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
        17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs

    <span data-ttu-id="a9221-258">Det finns en fördröjning på 10 sekunder mellan batchar av loggade informationen.</span><span class="sxs-lookup"><span data-stu-id="a9221-258">There is a 10-second delay between batches of logged information.</span></span>

2. <span data-ttu-id="a9221-259">Skapa en kopia av hello `topology.yaml` filen från hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="a9221-259">Make a copy of hello `topology.yaml` file from hello project.</span></span> <span data-ttu-id="a9221-260">Namnet hello nya filen `newtopology.yaml`.</span><span class="sxs-lookup"><span data-stu-id="a9221-260">Name hello new file `newtopology.yaml`.</span></span> <span data-ttu-id="a9221-261">I hello `newtopology.yaml` fil, hitta hello följande avsnitt och ändra hello värdet på `10` för`5`.</span><span class="sxs-lookup"><span data-stu-id="a9221-261">In hello `newtopology.yaml` file, find hello following section and change hello value of `10` too`5`.</span></span> <span data-ttu-id="a9221-262">Ändring ändringar hello intervallet mellan avger batchar av word räknar från too5 10 sekunder.</span><span class="sxs-lookup"><span data-stu-id="a9221-262">This modification changes hello interval between emitting batches of word counts from 10 seconds too5.</span></span>

    ```yaml
    - id: "counter-bolt"
    className: "com.microsoft.example.WordCount"
    constructorArgs:
    - 5
    parallelism: 1
    ```yaml

3. toorun hello topology, use hello following command:

    ```bash
    mvn exec:java -Dexec.args="--local /path/to/newtopology.yaml"
    ```

    <span data-ttu-id="a9221-263">Eller, om du använder Storm på din utvecklingsmiljö:</span><span class="sxs-lookup"><span data-stu-id="a9221-263">Or, if you have Storm on your development environment:</span></span>

    ```bash
    storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local /path/to/newtopology.yaml
    ```

    <span data-ttu-id="a9221-264">Ändra hello `/path/to/newtopology.yaml` toohello sökväg toohello newtopology.yaml fil du skapade i föregående steg i hello.</span><span class="sxs-lookup"><span data-stu-id="a9221-264">Change hello `/path/to/newtopology.yaml` toohello path toohello newtopology.yaml file you created in hello previous step.</span></span> <span data-ttu-id="a9221-265">Det här kommandot använder hello newtopology.yaml som hello topologi definition.</span><span class="sxs-lookup"><span data-stu-id="a9221-265">This command uses hello newtopology.yaml as hello topology definition.</span></span> <span data-ttu-id="a9221-266">Eftersom vi skickat hello `compile` parametern Maven använder hello version av hello-projekt som skapats i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="a9221-266">Since we didn't include hello `compile` parameter, Maven uses hello version of hello project built in previous steps.</span></span>

    <span data-ttu-id="a9221-267">En gång hello topologi börjar, bör upptäcker du hello tiden mellan angivna batchar har tooreflect hello värdet i newtopology.yaml.</span><span class="sxs-lookup"><span data-stu-id="a9221-267">Once hello topology starts, you should notice that hello time between emitted batches has changed tooreflect hello value in newtopology.yaml.</span></span> <span data-ttu-id="a9221-268">Så att du kan se att du kan ändra konfigurationen via en YAML utan toorecompile hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="a9221-268">So you can see that you can change your configuration through a YAML file without having toorecompile hello topology.</span></span>

<span data-ttu-id="a9221-269">Mer information om dessa och andra funktioner i hello som framework finns [som (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).</span><span class="sxs-lookup"><span data-stu-id="a9221-269">For more information on these and other features of hello Flux framework, see [Flux (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).</span></span>

## <a name="trident"></a><span data-ttu-id="a9221-270">Trident</span><span class="sxs-lookup"><span data-stu-id="a9221-270">Trident</span></span>

<span data-ttu-id="a9221-271">Trident är en abstraktion på hög nivå som tillhandahålls av Storm.</span><span class="sxs-lookup"><span data-stu-id="a9221-271">Trident is a high-level abstraction that is provided by Storm.</span></span> <span data-ttu-id="a9221-272">Det stöder tillståndskänslig bearbetning.</span><span class="sxs-lookup"><span data-stu-id="a9221-272">It supports stateful processing.</span></span> <span data-ttu-id="a9221-273">hello viktig fördel med Trident är att det garanterar att varje meddelande som anger hello topologi bearbetas bara en gång.</span><span class="sxs-lookup"><span data-stu-id="a9221-273">hello primary advantage of Trident is that it can guarantee that every message that enters hello topology is processed only once.</span></span> <span data-ttu-id="a9221-274">Utan att använda Trident garantera topologin endast att meddelanden bearbetas minst en gång.</span><span class="sxs-lookup"><span data-stu-id="a9221-274">Without using Trident, your topology can only guarantee that messages are processed at least once.</span></span> <span data-ttu-id="a9221-275">Det finns även andra skillnader, till exempel inbyggda komponenter som kan användas i stället för att skapa bultar.</span><span class="sxs-lookup"><span data-stu-id="a9221-275">There are also other differences, such as built-in components that can be used instead of creating bolts.</span></span> <span data-ttu-id="a9221-276">I själva verket ersättas bultar med mindre generisk komponenter, till exempel filter, projektioner och funktioner.</span><span class="sxs-lookup"><span data-stu-id="a9221-276">In fact, bolts are replaced by less-generic components, such as filters, projections, and functions.</span></span>

<span data-ttu-id="a9221-277">Trident program kan skapas med hjälp av Maven-projekt.</span><span class="sxs-lookup"><span data-stu-id="a9221-277">Trident applications can be created by using Maven projects.</span></span> <span data-ttu-id="a9221-278">Du använder hello samma grundläggande steg som visas tidigare i den här artikeln – endast hello koden är olika.</span><span class="sxs-lookup"><span data-stu-id="a9221-278">You use hello same basic steps as presented earlier in this article—only hello code is different.</span></span> <span data-ttu-id="a9221-279">Heller Trident kan inte (för närvarande) användas med hello som framework.</span><span class="sxs-lookup"><span data-stu-id="a9221-279">Trident also cannot (currently) be used with hello Flux framework.</span></span>

<span data-ttu-id="a9221-280">Mer information om Trident finns hello [Trident API översikt](http://storm.apache.org/documentation/Trident-API-Overview.html).</span><span class="sxs-lookup"><span data-stu-id="a9221-280">For more information about Trident, see hello [Trident API Overview](http://storm.apache.org/documentation/Trident-API-Overview.html).</span></span>

<span data-ttu-id="a9221-281">Ett exempel på en Trident-program finns i [Twitter trender avsnitt med Apache Storm på HDInsight](hdinsight-storm-twitter-trending.md).</span><span class="sxs-lookup"><span data-stu-id="a9221-281">For an example of a Trident application, see [Twitter trending topics with Apache Storm on HDInsight](hdinsight-storm-twitter-trending.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a9221-282">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a9221-282">Next Steps</span></span>

<span data-ttu-id="a9221-283">Du har lärt dig hur toocreate en Storm-topologi med Java.</span><span class="sxs-lookup"><span data-stu-id="a9221-283">You have learned how toocreate a Storm topology by using Java.</span></span> <span data-ttu-id="a9221-284">Nu Lär dig hur du:</span><span class="sxs-lookup"><span data-stu-id="a9221-284">Now learn how to:</span></span>

* [<span data-ttu-id="a9221-285">Distribuera och hantera Apache Storm-topologier på HDInsight</span><span class="sxs-lookup"><span data-stu-id="a9221-285">Deploy and manage Apache Storm topologies on HDInsight</span></span>](hdinsight-storm-deploy-monitor-topology.md)

* [<span data-ttu-id="a9221-286">Utveckla C#-topologier för Apache Storm på HDInsight med Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9221-286">Develop C# topologies for Apache Storm on HDInsight using Visual Studio</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)

<span data-ttu-id="a9221-287">Du hittar fler exempel på Storm-topologier genom att besöka [exempeltopologier för Storm på HDInsight](hdinsight-storm-example-topology.md).</span><span class="sxs-lookup"><span data-stu-id="a9221-287">You can find more example Storm topologies by visiting [Example topologies for Storm on HDInsight](hdinsight-storm-example-topology.md).</span></span>

