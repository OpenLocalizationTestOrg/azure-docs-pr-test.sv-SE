---
title: aaaConnect Excel tooHadoop med hello ODBC-drivrutinen Hive - Azure HDInsight | Microsoft Docs
description: "Lär dig hur tooset in och använda hello Microsoft Hive ODBC-drivrutin för Excel tooquery data i HDInsight-kluster från Microsoft Excel."
keywords: hadoop excel, hive excel, hive odbc
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
tags: azure-portal
editor: cgronlun
ms.assetid: a7665a14-0211-4521-b3e7-3b26e8029cc0
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/22/2017
ms.author: jgao
ms.openlocfilehash: f01f89e7d4203c739d56079dc589fc11f4aa2174
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-excel-toohadoop-in-azure-hdinsight-with-hello-microsoft-hive-odbc-driver"></a><span data-ttu-id="2276c-104">Ansluta Excel tooHadoop i Azure HDInsight med hello Microsoft Hive ODBC-drivrutin</span><span class="sxs-lookup"><span data-stu-id="2276c-104">Connect Excel tooHadoop in Azure HDInsight with hello Microsoft Hive ODBC driver</span></span>

[!INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

<span data-ttu-id="2276c-105">Microsofts Stordata lösningen integreras Microsoft Business Intelligence (BI) komponenter med Apache Hadoop-kluster som har distribuerats av hello Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="2276c-105">Microsoft's Big Data solution integrates Microsoft Business Intelligence (BI) components with Apache Hadoop clusters that have been deployed by hello Azure HDInsight.</span></span> <span data-ttu-id="2276c-106">Ett exempel på den här integreringen är hello möjlighet tooconnect Excel toohello Hive-datalagret i ett Hadoop-kluster i HDInsight med hello Microsoft Hive öppna ODBC (Database Connectivity) drivrutin.</span><span class="sxs-lookup"><span data-stu-id="2276c-106">An example of this integration is hello ability tooconnect Excel toohello Hive data warehouse of a Hadoop cluster in HDInsight using hello Microsoft Hive Open Database Connectivity (ODBC) Driver.</span></span>

<span data-ttu-id="2276c-107">Det är också möjligt tooconnect hello data som är associerade med ett HDInsight-kluster och andra datakällor, inklusive andra (icke-HDInsight) Hadoop-kluster, från Excel med hjälp av hello Microsoft Power Query-tillägg för Excel.</span><span class="sxs-lookup"><span data-stu-id="2276c-107">It is also possible tooconnect hello data associated with an HDInsight cluster and other data sources, including other (non-HDInsight) Hadoop clusters, from Excel using hello Microsoft Power Query add-in for Excel.</span></span> <span data-ttu-id="2276c-108">Information om att installera och använda Power Query finns [ansluta Excel tooHDInsight med Power Query][hdinsight-power-query].</span><span class="sxs-lookup"><span data-stu-id="2276c-108">For information on installing and using Power Query, see [Connect Excel tooHDInsight with Power Query][hdinsight-power-query].</span></span>

> [!NOTE]
> <span data-ttu-id="2276c-109">Medan hello stegen i den här artikeln kan användas med antingen en Linux eller Windows-baserade HDInsight-kluster Windows krävs för hello klientens arbetsstation.</span><span class="sxs-lookup"><span data-stu-id="2276c-109">While hello steps in this article can be used with either a Linux or Windows-based HDInsight cluster, Windows is required for hello client workstation.</span></span>
> 
> 

<span data-ttu-id="2276c-110">**Krav för**:</span><span class="sxs-lookup"><span data-stu-id="2276c-110">**Prerequisites**:</span></span>

<span data-ttu-id="2276c-111">Innan du börjar den här artikeln, måste du ha hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="2276c-111">Before you begin this article, you must have hello following items:</span></span>

* <span data-ttu-id="2276c-112">**Ett HDInsight-kluster**.</span><span class="sxs-lookup"><span data-stu-id="2276c-112">**An HDInsight cluster**.</span></span> <span data-ttu-id="2276c-113">toocreate, se [komma igång med Azure HDInsight][hdinsight-get-started].</span><span class="sxs-lookup"><span data-stu-id="2276c-113">toocreate one, see [Get started with Azure HDInsight][hdinsight-get-started].</span></span>
* <span data-ttu-id="2276c-114">**En arbetsstation** med Office 2013 Professional Plus Office 365 Pro Plus, fristående för Excel 2013 eller Office Professional Plus 2010.</span><span class="sxs-lookup"><span data-stu-id="2276c-114">**A workstation** with Office 2013 Professional Plus, Office 365 Pro Plus, Excel 2013 Standalone, or Office 2010 Professional Plus.</span></span>

## <a name="install-microsoft-hive-odbc-driver"></a><span data-ttu-id="2276c-115">Installera Microsoft Hive ODBC-drivrutin</span><span class="sxs-lookup"><span data-stu-id="2276c-115">Install Microsoft Hive ODBC driver</span></span>
<span data-ttu-id="2276c-116">Hämta och installera Microsoft Hive ODBC-drivrutinen från hello [Download Center][hive-odbc-driver-download].</span><span class="sxs-lookup"><span data-stu-id="2276c-116">Download and install Microsoft Hive ODBC Driver from hello [Download Center][hive-odbc-driver-download].</span></span>

<span data-ttu-id="2276c-117">Den här drivrutinen kan installeras på 32-bitars eller 64-bitars versioner av Windows 7, Windows 8, Windows 10, Windows Server 2008 R2 och Windows Server 2012.</span><span class="sxs-lookup"><span data-stu-id="2276c-117">This driver can be installed on 32-bit or 64-bit versions of Windows 7, Windows 8, Windows 10, Windows Server 2008 R2, and Windows Server 2012.</span></span> <span data-ttu-id="2276c-118">hello drivrutinen tillåter anslutning tooAzure HDInsight (version 1.6 och senare) och Azure HDInsight-emulatorn (v.1.0.0.0 och senare).</span><span class="sxs-lookup"><span data-stu-id="2276c-118">hello driver allows connection tooAzure HDInsight (version 1.6 and later) and Azure HDInsight Emulator (v.1.0.0.0 and later).</span></span> <span data-ttu-id="2276c-119">Du bör installera hello-version som matchar hello version av hello program där du använder hello ODBC-drivrutinen.</span><span class="sxs-lookup"><span data-stu-id="2276c-119">You shall install hello version that matches hello version of hello application where you use hello ODBC driver.</span></span> <span data-ttu-id="2276c-120">I den här självstudien används hello drivrutinen från Office Excel.</span><span class="sxs-lookup"><span data-stu-id="2276c-120">For this tutorial, hello driver is used from Office Excel.</span></span>

## <a name="create-hive-odbc-data-source"></a><span data-ttu-id="2276c-121">Skapa Hive ODBC-datakälla</span><span class="sxs-lookup"><span data-stu-id="2276c-121">Create Hive ODBC data source</span></span>
<span data-ttu-id="2276c-122">hello följande steg visar hur toocreate en Hive ODBC-datakällan.</span><span class="sxs-lookup"><span data-stu-id="2276c-122">hello following steps show you how toocreate a Hive ODBC Data Source.</span></span>

1. <span data-ttu-id="2276c-123">Tryck på startskärmen för hello Windows viktiga tooopen hello från Windows 8 eller Windows 10 och skriv sedan **datakällor**.</span><span class="sxs-lookup"><span data-stu-id="2276c-123">From Windows 8 or Windows 10, press hello Windows key tooopen hello Start screen, and then type **data sources**.</span></span>
2. <span data-ttu-id="2276c-124">Klicka på **installera ODBC-datakällor (32-bitars)** eller **ställa in ODBC-datakällor (64-bitars)** beroende på dina Office-version.</span><span class="sxs-lookup"><span data-stu-id="2276c-124">Click **Set up ODBC Data sources (32-bit)** or **Set up ODBC Data Sources (64-bit)** depending on your Office version.</span></span> <span data-ttu-id="2276c-125">Om du använder Windows 7, väljer du **ODBC-datakällor (32 bitar)** eller **ODBC-datakällor (64-bitars)** från **Administrationsverktyg**.</span><span class="sxs-lookup"><span data-stu-id="2276c-125">If you are using Windows 7, choose **ODBC Data Sources (32 bit)** or **ODBC Data Sources (64 bit)** from **Administrative Tools**.</span></span> <span data-ttu-id="2276c-126">Du bör se hello **ODBC Data Source Administrator** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2276c-126">You shall see hello **ODBC Data Source Administrator** dialog.</span></span>
   
    <span data-ttu-id="2276c-127">![Datakälla för OBDC](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png "konfigurera DSN med ODBC Data Source Administrator")</span><span class="sxs-lookup"><span data-stu-id="2276c-127">![OBDC data source administrator](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png "Configure a DSN using ODBC Data Source Administrator")</span></span>

3. <span data-ttu-id="2276c-128">Användaren DNS, klicka på **Lägg till** tooopen hello **Skapa ny datakälla** guiden.</span><span class="sxs-lookup"><span data-stu-id="2276c-128">From User DNS, click **Add** tooopen hello **Create New Data Source** wizard.</span></span>
4. <span data-ttu-id="2276c-129">Välj **Microsoft Hive ODBC-drivrutinen**, och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="2276c-129">Select **Microsoft Hive ODBC Driver**, and then click **Finish**.</span></span> <span data-ttu-id="2276c-130">Du bör se hello **Microsoft Hive ODBC-drivrutinen DNS-inställningar för** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2276c-130">You shall see hello **Microsoft Hive ODBC Driver DNS Setup** dialog.</span></span>
5. <span data-ttu-id="2276c-131">Ange eller välj hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="2276c-131">Type or select hello following values:</span></span>
   
   | <span data-ttu-id="2276c-132">Egenskap</span><span class="sxs-lookup"><span data-stu-id="2276c-132">Property</span></span> | <span data-ttu-id="2276c-133">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2276c-133">Description</span></span> |
   | --- | --- |
   |  <span data-ttu-id="2276c-134">Namn på datakälla</span><span class="sxs-lookup"><span data-stu-id="2276c-134">Data Source Name</span></span> |<span data-ttu-id="2276c-135">Ge en namnet tooyour-datakälla</span><span class="sxs-lookup"><span data-stu-id="2276c-135">Give a name tooyour data source</span></span> |
   |  <span data-ttu-id="2276c-136">Värd</span><span class="sxs-lookup"><span data-stu-id="2276c-136">Host</span></span> |<span data-ttu-id="2276c-137">Ange &lt;HDInsightClusterName>.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="2276c-137">Enter &lt;HDInsightClusterName>.azurehdinsight.net.</span></span> <span data-ttu-id="2276c-138">Till exempel myHDICluster.azurehdinsight.net</span><span class="sxs-lookup"><span data-stu-id="2276c-138">For example, myHDICluster.azurehdinsight.net</span></span> |
   |  <span data-ttu-id="2276c-139">Port</span><span class="sxs-lookup"><span data-stu-id="2276c-139">Port</span></span> |<span data-ttu-id="2276c-140">Använd <strong>443</strong>.</span><span class="sxs-lookup"><span data-stu-id="2276c-140">Use <strong>443</strong>.</span></span> <span data-ttu-id="2276c-141">(Den här porten har ändrats från 563 too443.)</span><span class="sxs-lookup"><span data-stu-id="2276c-141">(This port has been changed from 563 too443.)</span></span> |
   |  <span data-ttu-id="2276c-142">Databas</span><span class="sxs-lookup"><span data-stu-id="2276c-142">Database</span></span> |<span data-ttu-id="2276c-143">Använd <strong>Standard</strong>.</span><span class="sxs-lookup"><span data-stu-id="2276c-143">Use <strong>Default</strong>.</span></span> |
   |  <span data-ttu-id="2276c-144">Mekanism</span><span class="sxs-lookup"><span data-stu-id="2276c-144">Mechanism</span></span> |<span data-ttu-id="2276c-145">Välj <strong>Azure HDInsight-tjänst</strong></span><span class="sxs-lookup"><span data-stu-id="2276c-145">Select <strong>Azure HDInsight Service</strong></span></span> |
   |  <span data-ttu-id="2276c-146">Användarnamn</span><span class="sxs-lookup"><span data-stu-id="2276c-146">User Name</span></span> |<span data-ttu-id="2276c-147">Ange ett HDInsight-kluster HTTP användaren användarnamn.</span><span class="sxs-lookup"><span data-stu-id="2276c-147">Enter HDInsight cluster HTTP user username.</span></span> <span data-ttu-id="2276c-148">hello Standardanvändarnamnet är <strong>admin</strong>.</span><span class="sxs-lookup"><span data-stu-id="2276c-148">hello default username is <strong>admin</strong>.</span></span> |
   |  <span data-ttu-id="2276c-149">Lösenord</span><span class="sxs-lookup"><span data-stu-id="2276c-149">Password</span></span> |<span data-ttu-id="2276c-150">Ange användarlösenord för HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="2276c-150">Enter HDInsight cluster user password.</span></span> |
   
    </table>
   
    <span data-ttu-id="2276c-151">Det finns vissa viktiga parametrar toobe medveten om när du klickar på **avancerade alternativ**:</span><span class="sxs-lookup"><span data-stu-id="2276c-151">There are some important parameters toobe aware of when you click **Advanced Options**:</span></span>
   
   | <span data-ttu-id="2276c-152">Parameter</span><span class="sxs-lookup"><span data-stu-id="2276c-152">Parameter</span></span> | <span data-ttu-id="2276c-153">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2276c-153">Description</span></span> |
   | --- | --- |
   |  <span data-ttu-id="2276c-154">Använda en intern fråga</span><span class="sxs-lookup"><span data-stu-id="2276c-154">Use Native Query</span></span> |<span data-ttu-id="2276c-155">När den är markerad försöker hello ODBC-drivrutinen inte tooconvert TSQL i HiveQL.</span><span class="sxs-lookup"><span data-stu-id="2276c-155">When it is selected, hello ODBC driver does NOT try tooconvert TSQL into HiveQL.</span></span> <span data-ttu-id="2276c-156">Du kan använda den om du är 100% säker på att du skickar ren HiveQL-instruktioner.</span><span class="sxs-lookup"><span data-stu-id="2276c-156">You shall use it only if you are 100% sure you are submitting pure HiveQL statements.</span></span> <span data-ttu-id="2276c-157">När du ansluter tooSQL Server eller Azure SQL Database, bör den vara avmarkerad.</span><span class="sxs-lookup"><span data-stu-id="2276c-157">When connecting tooSQL Server or Azure SQL Database, you should leave it unchecked.</span></span> |
   |  <span data-ttu-id="2276c-158">Rader som hämtas per block</span><span class="sxs-lookup"><span data-stu-id="2276c-158">Rows fetched per block</span></span> |<span data-ttu-id="2276c-159">När du hämtar ett stort antal poster kan inställning av den här parametern vara nödvändiga tooensure optimala prestanda.</span><span class="sxs-lookup"><span data-stu-id="2276c-159">When fetching a large number of records, tuning this parameter may be required tooensure optimal performances.</span></span> |
   |  <span data-ttu-id="2276c-160">Standard kolumnen stränglängd, binära kolumnlängden, Decimal kolumnskalan</span><span class="sxs-lookup"><span data-stu-id="2276c-160">Default string column length, Binary column length, Decimal column scale</span></span> |<span data-ttu-id="2276c-161">hello datatypen längder och Precision-datorerna kan påverka hur data returneras.</span><span class="sxs-lookup"><span data-stu-id="2276c-161">hello data type lengths and precisions may affect how data is returned.</span></span> <span data-ttu-id="2276c-162">De orsakar felaktig information toobe returnerade på grund av tooloss av precision och/eller trunkering.</span><span class="sxs-lookup"><span data-stu-id="2276c-162">They cause incorrect information toobe returned due tooloss of precision and/or truncation.</span></span> |

    <span data-ttu-id="2276c-163">![Avancerade alternativ](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png "DSN avancerade konfigurationsalternativ")</span><span class="sxs-lookup"><span data-stu-id="2276c-163">![Advanced options](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png "Advanced DSN configuration options")</span></span>

1. <span data-ttu-id="2276c-164">Klicka på **Test** tootest hello-datakälla.</span><span class="sxs-lookup"><span data-stu-id="2276c-164">Click **Test** tootest hello data source.</span></span> <span data-ttu-id="2276c-165">Hello-datakällan är korrekt konfigurerad, visas *TESTERNA har SLUTFÖRTS!*.</span><span class="sxs-lookup"><span data-stu-id="2276c-165">When hello data source is configured correctly, it shows *TESTS COMPLETED SUCCESSFULLY!*.</span></span>
2. <span data-ttu-id="2276c-166">Klicka på **OK** tooclose hello Test dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2276c-166">Click **OK** tooclose hello Test dialog.</span></span> <span data-ttu-id="2276c-167">hello ny datakälla skall anges i hello **ODBC Data Source Administrator**.</span><span class="sxs-lookup"><span data-stu-id="2276c-167">hello new data source shall be listed on hello **ODBC Data Source Administrator**.</span></span>
3. <span data-ttu-id="2276c-168">Klicka på **OK** tooexit hello guiden.</span><span class="sxs-lookup"><span data-stu-id="2276c-168">Click **OK** tooexit hello wizard.</span></span>

## <a name="import-data-into-excel-from-hdinsight"></a><span data-ttu-id="2276c-169">Importera data till Excel från HDInsight</span><span class="sxs-lookup"><span data-stu-id="2276c-169">Import data into Excel from HDInsight</span></span>
<span data-ttu-id="2276c-170">hello följande steg beskriver hello sätt tooimport data från en Hive-tabell till en Excel-arbetsbok med hello ODBC-datakällan som du skapade i föregående avsnitt i hello.</span><span class="sxs-lookup"><span data-stu-id="2276c-170">hello following steps describe hello way tooimport data from a Hive table into an Excel workbook using hello ODBC data source that you created in hello previous section.</span></span>

1. <span data-ttu-id="2276c-171">Öppna en ny eller befintlig arbetsbok i Excel.</span><span class="sxs-lookup"><span data-stu-id="2276c-171">Open a new or existing workbook in Excel.</span></span>
2. <span data-ttu-id="2276c-172">Från hello **Data** klickar du på **hämta Data**, klickar du på **från andra källor**, och klicka sedan på **från ODBC** toolaunch hello  **Guiden**.</span><span class="sxs-lookup"><span data-stu-id="2276c-172">From hello **Data** tab, click **Get Data**, click **From Other Sources**, and then click **From ODBC** toolaunch hello **Data Connection Wizard**.</span></span>
   
    <span data-ttu-id="2276c-173">![Öppna guiden](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png "öppna guiden")</span><span class="sxs-lookup"><span data-stu-id="2276c-173">![Open data connection wizard](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png "Open data connection wizard")</span></span>
4. <span data-ttu-id="2276c-174">Välj hello data datakällans namn som du skapade i hello sista avsnittet och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="2276c-174">Select hello data source name that you created in hello last section, and then click **OK**.</span></span>
5. <span data-ttu-id="2276c-175">Ange Hadoop-användarnamn (hello standardnamnet är admin) hello lösenord och klicka sedan på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="2276c-175">Enter Hadoop user name (hello default name is admin) and hello password, and then click **Connect**.</span></span>
6. <span data-ttu-id="2276c-176">Expandera på Navigator **HIVE**, expandera **standard**, klickar du på **hivesampletable**, och klicka sedan på **belastningen**.</span><span class="sxs-lookup"><span data-stu-id="2276c-176">On Navigator, expand **HIVE**, expand **default**, click **hivesampletable**, and then click **Load**.</span></span> <span data-ttu-id="2276c-177">Det tar några sekunder innan data hämtar importerade tooExcel.</span><span class="sxs-lookup"><span data-stu-id="2276c-177">It takes a few seconds before data gets imported tooExcel.</span></span>

    <span data-ttu-id="2276c-178">![HDInsight Hive ODBC navigator](./media/hdinsight-connect-excel-hive-ODBC-driver/hdinsight.hive.odbc.navigator.png "öppna guiden")</span><span class="sxs-lookup"><span data-stu-id="2276c-178">![HDInsight Hive ODBC navigator](./media/hdinsight-connect-excel-hive-ODBC-driver/hdinsight.hive.odbc.navigator.png "Open data connection wizard")</span></span>


## <a name="next-steps"></a><span data-ttu-id="2276c-179">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2276c-179">Next steps</span></span>
<span data-ttu-id="2276c-180">I den här artikeln har du lärt dig hur toouse hello Microsoft Hive ODBC-drivrutinen tooretrieve data från hello HDInsight Service till Excel.</span><span class="sxs-lookup"><span data-stu-id="2276c-180">In this article, you learned how toouse hello Microsoft Hive ODBC driver tooretrieve data from hello HDInsight Service into Excel.</span></span> <span data-ttu-id="2276c-181">På liknande sätt kan du hämta data från hello HDInsight Service i SQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="2276c-181">Similarly, you can retrieve data from hello HDInsight Service into SQL Database.</span></span> <span data-ttu-id="2276c-182">Det är också möjligt tooupload data till ett HDInsight Service.</span><span class="sxs-lookup"><span data-stu-id="2276c-182">It is also possible tooupload data into an HDInsight Service.</span></span> <span data-ttu-id="2276c-183">Det finns fler toolearn:</span><span class="sxs-lookup"><span data-stu-id="2276c-183">toolearn more, see:</span></span>

* <span data-ttu-id="2276c-184">[Analysera svarta fördröjning data med HDInsight][hdinsight-analyze-flight-data]</span><span class="sxs-lookup"><span data-stu-id="2276c-184">[Analyze flight delay data using HDInsight][hdinsight-analyze-flight-data]</span></span>
* <span data-ttu-id="2276c-185">[Ladda upp Data tooHDInsight][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="2276c-185">[Upload Data tooHDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="2276c-186">[Använda Sqoop med HDInsight][hdinsight-use-sqoop]</span><span class="sxs-lookup"><span data-stu-id="2276c-186">[Use Sqoop with HDInsight][hdinsight-use-sqoop]</span></span>

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-get-started]: hdinsight-hadoop-tutorial-get-started-windows.md

[hive-odbc-driver-download]: http://go.microsoft.com/fwlink/?LinkID=286698


