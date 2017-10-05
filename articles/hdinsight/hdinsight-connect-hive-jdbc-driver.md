---
title: "Fråga Hive genom JDBC driver - Azure HDInsight | Microsoft Docs"
description: "Använda JDBC-drivrutinen från ett Java-program för att skicka Hive-frågor till Hadoop på HDInsight. Ansluta programmässigt och SQuirrel SQL-klient."
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
ms.openlocfilehash: f2de58c2763b2ddd0926336164738929c477c4ae
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="query-hive-through-the-jdbc-driver-in-hdinsight"></a><span data-ttu-id="34317-104">Frågan Hive genom JDBC driver i HDInsight</span><span class="sxs-lookup"><span data-stu-id="34317-104">Query Hive through the JDBC driver in HDInsight</span></span>

[!INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

<span data-ttu-id="34317-105">Lär dig hur du använder JDBC driver från ett Java-program för att skicka Hive-frågor till Hadoop i Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="34317-105">Learn how to use the JDBC driver from a Java application to submit Hive queries to Hadoop in Azure HDInsight.</span></span> <span data-ttu-id="34317-106">Informationen i det här dokumentet beskrivs hur du ansluta programmässigt och SQuirrel SQL-klient.</span><span class="sxs-lookup"><span data-stu-id="34317-106">The information in this document demonstrates how to connect programmatically and from the SQuirrel SQL client.</span></span>

<span data-ttu-id="34317-107">Läs mer på gränssnittet Hive JDBC [HiveJDBCInterface](https://cwiki.apache.org/confluence/display/Hive/HiveJDBCInterface).</span><span class="sxs-lookup"><span data-stu-id="34317-107">For more information on the Hive JDBC Interface, see [HiveJDBCInterface](https://cwiki.apache.org/confluence/display/Hive/HiveJDBCInterface).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="34317-108">Krav</span><span class="sxs-lookup"><span data-stu-id="34317-108">Prerequisites</span></span>

* <span data-ttu-id="34317-109">En Hadoop på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="34317-109">A Hadoop on HDInsight cluster.</span></span> <span data-ttu-id="34317-110">Linux-baserade eller Windows-baserade kluster fungerar.</span><span class="sxs-lookup"><span data-stu-id="34317-110">Either Linux-based or Windows-based clusters work.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="34317-111">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="34317-111">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="34317-112">Mer information finns i [HDInsight 3.3 pensionering](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="34317-112">For more information, see [HDInsight 3.3 retirement](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="34317-113">[SQuirreL SQL](http://squirrel-sql.sourceforge.net/).</span><span class="sxs-lookup"><span data-stu-id="34317-113">[SQuirreL SQL](http://squirrel-sql.sourceforge.net/).</span></span> <span data-ttu-id="34317-114">SQuirreL är en JDBC-klientprogram.</span><span class="sxs-lookup"><span data-stu-id="34317-114">SQuirreL is a JDBC client application.</span></span>

* <span data-ttu-id="34317-115">Den [Java Developer Kit (JDK) version 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) eller högre.</span><span class="sxs-lookup"><span data-stu-id="34317-115">The [Java Developer Kit (JDK) version 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) or higher.</span></span>

* <span data-ttu-id="34317-116">[Apache Maven](https://maven.apache.org).</span><span class="sxs-lookup"><span data-stu-id="34317-116">[Apache Maven](https://maven.apache.org).</span></span> <span data-ttu-id="34317-117">Maven är ett projekt bygg system för Java-projekt som används av projektet som associeras med den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="34317-117">Maven is a project build system for Java projects that is used by the project associated with this article.</span></span>

## <a name="jdbc-connection-string"></a><span data-ttu-id="34317-118">JDBC-anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="34317-118">JDBC connection string</span></span>

<span data-ttu-id="34317-119">JDBC-anslutningar till ett HDInsight-kluster i Azure görs över 443 och trafiken skyddas med SSL.</span><span class="sxs-lookup"><span data-stu-id="34317-119">JDBC connections to an HDInsight cluster on Azure are made over 443, and the traffic is secured using SSL.</span></span> <span data-ttu-id="34317-120">Den offentliga gateway som kluster hamnar bakom omdirigerar trafik till den port som faktiskt HiveServer2 lyssnar på.</span><span class="sxs-lookup"><span data-stu-id="34317-120">The public gateway that the clusters sit behind redirects the traffic to the port that HiveServer2 is actually listening on.</span></span> <span data-ttu-id="34317-121">Följande är ett exempel anslutningssträngen:</span><span class="sxs-lookup"><span data-stu-id="34317-121">The following is an example connection string:</span></span>

    jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2

<span data-ttu-id="34317-122">Ersätt `CLUSTERNAME` med namnet på HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="34317-122">Replace `CLUSTERNAME` with the name of your HDInsight cluster.</span></span>

## <a name="authentication"></a><span data-ttu-id="34317-123">Autentisering</span><span class="sxs-lookup"><span data-stu-id="34317-123">Authentication</span></span>

<span data-ttu-id="34317-124">När anslutningen upprättas, måste du använda HDInsight-kluster admin namn och lösenord för att autentisera till kluster-gateway.</span><span class="sxs-lookup"><span data-stu-id="34317-124">When establishing the connection, you must use the HDInsight cluster admin name and password to authenticate to the cluster gateway.</span></span> <span data-ttu-id="34317-125">När du ansluter från JDBC-klienter, till exempel SQuirreL SQL måste du ange admin namn och lösenord i klientinställningarna.</span><span class="sxs-lookup"><span data-stu-id="34317-125">When connecting from JDBC clients such as SQuirreL SQL, you must enter the admin name and password in client settings.</span></span>

<span data-ttu-id="34317-126">Från ett Java-program, måste du använda namn och lösenord när en anslutning upprättas.</span><span class="sxs-lookup"><span data-stu-id="34317-126">From a Java application, you must use the name and password when establishing a connection.</span></span> <span data-ttu-id="34317-127">Till exempel öppnas följande Java-kod en ny anslutning med anslutningssträngen, admin namn och lösenord:</span><span class="sxs-lookup"><span data-stu-id="34317-127">For example, the following Java code opens a new connection using the connection string, admin name, and password:</span></span>

```java
DriverManager.getConnection(connectionString,clusterAdmin,clusterPassword);
```

## <a name="connect-with-squirrel-sql-client"></a><span data-ttu-id="34317-128">Ansluta med SQuirreL SQL-klienten</span><span class="sxs-lookup"><span data-stu-id="34317-128">Connect with SQuirreL SQL client</span></span>

<span data-ttu-id="34317-129">SQuirreL SQL är en JDBC-klient som kan användas för att köra Hive-frågor via fjärranslutning med ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="34317-129">SQuirreL SQL is a JDBC client that can be used to remotely run Hive queries with your HDInsight cluster.</span></span> <span data-ttu-id="34317-130">Följande steg förutsätter att du redan har installerat SQuirreL SQL.</span><span class="sxs-lookup"><span data-stu-id="34317-130">The following steps assume that you have already installed SQuirreL SQL.</span></span>

1. <span data-ttu-id="34317-131">Kopiera Hive JDBC-drivrutiner från ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="34317-131">Copy the Hive JDBC drivers from your HDInsight cluster.</span></span>

    * <span data-ttu-id="34317-132">För **Linux-baserat HDInsight**, Använd följande steg för att hämta nödvändiga jar-filer.</span><span class="sxs-lookup"><span data-stu-id="34317-132">For **Linux-based HDInsight**, use the following steps to download the required jar files.</span></span>

        1. <span data-ttu-id="34317-133">Skapa en katalog som innehåller filerna.</span><span class="sxs-lookup"><span data-stu-id="34317-133">Create a directory that contains the files.</span></span> <span data-ttu-id="34317-134">Till exempel `mkdir hivedriver`.</span><span class="sxs-lookup"><span data-stu-id="34317-134">For example, `mkdir hivedriver`.</span></span>

        2. <span data-ttu-id="34317-135">Använd följande kommandon för att kopiera filer från HDInsight-kluster från en kommandorad:</span><span class="sxs-lookup"><span data-stu-id="34317-135">From a command line, use the following commands to copy the files from the HDInsight cluster:</span></span>

            ```bash
            scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/hive-jdbc*standalone.jar .
            scp USERNAME@CLUSTERNAME:/usr/hdp/current/hadoop-client/hadoop-common.jar .
            scp USERNAME@CLUSTERNAME:/usr/hdp/current/hadoop-client/hadoop-auth.jar .
            ```

            <span data-ttu-id="34317-136">Ersätt `USERNAME` med SSH användarkontonamnet för klustret.</span><span class="sxs-lookup"><span data-stu-id="34317-136">Replace `USERNAME` with the SSH user account name for the cluster.</span></span> <span data-ttu-id="34317-137">Ersätt `CLUSTERNAME` med HDInsight-klustrets namn.</span><span class="sxs-lookup"><span data-stu-id="34317-137">Replace `CLUSTERNAME` with the HDInsight cluster name.</span></span>

    * <span data-ttu-id="34317-138">För **Windows-baserade HDInsight**, Använd följande steg för att hämta jar-filer.</span><span class="sxs-lookup"><span data-stu-id="34317-138">For **Windows-based HDInsight**, use the following steps to download the jar files.</span></span>

        1. <span data-ttu-id="34317-139">Välj ditt HDInsight-kluster från Azure-portalen och välj sedan den **fjärrskrivbord** ikon.</span><span class="sxs-lookup"><span data-stu-id="34317-139">From the Azure portal, select your HDInsight cluster, and then select the **Remote Desktop** icon.</span></span>

            ![Ikon för Remote Desktop](./media/hdinsight-connect-hive-jdbc-driver/remotedesktopicon.png)

        2. <span data-ttu-id="34317-141">I bladet fjärrskrivbord kan använda den **Anslut** för att ansluta till klustret.</span><span class="sxs-lookup"><span data-stu-id="34317-141">On the Remote Desktop blade, use the **Connect** button to connect to the cluster.</span></span> <span data-ttu-id="34317-142">Om fjärrdatorn inte är aktiverad, Använd formuläret för att ange ett användarnamn och lösenord och väljer sedan **aktivera** att aktivera Remote Desktop för klustret.</span><span class="sxs-lookup"><span data-stu-id="34317-142">If the Remote Desktop is not enabled, use the form to provide a user name and password, then select **Enable** to enable Remote Desktop for the cluster.</span></span>

            ![Remote desktop bladet](./media/hdinsight-connect-hive-jdbc-driver/remotedesktopblade.png)

            <span data-ttu-id="34317-144">När du har valt **Anslut**, hämtas en RDP-fil.</span><span class="sxs-lookup"><span data-stu-id="34317-144">After selecting **Connect**, a .rdp file is downloaded.</span></span> <span data-ttu-id="34317-145">Använd den här filen för att starta fjärrskrivbordsklienten.</span><span class="sxs-lookup"><span data-stu-id="34317-145">Use this file to launch the Remote Desktop client.</span></span> <span data-ttu-id="34317-146">När du uppmanas, använder du användarnamnet och lösenordet du angav för åtkomst via fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="34317-146">When prompted, use the user name and password you entered for Remote Desktop access.</span></span>

        3. <span data-ttu-id="34317-147">När du är ansluten, kopiera följande filer från sessionen för fjärrskrivbord på din lokala dator.</span><span class="sxs-lookup"><span data-stu-id="34317-147">Once connected, copy the following files from the Remote Desktop session to your local machine.</span></span> <span data-ttu-id="34317-148">Placera dem i en lokal katalog med namnet `hivedriver`.</span><span class="sxs-lookup"><span data-stu-id="34317-148">Put them in a local directory named `hivedriver`.</span></span>

            * <span data-ttu-id="34317-149">C:\apps\dist\hive-0.14.0.2.2.9.1-7\lib\hive-JDBC-0.14.0.2.2.9.1-7-standalone.JAR</span><span class="sxs-lookup"><span data-stu-id="34317-149">C:\apps\dist\hive-0.14.0.2.2.9.1-7\lib\hive-jdbc-0.14.0.2.2.9.1-7-standalone.jar</span></span>
            * <span data-ttu-id="34317-150">C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\hadoop-Common-2.6.0.2.2.9.1-7.JAR</span><span class="sxs-lookup"><span data-stu-id="34317-150">C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\hadoop-common-2.6.0.2.2.9.1-7.jar</span></span>
            * <span data-ttu-id="34317-151">C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\lib\hadoop-auth-2.6.0.2.2.9.1-7.JAR</span><span class="sxs-lookup"><span data-stu-id="34317-151">C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\lib\hadoop-auth-2.6.0.2.2.9.1-7.jar</span></span>

            > [!NOTE]
            > <span data-ttu-id="34317-152">Versionsnummer som ingår i sökvägar och filnamn kan vara olika för klustret.</span><span class="sxs-lookup"><span data-stu-id="34317-152">The version numbers included in the paths and file names may be different for your cluster.</span></span>

        4. <span data-ttu-id="34317-153">Koppla från fjärrskrivbordssession när du har slutfört kopieringen av filer.</span><span class="sxs-lookup"><span data-stu-id="34317-153">Disconnect the Remote Desktop session once you have finished copying the files.</span></span>

2. <span data-ttu-id="34317-154">Starta SQuirreL SQL-programmet.</span><span class="sxs-lookup"><span data-stu-id="34317-154">Start the SQuirreL SQL application.</span></span> <span data-ttu-id="34317-155">Till vänster i fönstret Välj **drivrutiner**.</span><span class="sxs-lookup"><span data-stu-id="34317-155">From the left of the window, select **Drivers**.</span></span>

    ![Fliken drivrutiner till vänster i fönstret](./media/hdinsight-connect-hive-jdbc-driver/squirreldrivers.png)

3. <span data-ttu-id="34317-157">Från ikonerna längst upp i den **drivrutiner** markerar den  **+**  ikon för att skapa en drivrutin.</span><span class="sxs-lookup"><span data-stu-id="34317-157">From the icons at the top of the **Drivers** dialog, select the **+** icon to create a driver.</span></span>

    ![Drivrutiner ikoner](./media/hdinsight-connect-hive-jdbc-driver/driversicons.png)

4. <span data-ttu-id="34317-159">Lägg till följande information i dialogrutan Lägg till drivrutin:</span><span class="sxs-lookup"><span data-stu-id="34317-159">In the Add Driver dialog, add the following information:</span></span>

    * <span data-ttu-id="34317-160">**Namnet**: Hive</span><span class="sxs-lookup"><span data-stu-id="34317-160">**Name**: Hive</span></span>
    * <span data-ttu-id="34317-161">**Exempel-URL**:`jdbc:hive2://localhost:443/default;transportMode=http;ssl=true;httpPath=/hive2`</span><span class="sxs-lookup"><span data-stu-id="34317-161">**Example URL**: `jdbc:hive2://localhost:443/default;transportMode=http;ssl=true;httpPath=/hive2`</span></span>
    * <span data-ttu-id="34317-162">**Extra klassökvägen**: Använd knappen Lägg till för att lägga till jar-filer tidigare</span><span class="sxs-lookup"><span data-stu-id="34317-162">**Extra Class Path**: Use the Add button to add the jar files downloaded earlier</span></span>
    * <span data-ttu-id="34317-163">**Klassnamn**: org.apache.hive.jdbc.HiveDriver</span><span class="sxs-lookup"><span data-stu-id="34317-163">**Class Name**: org.apache.hive.jdbc.HiveDriver</span></span>

   ![Lägg till dialogrutan för drivrutin](./media/hdinsight-connect-hive-jdbc-driver/adddriver.png)

   <span data-ttu-id="34317-165">Klicka på **OK** dessa inställningar ska sparas.</span><span class="sxs-lookup"><span data-stu-id="34317-165">Click **OK** to save these settings.</span></span>

5. <span data-ttu-id="34317-166">Till vänster i fönstret SQuirreL SQL markerar **alias**.</span><span class="sxs-lookup"><span data-stu-id="34317-166">On the left of the SQuirreL SQL window, select **Aliases**.</span></span> <span data-ttu-id="34317-167">Klicka på den  **+**  ikon för att skapa ett alias för anslutningen.</span><span class="sxs-lookup"><span data-stu-id="34317-167">Then click the **+** icon to create a connection alias.</span></span>

    ![Lägg till nytt alias](./media/hdinsight-connect-hive-jdbc-driver/aliases.png)

6. <span data-ttu-id="34317-169">Använd följande värden för den **lägga till Alias** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="34317-169">Use the following values for the **Add Alias** dialog.</span></span>

    * <span data-ttu-id="34317-170">**Namnet**: Hive i HDInsight</span><span class="sxs-lookup"><span data-stu-id="34317-170">**Name**: Hive on HDInsight</span></span>

    * <span data-ttu-id="34317-171">**Drivrutinen**: Använd listrutan för att välja den **Hive** drivrutin</span><span class="sxs-lookup"><span data-stu-id="34317-171">**Driver**: Use the dropdown to select the **Hive** driver</span></span>

    * <span data-ttu-id="34317-172">**URL: en**: jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2</span><span class="sxs-lookup"><span data-stu-id="34317-172">**URL**: jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2</span></span>

        <span data-ttu-id="34317-173">Ersätt **KLUSTERNAMN** med namnet på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="34317-173">Replace **CLUSTERNAME** with the name of your HDInsight cluster.</span></span>

    * <span data-ttu-id="34317-174">**Användarnamnet**: klustret konto inloggningsnamnet för ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="34317-174">**User Name**: The cluster login account name for your HDInsight cluster.</span></span> <span data-ttu-id="34317-175">Standardvärdet är `admin`.</span><span class="sxs-lookup"><span data-stu-id="34317-175">The default is `admin`.</span></span>

    * <span data-ttu-id="34317-176">**Lösenordet**: lösenordet för inloggningskontot för klustret.</span><span class="sxs-lookup"><span data-stu-id="34317-176">**Password**: The password for the cluster login account.</span></span>

 ![alias dialogrutan Lägg till](./media/hdinsight-connect-hive-jdbc-driver/addalias.png)

    <span data-ttu-id="34317-178">Använd den **Test** för att kontrollera att anslutningen fungerar.</span><span class="sxs-lookup"><span data-stu-id="34317-178">Use the **Test** button to verify that the connection works.</span></span> <span data-ttu-id="34317-179">När **ansluta till: Hive i HDInsight** dialogrutan visas, väljer du **Anslut** att utföra testet.</span><span class="sxs-lookup"><span data-stu-id="34317-179">When **Connect to: Hive on HDInsight** dialog appears, select **Connect** to perform the test.</span></span> <span data-ttu-id="34317-180">Om testet lyckas, ser du en **lyckad anslutning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="34317-180">If the test succeeds, you see a **Connection successful** dialog.</span></span>

    <span data-ttu-id="34317-181">För att spara anslutningen alias, Använd den **Ok** knappen längst ned i den **lägga till Alias** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="34317-181">To save the connection alias, use the **Ok** button at the bottom of the **Add Alias** dialog.</span></span>

7. <span data-ttu-id="34317-182">Från den **ansluta till** listrutan överst i SQuirreL SQL, Välj **Hive i HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="34317-182">From the **Connect to** dropdown at the top of SQuirreL SQL, select **Hive on HDInsight**.</span></span> <span data-ttu-id="34317-183">När du uppmanas, Välj **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="34317-183">When prompted, select **Connect**.</span></span>

    ![anslutningsdialogrutan](./media/hdinsight-connect-hive-jdbc-driver/connect.png)

8. <span data-ttu-id="34317-185">När du är ansluten, ange följande fråga i dialogrutan för SQL-fråga och välj sedan den **kör** ikon.</span><span class="sxs-lookup"><span data-stu-id="34317-185">Once connected, enter the following query into the SQL query dialog, and then select the **Run** icon.</span></span> <span data-ttu-id="34317-186">Resultatområdet ska visa resultatet av frågan.</span><span class="sxs-lookup"><span data-stu-id="34317-186">The results area should show the results of the query.</span></span>

        select * from hivesampletable limit 10;

    ![dialogrutan för SQL-fråga, inklusive resultat](./media/hdinsight-connect-hive-jdbc-driver/sqlquery.png)

## <a name="connect-from-an-example-java-application"></a><span data-ttu-id="34317-188">Ansluta från ett exempel Java-program</span><span class="sxs-lookup"><span data-stu-id="34317-188">Connect from an example Java application</span></span>

<span data-ttu-id="34317-189">Ett exempel på hur en Java-klient för att fråga Hive i HDInsight finns på [https://github.com/Azure-Samples/hdinsight-java-hive-jdbc](https://github.com/Azure-Samples/hdinsight-java-hive-jdbc).</span><span class="sxs-lookup"><span data-stu-id="34317-189">An example of using a Java client to query Hive on HDInsight is available at [https://github.com/Azure-Samples/hdinsight-java-hive-jdbc](https://github.com/Azure-Samples/hdinsight-java-hive-jdbc).</span></span> <span data-ttu-id="34317-190">Följ instruktionerna i databasen för att skapa och köra exemplet.</span><span class="sxs-lookup"><span data-stu-id="34317-190">Follow the instructions in the repository to build and run the sample.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="34317-191">Felsökning</span><span class="sxs-lookup"><span data-stu-id="34317-191">Troubleshooting</span></span>

### <a name="unexpected-error-occurred-attempting-to-open-an-sql-connection"></a><span data-ttu-id="34317-192">Ett oväntat fel uppstod när att öppna en SQL-anslutning</span><span class="sxs-lookup"><span data-stu-id="34317-192">Unexpected Error occurred attempting to open an SQL connection</span></span>

<span data-ttu-id="34317-193">**Symptom**: vid anslutning till ett HDInsight-kluster som är version 3.3 eller 3.4, kan du få ett felmeddelande som ett oväntat fel uppstod.</span><span class="sxs-lookup"><span data-stu-id="34317-193">**Symptoms**: When connecting to an HDInsight cluster that is version 3.3 or 3.4, you may receive an error that an unexpected error occurred.</span></span> <span data-ttu-id="34317-194">Stack-spårning för det här felet börjar med följande rader:</span><span class="sxs-lookup"><span data-stu-id="34317-194">The stack trace for this error begins with the following lines:</span></span>

```java
java.util.concurrent.ExecutionException: java.lang.RuntimeException: java.lang.NoSuchMethodError: org.apache.commons.codec.binary.Base64.<init>(I)V
at java.util.concurrent.FutureTas...(FutureTask.java:122)
at java.util.concurrent.FutureTask.get(FutureTask.java:206)
```

<span data-ttu-id="34317-195">**Orsak**: det här felet beror på ett matchningsfel för versionen av filen commons codec.jar används av SQuirreL och den som krävs för komponenterna som Hive JDBC.</span><span class="sxs-lookup"><span data-stu-id="34317-195">**Cause**: This error is caused by a mismatch in the version of the commons-codec.jar file used by SQuirreL and the one required by the Hive JDBC components.</span></span>

<span data-ttu-id="34317-196">**Lösning**: åtgärda felet genom att använda följande steg:</span><span class="sxs-lookup"><span data-stu-id="34317-196">**Resolution**: To fix this error, use the following steps:</span></span>

1. <span data-ttu-id="34317-197">Hämta commons codec jar-filen från ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="34317-197">Download the commons-codec jar file from your HDInsight cluster.</span></span>

        scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/commons-codec*.jar ./commons-codec.jar

2. <span data-ttu-id="34317-198">Avsluta SQuirreL och gå sedan till katalogen där SQuirreL är installerad på datorn.</span><span class="sxs-lookup"><span data-stu-id="34317-198">Exit SQuirreL, and then go to the directory where SQuirreL is installed on your system.</span></span> <span data-ttu-id="34317-199">I katalogen SquirreL under den `lib` directory, Ersätt den befintliga commons codec.jar med det hämtade från HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="34317-199">In the SquirreL directory, under the `lib` directory, replace the existing commons-codec.jar with the one downloaded from the HDInsight cluster.</span></span>

3. <span data-ttu-id="34317-200">Starta om SQuirreL.</span><span class="sxs-lookup"><span data-stu-id="34317-200">Restart SQuirreL.</span></span> <span data-ttu-id="34317-201">Fel bör inte längre uppstå när du ansluter till Hive i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="34317-201">The error should no longer occur when connecting to Hive on HDInsight.</span></span>

## <a name="next-steps"></a><span data-ttu-id="34317-202">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="34317-202">Next steps</span></span>

<span data-ttu-id="34317-203">Nu när du har lärt dig hur du använder JDBC för att arbeta med Hive, utforska andra sätt att arbeta med Azure HDInsight med hjälp av följande länkar.</span><span class="sxs-lookup"><span data-stu-id="34317-203">Now that you have learned how to use JDBC to work with Hive, use the following links to explore other ways to work with Azure HDInsight.</span></span>

* [<span data-ttu-id="34317-204">Överföra data till HDInsight</span><span class="sxs-lookup"><span data-stu-id="34317-204">Upload data to HDInsight</span></span>](hdinsight-upload-data.md)
* [<span data-ttu-id="34317-205">Använda Hive med HDInsight</span><span class="sxs-lookup"><span data-stu-id="34317-205">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="34317-206">Använda Pig med HDInsight</span><span class="sxs-lookup"><span data-stu-id="34317-206">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="34317-207">Använda MapReduce-jobb med HDInsight</span><span class="sxs-lookup"><span data-stu-id="34317-207">Use MapReduce jobs with HDInsight</span></span>](hdinsight-use-mapreduce.md)
