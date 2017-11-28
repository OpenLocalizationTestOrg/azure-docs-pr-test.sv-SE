---
title: aaaQuery Hive via hello JDBC driver - Azure HDInsight | Microsoft Docs
description: "Använd hello JDBC-drivrutinen från en Java-program toosubmit Hive-frågor tooHadoop på HDInsight. Ansluta och programmässigt hello SQuirrel SQL-klient."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 928f8d2a-684d-48cb-894c-11c59a5599ae
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/14/2017
ms.author: larryfr
ms.openlocfilehash: 93178da3b8d497faa4c788e91dba89c4e45d3fff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="query-hive-through-hello-jdbc-driver-in-hdinsight"></a><span data-ttu-id="e4f04-104">Fråga Hive via hello JDBC-drivrutinen i HDInsight</span><span class="sxs-lookup"><span data-stu-id="e4f04-104">Query Hive through hello JDBC driver in HDInsight</span></span>

[!INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

<span data-ttu-id="e4f04-105">Lär dig hur toouse hello JDBC-drivrutinen från en Java-program toosubmit Hive frågar tooHadoop i Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="e4f04-105">Learn how toouse hello JDBC driver from a Java application toosubmit Hive queries tooHadoop in Azure HDInsight.</span></span> <span data-ttu-id="e4f04-106">hello informationen i det här dokumentet visar hur tooconnect och programmässigt från hello SQuirrel SQL-klienten.</span><span class="sxs-lookup"><span data-stu-id="e4f04-106">hello information in this document demonstrates how tooconnect programmatically and from hello SQuirrel SQL client.</span></span>

<span data-ttu-id="e4f04-107">Mer information om hello Hive JDBC-gränssnittet finns [HiveJDBCInterface](https://cwiki.apache.org/confluence/display/Hive/HiveJDBCInterface).</span><span class="sxs-lookup"><span data-stu-id="e4f04-107">For more information on hello Hive JDBC Interface, see [HiveJDBCInterface](https://cwiki.apache.org/confluence/display/Hive/HiveJDBCInterface).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e4f04-108">Krav</span><span class="sxs-lookup"><span data-stu-id="e4f04-108">Prerequisites</span></span>

* <span data-ttu-id="e4f04-109">En Hadoop på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e4f04-109">A Hadoop on HDInsight cluster.</span></span> <span data-ttu-id="e4f04-110">Linux-baserade eller Windows-baserade kluster fungerar.</span><span class="sxs-lookup"><span data-stu-id="e4f04-110">Either Linux-based or Windows-based clusters work.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="e4f04-111">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="e4f04-111">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="e4f04-112">Mer information finns i [HDInsight 3.3 pensionering](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="e4f04-112">For more information, see [HDInsight 3.3 retirement](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="e4f04-113">[SQuirreL SQL](http://squirrel-sql.sourceforge.net/).</span><span class="sxs-lookup"><span data-stu-id="e4f04-113">[SQuirreL SQL](http://squirrel-sql.sourceforge.net/).</span></span> <span data-ttu-id="e4f04-114">SQuirreL är en JDBC-klientprogram.</span><span class="sxs-lookup"><span data-stu-id="e4f04-114">SQuirreL is a JDBC client application.</span></span>

* <span data-ttu-id="e4f04-115">Hej [Java Developer Kit (JDK) version 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) eller högre.</span><span class="sxs-lookup"><span data-stu-id="e4f04-115">hello [Java Developer Kit (JDK) version 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) or higher.</span></span>

* <span data-ttu-id="e4f04-116">[Apache Maven](https://maven.apache.org).</span><span class="sxs-lookup"><span data-stu-id="e4f04-116">[Apache Maven](https://maven.apache.org).</span></span> <span data-ttu-id="e4f04-117">Maven är ett projekt bygg system för Java-projekt som används av hello projekt som är associerat med den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="e4f04-117">Maven is a project build system for Java projects that is used by hello project associated with this article.</span></span>

## <a name="jdbc-connection-string"></a><span data-ttu-id="e4f04-118">JDBC-anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="e4f04-118">JDBC connection string</span></span>

<span data-ttu-id="e4f04-119">JDBC-anslutningar tooan HDInsight-kluster i Azure görs över 443 och hello trafik skyddas med SSL.</span><span class="sxs-lookup"><span data-stu-id="e4f04-119">JDBC connections tooan HDInsight cluster on Azure are made over 443, and hello traffic is secured using SSL.</span></span> <span data-ttu-id="e4f04-120">hello offentliga gateway hello kluster hamnar bakom omdirigerar hello trafik toohello port HiveServer2 faktiskt lyssnar på.</span><span class="sxs-lookup"><span data-stu-id="e4f04-120">hello public gateway that hello clusters sit behind redirects hello traffic toohello port that HiveServer2 is actually listening on.</span></span> <span data-ttu-id="e4f04-121">hello följande är ett exempel anslutningssträng:</span><span class="sxs-lookup"><span data-stu-id="e4f04-121">hello following is an example connection string:</span></span>

    jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2

<span data-ttu-id="e4f04-122">Ersätt `CLUSTERNAME` med hello namnet på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e4f04-122">Replace `CLUSTERNAME` with hello name of your HDInsight cluster.</span></span>

## <a name="authentication"></a><span data-ttu-id="e4f04-123">Autentisering</span><span class="sxs-lookup"><span data-stu-id="e4f04-123">Authentication</span></span>

<span data-ttu-id="e4f04-124">När anslutningen har upprättats hello måste du använda hello HDInsight-kluster admin namn och lösenord tooauthenticate toohello klustret gateway.</span><span class="sxs-lookup"><span data-stu-id="e4f04-124">When establishing hello connection, you must use hello HDInsight cluster admin name and password tooauthenticate toohello cluster gateway.</span></span> <span data-ttu-id="e4f04-125">När du ansluter från JDBC-klienter, till exempel SQuirreL SQL måste du ange hello admin namn och lösenord i klientinställningarna.</span><span class="sxs-lookup"><span data-stu-id="e4f04-125">When connecting from JDBC clients such as SQuirreL SQL, you must enter hello admin name and password in client settings.</span></span>

<span data-ttu-id="e4f04-126">Från ett Java-program, måste du använda hello namn och lösenord när en anslutning upprättas.</span><span class="sxs-lookup"><span data-stu-id="e4f04-126">From a Java application, you must use hello name and password when establishing a connection.</span></span> <span data-ttu-id="e4f04-127">Till exempel öppnas hello följande Java-kod en ny anslutning med hello anslutningssträngen, admin namn och lösenord:</span><span class="sxs-lookup"><span data-stu-id="e4f04-127">For example, hello following Java code opens a new connection using hello connection string, admin name, and password:</span></span>

```java
DriverManager.getConnection(connectionString,clusterAdmin,clusterPassword);
```

## <a name="connect-with-squirrel-sql-client"></a><span data-ttu-id="e4f04-128">Ansluta med SQuirreL SQL-klienten</span><span class="sxs-lookup"><span data-stu-id="e4f04-128">Connect with SQuirreL SQL client</span></span>

<span data-ttu-id="e4f04-129">SQuirreL SQL är en JDBC-klient som kan använda tooremotely kör Hive-frågor med ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e4f04-129">SQuirreL SQL is a JDBC client that can be used tooremotely run Hive queries with your HDInsight cluster.</span></span> <span data-ttu-id="e4f04-130">hello förutsätter följande steg att du redan har installerat SQuirreL SQL.</span><span class="sxs-lookup"><span data-stu-id="e4f04-130">hello following steps assume that you have already installed SQuirreL SQL.</span></span>

1. <span data-ttu-id="e4f04-131">Kopiera hello Hive JDBC-drivrutiner från ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e4f04-131">Copy hello Hive JDBC drivers from your HDInsight cluster.</span></span>

    * <span data-ttu-id="e4f04-132">För **Linux-baserat HDInsight**, Använd hello följande steg toodownload hello krävs jar-filer.</span><span class="sxs-lookup"><span data-stu-id="e4f04-132">For **Linux-based HDInsight**, use hello following steps toodownload hello required jar files.</span></span>

        1. <span data-ttu-id="e4f04-133">Skapa en katalog som innehåller hello-filer.</span><span class="sxs-lookup"><span data-stu-id="e4f04-133">Create a directory that contains hello files.</span></span> <span data-ttu-id="e4f04-134">Till exempel `mkdir hivedriver`.</span><span class="sxs-lookup"><span data-stu-id="e4f04-134">For example, `mkdir hivedriver`.</span></span>

        2. <span data-ttu-id="e4f04-135">Använd hello följande kommandon från en kommandorad toocopy hello filer från hello HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="e4f04-135">From a command line, use hello following commands toocopy hello files from hello HDInsight cluster:</span></span>

            ```bash
            scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/hive-jdbc*standalone.jar .
            scp USERNAME@CLUSTERNAME:/usr/hdp/current/hadoop-client/hadoop-common.jar .
            scp USERNAME@CLUSTERNAME:/usr/hdp/current/hadoop-client/hadoop-auth.jar .
            ```

            <span data-ttu-id="e4f04-136">Ersätt `USERNAME` med hello SSH användarkontonamnet för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="e4f04-136">Replace `USERNAME` with hello SSH user account name for hello cluster.</span></span> <span data-ttu-id="e4f04-137">Ersätt `CLUSTERNAME` med hello HDInsight-klustrets namn.</span><span class="sxs-lookup"><span data-stu-id="e4f04-137">Replace `CLUSTERNAME` with hello HDInsight cluster name.</span></span>

    * <span data-ttu-id="e4f04-138">För **Windows-baserade HDInsight**, Använd hello följande steg toodownload hello jar-filer.</span><span class="sxs-lookup"><span data-stu-id="e4f04-138">For **Windows-based HDInsight**, use hello following steps toodownload hello jar files.</span></span>

        1. <span data-ttu-id="e4f04-139">Välj ditt HDInsight-kluster från hello Azure-portalen, och välj sedan hello **fjärrskrivbord** ikon.</span><span class="sxs-lookup"><span data-stu-id="e4f04-139">From hello Azure portal, select your HDInsight cluster, and then select hello **Remote Desktop** icon.</span></span>

            ![Ikon för Remote Desktop](./media/hdinsight-connect-hive-jdbc-driver/remotedesktopicon.png)

        2. <span data-ttu-id="e4f04-141">Använd hello på hello fjärrskrivbord bladet **Anslut** knappen tooconnect toohello klustret.</span><span class="sxs-lookup"><span data-stu-id="e4f04-141">On hello Remote Desktop blade, use hello **Connect** button tooconnect toohello cluster.</span></span> <span data-ttu-id="e4f04-142">Om hello fjärrskrivbord inte är aktiverad använder hello formuläret tooprovide ett användarnamn och lösenord och väljer sedan **aktivera** tooenable Remote Desktop för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="e4f04-142">If hello Remote Desktop is not enabled, use hello form tooprovide a user name and password, then select **Enable** tooenable Remote Desktop for hello cluster.</span></span>

            ![Remote desktop bladet](./media/hdinsight-connect-hive-jdbc-driver/remotedesktopblade.png)

            <span data-ttu-id="e4f04-144">När du har valt **Anslut**, hämtas en RDP-fil.</span><span class="sxs-lookup"><span data-stu-id="e4f04-144">After selecting **Connect**, a .rdp file is downloaded.</span></span> <span data-ttu-id="e4f04-145">Använd den här filen toolaunch hello fjärrskrivbordsklienten.</span><span class="sxs-lookup"><span data-stu-id="e4f04-145">Use this file toolaunch hello Remote Desktop client.</span></span> <span data-ttu-id="e4f04-146">När du uppmanas, Använd hello användarnamn och lösenord som du angav för åtkomst via fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="e4f04-146">When prompted, use hello user name and password you entered for Remote Desktop access.</span></span>

        3. <span data-ttu-id="e4f04-147">När du är ansluten, kopiera följande filer från hello Remote Desktop session tooyour lokal dator hello.</span><span class="sxs-lookup"><span data-stu-id="e4f04-147">Once connected, copy hello following files from hello Remote Desktop session tooyour local machine.</span></span> <span data-ttu-id="e4f04-148">Placera dem i en lokal katalog med namnet `hivedriver`.</span><span class="sxs-lookup"><span data-stu-id="e4f04-148">Put them in a local directory named `hivedriver`.</span></span>

            * <span data-ttu-id="e4f04-149">C:\apps\dist\hive-0.14.0.2.2.9.1-7\lib\hive-JDBC-0.14.0.2.2.9.1-7-standalone.JAR</span><span class="sxs-lookup"><span data-stu-id="e4f04-149">C:\apps\dist\hive-0.14.0.2.2.9.1-7\lib\hive-jdbc-0.14.0.2.2.9.1-7-standalone.jar</span></span>
            * <span data-ttu-id="e4f04-150">C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\hadoop-Common-2.6.0.2.2.9.1-7.JAR</span><span class="sxs-lookup"><span data-stu-id="e4f04-150">C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\hadoop-common-2.6.0.2.2.9.1-7.jar</span></span>
            * <span data-ttu-id="e4f04-151">C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\lib\hadoop-auth-2.6.0.2.2.9.1-7.JAR</span><span class="sxs-lookup"><span data-stu-id="e4f04-151">C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\lib\hadoop-auth-2.6.0.2.2.9.1-7.jar</span></span>

            > [!NOTE]
            > <span data-ttu-id="e4f04-152">hello version siffror som ingår i hello sökvägar och filnamn kan vara olika för klustret.</span><span class="sxs-lookup"><span data-stu-id="e4f04-152">hello version numbers included in hello paths and file names may be different for your cluster.</span></span>

        4. <span data-ttu-id="e4f04-153">Koppla från hello fjärrskrivbordssession när du är klar med att kopiera hello-filer.</span><span class="sxs-lookup"><span data-stu-id="e4f04-153">Disconnect hello Remote Desktop session once you have finished copying hello files.</span></span>

2. <span data-ttu-id="e4f04-154">Starta hello SQuirreL SQL-programmet.</span><span class="sxs-lookup"><span data-stu-id="e4f04-154">Start hello SQuirreL SQL application.</span></span> <span data-ttu-id="e4f04-155">Välj hello till vänster i fönstret hello **drivrutiner**.</span><span class="sxs-lookup"><span data-stu-id="e4f04-155">From hello left of hello window, select **Drivers**.</span></span>

    ![Fliken drivrutiner på hello till vänster i fönstret hello](./media/hdinsight-connect-hive-jdbc-driver/squirreldrivers.png)

3. <span data-ttu-id="e4f04-157">Från hello ikoner hello överst i hello **drivrutiner** dialogrutan, Välj hello  **+**  ikonen toocreate en drivrutin.</span><span class="sxs-lookup"><span data-stu-id="e4f04-157">From hello icons at hello top of hello **Drivers** dialog, select hello **+** icon toocreate a driver.</span></span>

    ![Drivrutiner ikoner](./media/hdinsight-connect-hive-jdbc-driver/driversicons.png)

4. <span data-ttu-id="e4f04-159">Lägg till hello följande information i dialogrutan Lägg till drivrutin hello:</span><span class="sxs-lookup"><span data-stu-id="e4f04-159">In hello Add Driver dialog, add hello following information:</span></span>

    * <span data-ttu-id="e4f04-160">**Namnet**: Hive</span><span class="sxs-lookup"><span data-stu-id="e4f04-160">**Name**: Hive</span></span>
    * <span data-ttu-id="e4f04-161">**Exempel-URL**:`jdbc:hive2://localhost:443/default;transportMode=http;ssl=true;httpPath=/hive2`</span><span class="sxs-lookup"><span data-stu-id="e4f04-161">**Example URL**: `jdbc:hive2://localhost:443/default;transportMode=http;ssl=true;httpPath=/hive2`</span></span>
    * <span data-ttu-id="e4f04-162">**Extra klassökvägen**: Använd hello Lägg till knappen tooadd hello jar-filer hämtade tidigare</span><span class="sxs-lookup"><span data-stu-id="e4f04-162">**Extra Class Path**: Use hello Add button tooadd hello jar files downloaded earlier</span></span>
    * <span data-ttu-id="e4f04-163">**Klassnamn**: org.apache.hive.jdbc.HiveDriver</span><span class="sxs-lookup"><span data-stu-id="e4f04-163">**Class Name**: org.apache.hive.jdbc.HiveDriver</span></span>

   ![Lägg till dialogrutan för drivrutin](./media/hdinsight-connect-hive-jdbc-driver/adddriver.png)

   <span data-ttu-id="e4f04-165">Klicka på **OK** toosave inställningarna.</span><span class="sxs-lookup"><span data-stu-id="e4f04-165">Click **OK** toosave these settings.</span></span>

5. <span data-ttu-id="e4f04-166">Hello till vänster i hello SQuirreL SQL-fönstret, Välj **alias**.</span><span class="sxs-lookup"><span data-stu-id="e4f04-166">On hello left of hello SQuirreL SQL window, select **Aliases**.</span></span> <span data-ttu-id="e4f04-167">Klicka på hello  **+**  ikonen toocreate ett alias för anslutningen.</span><span class="sxs-lookup"><span data-stu-id="e4f04-167">Then click hello **+** icon toocreate a connection alias.</span></span>

    ![Lägg till nytt alias](./media/hdinsight-connect-hive-jdbc-driver/aliases.png)

6. <span data-ttu-id="e4f04-169">Använd hello följande värden för hello **lägga till Alias** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e4f04-169">Use hello following values for hello **Add Alias** dialog.</span></span>

    * <span data-ttu-id="e4f04-170">**Namnet**: Hive i HDInsight</span><span class="sxs-lookup"><span data-stu-id="e4f04-170">**Name**: Hive on HDInsight</span></span>

    * <span data-ttu-id="e4f04-171">**Drivrutinen**: Använd hello listrutan tooselect hello **Hive** drivrutin</span><span class="sxs-lookup"><span data-stu-id="e4f04-171">**Driver**: Use hello dropdown tooselect hello **Hive** driver</span></span>

    * <span data-ttu-id="e4f04-172">**URL: en**: jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2</span><span class="sxs-lookup"><span data-stu-id="e4f04-172">**URL**: jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2</span></span>

        <span data-ttu-id="e4f04-173">Ersätt **KLUSTERNAMN** med hello namnet på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e4f04-173">Replace **CLUSTERNAME** with hello name of your HDInsight cluster.</span></span>

    * <span data-ttu-id="e4f04-174">**Användarnamnet**: hello klustrets inloggningsnamn konto för ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e4f04-174">**User Name**: hello cluster login account name for your HDInsight cluster.</span></span> <span data-ttu-id="e4f04-175">hello standardvärdet är `admin`.</span><span class="sxs-lookup"><span data-stu-id="e4f04-175">hello default is `admin`.</span></span>

    * <span data-ttu-id="e4f04-176">**Lösenordet**: hello lösenordet för hello inloggningskonto för klustret.</span><span class="sxs-lookup"><span data-stu-id="e4f04-176">**Password**: hello password for hello cluster login account.</span></span>

 ![alias dialogrutan Lägg till](./media/hdinsight-connect-hive-jdbc-driver/addalias.png)

    <span data-ttu-id="e4f04-178">Använd hello **Test** knappen tooverify som hello anslutning fungerar.</span><span class="sxs-lookup"><span data-stu-id="e4f04-178">Use hello **Test** button tooverify that hello connection works.</span></span> <span data-ttu-id="e4f04-179">När **ansluta till: Hive i HDInsight** dialogrutan visas, väljer du **Anslut** tooperform hello test.</span><span class="sxs-lookup"><span data-stu-id="e4f04-179">When **Connect to: Hive on HDInsight** dialog appears, select **Connect** tooperform hello test.</span></span> <span data-ttu-id="e4f04-180">Om hello testet lyckas visas en **lyckad anslutning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e4f04-180">If hello test succeeds, you see a **Connection successful** dialog.</span></span>

    <span data-ttu-id="e4f04-181">toosave hello anslutning alias, använda hello **Ok** knappen längst ned hello hello **lägga till Alias** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e4f04-181">toosave hello connection alias, use hello **Ok** button at hello bottom of hello **Add Alias** dialog.</span></span>

7. <span data-ttu-id="e4f04-182">Från hello **ansluta till** listrutan överst hello i SQuirreL SQL, Välj **Hive i HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="e4f04-182">From hello **Connect to** dropdown at hello top of SQuirreL SQL, select **Hive on HDInsight**.</span></span> <span data-ttu-id="e4f04-183">När du uppmanas, Välj **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="e4f04-183">When prompted, select **Connect**.</span></span>

    ![anslutningsdialogrutan](./media/hdinsight-connect-hive-jdbc-driver/connect.png)

8. <span data-ttu-id="e4f04-185">När du är ansluten, ange hello följande fråga i dialogrutan för hello SQL-fråga och välj sedan hello **kör** ikon.</span><span class="sxs-lookup"><span data-stu-id="e4f04-185">Once connected, enter hello following query into hello SQL query dialog, and then select hello **Run** icon.</span></span> <span data-ttu-id="e4f04-186">hello resultatområdet ska visa hello resultaten av hello-frågan.</span><span class="sxs-lookup"><span data-stu-id="e4f04-186">hello results area should show hello results of hello query.</span></span>

        select * from hivesampletable limit 10;

    ![dialogrutan för SQL-fråga, inklusive resultat](./media/hdinsight-connect-hive-jdbc-driver/sqlquery.png)

## <a name="connect-from-an-example-java-application"></a><span data-ttu-id="e4f04-188">Ansluta från ett exempel Java-program</span><span class="sxs-lookup"><span data-stu-id="e4f04-188">Connect from an example Java application</span></span>

<span data-ttu-id="e4f04-189">Ett exempel på hur en Java-klienten tooquery Hive i HDInsight finns på [https://github.com/Azure-Samples/hdinsight-java-hive-jdbc](https://github.com/Azure-Samples/hdinsight-java-hive-jdbc).</span><span class="sxs-lookup"><span data-stu-id="e4f04-189">An example of using a Java client tooquery Hive on HDInsight is available at [https://github.com/Azure-Samples/hdinsight-java-hive-jdbc](https://github.com/Azure-Samples/hdinsight-java-hive-jdbc).</span></span> <span data-ttu-id="e4f04-190">Följ anvisningarna för hello i hello databasen toobuild och kör hello exempel.</span><span class="sxs-lookup"><span data-stu-id="e4f04-190">Follow hello instructions in hello repository toobuild and run hello sample.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="e4f04-191">Felsökning</span><span class="sxs-lookup"><span data-stu-id="e4f04-191">Troubleshooting</span></span>

### <a name="unexpected-error-occurred-attempting-tooopen-an-sql-connection"></a><span data-ttu-id="e4f04-192">Oväntat fel uppstod när tooopen en SQL-anslutning</span><span class="sxs-lookup"><span data-stu-id="e4f04-192">Unexpected Error occurred attempting tooopen an SQL connection</span></span>

<span data-ttu-id="e4f04-193">**Symptom**: när du ansluter tooan HDInsight-kluster som är version 3.3 eller 3.4, kan ett felmeddelande som ett oväntat fel uppstod.</span><span class="sxs-lookup"><span data-stu-id="e4f04-193">**Symptoms**: When connecting tooan HDInsight cluster that is version 3.3 or 3.4, you may receive an error that an unexpected error occurred.</span></span> <span data-ttu-id="e4f04-194">hello stack-spårning för det här felet börjar med hello följande rader:</span><span class="sxs-lookup"><span data-stu-id="e4f04-194">hello stack trace for this error begins with hello following lines:</span></span>

```java
java.util.concurrent.ExecutionException: java.lang.RuntimeException: java.lang.NoSuchMethodError: org.apache.commons.codec.binary.Base64.<init>(I)V
at java.util.concurrent.FutureTas...(FutureTask.java:122)
at java.util.concurrent.FutureTask.get(FutureTask.java:206)
```

<span data-ttu-id="e4f04-195">**Orsak**: det här felet beror på ett matchningsfel för hello-versionen av hello commons codec.jar filen används av SQuirreL och hello en krävs av hello Hive JDBC-komponenter.</span><span class="sxs-lookup"><span data-stu-id="e4f04-195">**Cause**: This error is caused by a mismatch in hello version of hello commons-codec.jar file used by SQuirreL and hello one required by hello Hive JDBC components.</span></span>

<span data-ttu-id="e4f04-196">**Lösning**: toofix felet, Använd hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="e4f04-196">**Resolution**: toofix this error, use hello following steps:</span></span>

1. <span data-ttu-id="e4f04-197">Hämta hello commons codec jar-filen från ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e4f04-197">Download hello commons-codec jar file from your HDInsight cluster.</span></span>

        scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/commons-codec*.jar ./commons-codec.jar

2. <span data-ttu-id="e4f04-198">Avsluta SQuirreL och sedan gå toohello directory där SQuirreL är installerad på datorn.</span><span class="sxs-lookup"><span data-stu-id="e4f04-198">Exit SQuirreL, and then go toohello directory where SQuirreL is installed on your system.</span></span> <span data-ttu-id="e4f04-199">I hello SquirreL katalog under hello `lib` directory, Ersätt hello befintliga commons-codec.jar med hello en hämtas från hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e4f04-199">In hello SquirreL directory, under hello `lib` directory, replace hello existing commons-codec.jar with hello one downloaded from hello HDInsight cluster.</span></span>

3. <span data-ttu-id="e4f04-200">Starta om SQuirreL.</span><span class="sxs-lookup"><span data-stu-id="e4f04-200">Restart SQuirreL.</span></span> <span data-ttu-id="e4f04-201">hello fel bör inte längre inträffa när du ansluter tooHive på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="e4f04-201">hello error should no longer occur when connecting tooHive on HDInsight.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e4f04-202">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e4f04-202">Next steps</span></span>

<span data-ttu-id="e4f04-203">Nu när du har lärt dig hur länkar toouse JDBC toowork med Hive, Använd hello följande tooexplore andra sätt toowork med Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="e4f04-203">Now that you have learned how toouse JDBC toowork with Hive, use hello following links tooexplore other ways toowork with Azure HDInsight.</span></span>

* [<span data-ttu-id="e4f04-204">Ladda upp data tooHDInsight</span><span class="sxs-lookup"><span data-stu-id="e4f04-204">Upload data tooHDInsight</span></span>](hdinsight-upload-data.md)
* [<span data-ttu-id="e4f04-205">Använda Hive med HDInsight</span><span class="sxs-lookup"><span data-stu-id="e4f04-205">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="e4f04-206">Använda Pig med HDInsight</span><span class="sxs-lookup"><span data-stu-id="e4f04-206">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="e4f04-207">Använda MapReduce-jobb med HDInsight</span><span class="sxs-lookup"><span data-stu-id="e4f04-207">Use MapReduce jobs with HDInsight</span></span>](hdinsight-use-mapreduce.md)
