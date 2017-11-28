---
title: Ansluta Excel till Hadoop med Hive ODBC-drivrutin - Azure HDInsight | Microsoft Docs
description: "Lär dig hur du ställer in och använda Microsoft Hive ODBC-drivrutin för Excel för att fråga efter data i HDInsight-kluster från Microsoft Excel."
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
ms.openlocfilehash: 7818093e42c34ee671a035cde783a6622fb2a798
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="connect-excel-to-hadoop-in-azure-hdinsight-with-the-microsoft-hive-odbc-driver"></a><span data-ttu-id="7aff2-104">Ansluta Excel till Hadoop i Azure HDInsight med Microsoft Hive ODBC-drivrutin</span><span class="sxs-lookup"><span data-stu-id="7aff2-104">Connect Excel to Hadoop in Azure HDInsight with the Microsoft Hive ODBC driver</span></span>

[!INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

<span data-ttu-id="7aff2-105">Microsofts Stordata lösningen integreras Microsoft Business Intelligence (BI) komponenter med Apache Hadoop-kluster som har distribuerats i Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="7aff2-105">Microsoft's Big Data solution integrates Microsoft Business Intelligence (BI) components with Apache Hadoop clusters that have been deployed by the Azure HDInsight.</span></span> <span data-ttu-id="7aff2-106">Ett exempel på den här integreringen är möjligheten att ansluta Excel till datalagret Hive i ett Hadoop-kluster i HDInsight med hjälp av drivrutinen för Microsoft Hive ODBC Open Database Connectivity ().</span><span class="sxs-lookup"><span data-stu-id="7aff2-106">An example of this integration is the ability to connect Excel to the Hive data warehouse of a Hadoop cluster in HDInsight using the Microsoft Hive Open Database Connectivity (ODBC) Driver.</span></span>

<span data-ttu-id="7aff2-107">Det är också möjligt att ansluta data som är associerade med ett HDInsight-kluster och andra datakällor, inklusive andra (ej HDInsight) Hadoop-kluster, från Excel med hjälp av Microsoft Power Query-tillägget för Excel.</span><span class="sxs-lookup"><span data-stu-id="7aff2-107">It is also possible to connect the data associated with an HDInsight cluster and other data sources, including other (non-HDInsight) Hadoop clusters, from Excel using the Microsoft Power Query add-in for Excel.</span></span> <span data-ttu-id="7aff2-108">Information om att installera och använda Power Query finns [ansluta Excel till HDInsight med Power Query][hdinsight-power-query].</span><span class="sxs-lookup"><span data-stu-id="7aff2-108">For information on installing and using Power Query, see [Connect Excel to HDInsight with Power Query][hdinsight-power-query].</span></span>

> [!NOTE]
> <span data-ttu-id="7aff2-109">När stegen i den här artikeln kan användas med en Linux- eller Windows-baserade HDInsight-kluster, krävs Windows för klientens arbetsstation.</span><span class="sxs-lookup"><span data-stu-id="7aff2-109">While the steps in this article can be used with either a Linux or Windows-based HDInsight cluster, Windows is required for the client workstation.</span></span>
> 
> 

<span data-ttu-id="7aff2-110">**Krav för**:</span><span class="sxs-lookup"><span data-stu-id="7aff2-110">**Prerequisites**:</span></span>

<span data-ttu-id="7aff2-111">Innan du börjar den här artikeln, måste du ha följande:</span><span class="sxs-lookup"><span data-stu-id="7aff2-111">Before you begin this article, you must have the following items:</span></span>

* <span data-ttu-id="7aff2-112">**Ett HDInsight-kluster**.</span><span class="sxs-lookup"><span data-stu-id="7aff2-112">**An HDInsight cluster**.</span></span> <span data-ttu-id="7aff2-113">Om du vill skapa en finns [komma igång med Azure HDInsight][hdinsight-get-started].</span><span class="sxs-lookup"><span data-stu-id="7aff2-113">To create one, see [Get started with Azure HDInsight][hdinsight-get-started].</span></span>
* <span data-ttu-id="7aff2-114">**En arbetsstation** med Office 2013 Professional Plus Office 365 Pro Plus, fristående för Excel 2013 eller Office Professional Plus 2010.</span><span class="sxs-lookup"><span data-stu-id="7aff2-114">**A workstation** with Office 2013 Professional Plus, Office 365 Pro Plus, Excel 2013 Standalone, or Office 2010 Professional Plus.</span></span>

## <a name="install-microsoft-hive-odbc-driver"></a><span data-ttu-id="7aff2-115">Installera Microsoft Hive ODBC-drivrutin</span><span class="sxs-lookup"><span data-stu-id="7aff2-115">Install Microsoft Hive ODBC driver</span></span>
<span data-ttu-id="7aff2-116">Hämta och installera Microsoft Hive ODBC-drivrutinen från den [Download Center][hive-odbc-driver-download].</span><span class="sxs-lookup"><span data-stu-id="7aff2-116">Download and install Microsoft Hive ODBC Driver from the [Download Center][hive-odbc-driver-download].</span></span>

<span data-ttu-id="7aff2-117">Den här drivrutinen kan installeras på 32-bitars eller 64-bitars versioner av Windows 7, Windows 8, Windows 10, Windows Server 2008 R2 och Windows Server 2012.</span><span class="sxs-lookup"><span data-stu-id="7aff2-117">This driver can be installed on 32-bit or 64-bit versions of Windows 7, Windows 8, Windows 10, Windows Server 2008 R2, and Windows Server 2012.</span></span> <span data-ttu-id="7aff2-118">Drivrutinen tillåter anslutning till Azure HDInsight (version 1.6 och senare) och Azure HDInsight-emulatorn (v.1.0.0.0 och senare).</span><span class="sxs-lookup"><span data-stu-id="7aff2-118">The driver allows connection to Azure HDInsight (version 1.6 and later) and Azure HDInsight Emulator (v.1.0.0.0 and later).</span></span> <span data-ttu-id="7aff2-119">Du kan installera den version som matchar versionen av programmet där du använder ODBC-drivrutinen.</span><span class="sxs-lookup"><span data-stu-id="7aff2-119">You shall install the version that matches the version of the application where you use the ODBC driver.</span></span> <span data-ttu-id="7aff2-120">Den här självstudien används drivrutinen från Office Excel.</span><span class="sxs-lookup"><span data-stu-id="7aff2-120">For this tutorial, the driver is used from Office Excel.</span></span>

## <a name="create-hive-odbc-data-source"></a><span data-ttu-id="7aff2-121">Skapa Hive ODBC-datakälla</span><span class="sxs-lookup"><span data-stu-id="7aff2-121">Create Hive ODBC data source</span></span>
<span data-ttu-id="7aff2-122">Följande steg visar hur du skapar en Hive ODBC-datakällan.</span><span class="sxs-lookup"><span data-stu-id="7aff2-122">The following steps show you how to create a Hive ODBC Data Source.</span></span>

1. <span data-ttu-id="7aff2-123">Från Windows 8 eller Windows 10, tryck på Windows-tangenten för att öppna startskärmen och skriv **datakällor**.</span><span class="sxs-lookup"><span data-stu-id="7aff2-123">From Windows 8 or Windows 10, press the Windows key to open the Start screen, and then type **data sources**.</span></span>
2. <span data-ttu-id="7aff2-124">Klicka på **installera ODBC-datakällor (32-bitars)** eller **ställa in ODBC-datakällor (64-bitars)** beroende på dina Office-version.</span><span class="sxs-lookup"><span data-stu-id="7aff2-124">Click **Set up ODBC Data sources (32-bit)** or **Set up ODBC Data Sources (64-bit)** depending on your Office version.</span></span> <span data-ttu-id="7aff2-125">Om du använder Windows 7, väljer du **ODBC-datakällor (32 bitar)** eller **ODBC-datakällor (64-bitars)** från **Administrationsverktyg**.</span><span class="sxs-lookup"><span data-stu-id="7aff2-125">If you are using Windows 7, choose **ODBC Data Sources (32 bit)** or **ODBC Data Sources (64 bit)** from **Administrative Tools**.</span></span> <span data-ttu-id="7aff2-126">Du bör se den **ODBC Data Source Administrator** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7aff2-126">You shall see the **ODBC Data Source Administrator** dialog.</span></span>
   
    <span data-ttu-id="7aff2-127">![Datakälla för OBDC](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png "konfigurera DSN med ODBC Data Source Administrator")</span><span class="sxs-lookup"><span data-stu-id="7aff2-127">![OBDC data source administrator](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png "Configure a DSN using ODBC Data Source Administrator")</span></span>

3. <span data-ttu-id="7aff2-128">Användaren DNS, klicka på **Lägg till** att öppna den **Skapa ny datakälla** guiden.</span><span class="sxs-lookup"><span data-stu-id="7aff2-128">From User DNS, click **Add** to open the **Create New Data Source** wizard.</span></span>
4. <span data-ttu-id="7aff2-129">Välj **Microsoft Hive ODBC-drivrutinen**, och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="7aff2-129">Select **Microsoft Hive ODBC Driver**, and then click **Finish**.</span></span> <span data-ttu-id="7aff2-130">Du bör se den **Microsoft Hive ODBC-drivrutinen DNS-inställningar för** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7aff2-130">You shall see the **Microsoft Hive ODBC Driver DNS Setup** dialog.</span></span>
5. <span data-ttu-id="7aff2-131">Ange eller välj följande värden:</span><span class="sxs-lookup"><span data-stu-id="7aff2-131">Type or select the following values:</span></span>
   
   | <span data-ttu-id="7aff2-132">Egenskap</span><span class="sxs-lookup"><span data-stu-id="7aff2-132">Property</span></span> | <span data-ttu-id="7aff2-133">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7aff2-133">Description</span></span> |
   | --- | --- |
   |  <span data-ttu-id="7aff2-134">Namn på datakälla</span><span class="sxs-lookup"><span data-stu-id="7aff2-134">Data Source Name</span></span> |<span data-ttu-id="7aff2-135">Namnge din datakälla</span><span class="sxs-lookup"><span data-stu-id="7aff2-135">Give a name to your data source</span></span> |
   |  <span data-ttu-id="7aff2-136">Värd</span><span class="sxs-lookup"><span data-stu-id="7aff2-136">Host</span></span> |<span data-ttu-id="7aff2-137">Ange &lt;HDInsightClusterName>.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="7aff2-137">Enter &lt;HDInsightClusterName>.azurehdinsight.net.</span></span> <span data-ttu-id="7aff2-138">Till exempel myHDICluster.azurehdinsight.net</span><span class="sxs-lookup"><span data-stu-id="7aff2-138">For example, myHDICluster.azurehdinsight.net</span></span> |
   |  <span data-ttu-id="7aff2-139">Port</span><span class="sxs-lookup"><span data-stu-id="7aff2-139">Port</span></span> |<span data-ttu-id="7aff2-140">Använd <strong>443</strong>.</span><span class="sxs-lookup"><span data-stu-id="7aff2-140">Use <strong>443</strong>.</span></span> <span data-ttu-id="7aff2-141">(Den här porten har ändrats från 563 till 443.)</span><span class="sxs-lookup"><span data-stu-id="7aff2-141">(This port has been changed from 563 to 443.)</span></span> |
   |  <span data-ttu-id="7aff2-142">Databas</span><span class="sxs-lookup"><span data-stu-id="7aff2-142">Database</span></span> |<span data-ttu-id="7aff2-143">Använd <strong>Standard</strong>.</span><span class="sxs-lookup"><span data-stu-id="7aff2-143">Use <strong>Default</strong>.</span></span> |
   |  <span data-ttu-id="7aff2-144">Mekanism</span><span class="sxs-lookup"><span data-stu-id="7aff2-144">Mechanism</span></span> |<span data-ttu-id="7aff2-145">Välj <strong>Azure HDInsight-tjänst</strong></span><span class="sxs-lookup"><span data-stu-id="7aff2-145">Select <strong>Azure HDInsight Service</strong></span></span> |
   |  <span data-ttu-id="7aff2-146">Användarnamn</span><span class="sxs-lookup"><span data-stu-id="7aff2-146">User Name</span></span> |<span data-ttu-id="7aff2-147">Ange ett HDInsight-kluster HTTP användaren användarnamn.</span><span class="sxs-lookup"><span data-stu-id="7aff2-147">Enter HDInsight cluster HTTP user username.</span></span> <span data-ttu-id="7aff2-148">Standardanvändarnamnet är <strong>admin</strong>.</span><span class="sxs-lookup"><span data-stu-id="7aff2-148">The default username is <strong>admin</strong>.</span></span> |
   |  <span data-ttu-id="7aff2-149">Lösenord</span><span class="sxs-lookup"><span data-stu-id="7aff2-149">Password</span></span> |<span data-ttu-id="7aff2-150">Ange användarlösenord för HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="7aff2-150">Enter HDInsight cluster user password.</span></span> |
   
    </table>
   
    <span data-ttu-id="7aff2-151">Det finns vissa viktiga parametrar att vara medveten om när du klickar på **avancerade alternativ**:</span><span class="sxs-lookup"><span data-stu-id="7aff2-151">There are some important parameters to be aware of when you click **Advanced Options**:</span></span>
   
   | <span data-ttu-id="7aff2-152">Parameter</span><span class="sxs-lookup"><span data-stu-id="7aff2-152">Parameter</span></span> | <span data-ttu-id="7aff2-153">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7aff2-153">Description</span></span> |
   | --- | --- |
   |  <span data-ttu-id="7aff2-154">Använda en intern fråga</span><span class="sxs-lookup"><span data-stu-id="7aff2-154">Use Native Query</span></span> |<span data-ttu-id="7aff2-155">När den är markerad försöker ODBC-drivrutinen inte konvertera TSQL till HiveQL.</span><span class="sxs-lookup"><span data-stu-id="7aff2-155">When it is selected, the ODBC driver does NOT try to convert TSQL into HiveQL.</span></span> <span data-ttu-id="7aff2-156">Du kan använda den om du är 100% säker på att du skickar ren HiveQL-instruktioner.</span><span class="sxs-lookup"><span data-stu-id="7aff2-156">You shall use it only if you are 100% sure you are submitting pure HiveQL statements.</span></span> <span data-ttu-id="7aff2-157">När du ansluter till SQL Server eller Azure SQL Database, bör den vara avmarkerad.</span><span class="sxs-lookup"><span data-stu-id="7aff2-157">When connecting to SQL Server or Azure SQL Database, you should leave it unchecked.</span></span> |
   |  <span data-ttu-id="7aff2-158">Rader som hämtas per block</span><span class="sxs-lookup"><span data-stu-id="7aff2-158">Rows fetched per block</span></span> |<span data-ttu-id="7aff2-159">När du hämtar ett stort antal poster kan inställning av den här parametern krävas att säkerställa optimala prestanda.</span><span class="sxs-lookup"><span data-stu-id="7aff2-159">When fetching a large number of records, tuning this parameter may be required to ensure optimal performances.</span></span> |
   |  <span data-ttu-id="7aff2-160">Standard kolumnen stränglängd, binära kolumnlängden, Decimal kolumnskalan</span><span class="sxs-lookup"><span data-stu-id="7aff2-160">Default string column length, Binary column length, Decimal column scale</span></span> |<span data-ttu-id="7aff2-161">Datatypen längder och Precision-datorerna kan påverka hur data returneras.</span><span class="sxs-lookup"><span data-stu-id="7aff2-161">The data type lengths and precisions may affect how data is returned.</span></span> <span data-ttu-id="7aff2-162">De orsakar felaktig information som ska returneras på grund av förlust av precision och/eller trunkering.</span><span class="sxs-lookup"><span data-stu-id="7aff2-162">They cause incorrect information to be returned due to loss of precision and/or truncation.</span></span> |

    <span data-ttu-id="7aff2-163">![Avancerade alternativ](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png "DSN avancerade konfigurationsalternativ")</span><span class="sxs-lookup"><span data-stu-id="7aff2-163">![Advanced options](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png "Advanced DSN configuration options")</span></span>

1. <span data-ttu-id="7aff2-164">Klicka på **testa** att testa datakällan.</span><span class="sxs-lookup"><span data-stu-id="7aff2-164">Click **Test** to test the data source.</span></span> <span data-ttu-id="7aff2-165">När datakällan är korrekt konfigurerad, visas *TESTERNA har SLUTFÖRTS!*.</span><span class="sxs-lookup"><span data-stu-id="7aff2-165">When the data source is configured correctly, it shows *TESTS COMPLETED SUCCESSFULLY!*.</span></span>
2. <span data-ttu-id="7aff2-166">Klicka på **OK** att stänga dialogrutan Test.</span><span class="sxs-lookup"><span data-stu-id="7aff2-166">Click **OK** to close the Test dialog.</span></span> <span data-ttu-id="7aff2-167">Den nya datakällan skall anges i den **ODBC Data Source Administrator**.</span><span class="sxs-lookup"><span data-stu-id="7aff2-167">The new data source shall be listed on the **ODBC Data Source Administrator**.</span></span>
3. <span data-ttu-id="7aff2-168">Klicka på **OK** vill avsluta guiden.</span><span class="sxs-lookup"><span data-stu-id="7aff2-168">Click **OK** to exit the wizard.</span></span>

## <a name="import-data-into-excel-from-hdinsight"></a><span data-ttu-id="7aff2-169">Importera data till Excel från HDInsight</span><span class="sxs-lookup"><span data-stu-id="7aff2-169">Import data into Excel from HDInsight</span></span>
<span data-ttu-id="7aff2-170">Följande steg beskriver hur du importerar data från en Hive-tabell till en Excel-arbetsbok med hjälp av ODBC-datakällan som du skapade i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7aff2-170">The following steps describe the way to import data from a Hive table into an Excel workbook using the ODBC data source that you created in the previous section.</span></span>

1. <span data-ttu-id="7aff2-171">Öppna en ny eller befintlig arbetsbok i Excel.</span><span class="sxs-lookup"><span data-stu-id="7aff2-171">Open a new or existing workbook in Excel.</span></span>
2. <span data-ttu-id="7aff2-172">Från den **Data** klickar du på **hämta Data**, klickar du på **från andra källor**, och klicka sedan på **från ODBC** att starta den **Dataanslutningsguiden**.</span><span class="sxs-lookup"><span data-stu-id="7aff2-172">From the **Data** tab, click **Get Data**, click **From Other Sources**, and then click **From ODBC** to launch the **Data Connection Wizard**.</span></span>
   
    <span data-ttu-id="7aff2-173">![Öppna guiden](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png "öppna guiden")</span><span class="sxs-lookup"><span data-stu-id="7aff2-173">![Open data connection wizard](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png "Open data connection wizard")</span></span>
4. <span data-ttu-id="7aff2-174">Välj namnet på datakällan som du skapade i det sista avsnittet och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7aff2-174">Select the data source name that you created in the last section, and then click **OK**.</span></span>
5. <span data-ttu-id="7aff2-175">Ange Hadoop-användarnamn (standardnamnet är admin) och lösenordet och klicka sedan på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="7aff2-175">Enter Hadoop user name (the default name is admin) and the password, and then click **Connect**.</span></span>
6. <span data-ttu-id="7aff2-176">Expandera på Navigator **HIVE**, expandera **standard**, klickar du på **hivesampletable**, och klicka sedan på **belastningen**.</span><span class="sxs-lookup"><span data-stu-id="7aff2-176">On Navigator, expand **HIVE**, expand **default**, click **hivesampletable**, and then click **Load**.</span></span> <span data-ttu-id="7aff2-177">Det tar några sekunder innan data har importerats till Excel.</span><span class="sxs-lookup"><span data-stu-id="7aff2-177">It takes a few seconds before data gets imported to Excel.</span></span>

    <span data-ttu-id="7aff2-178">![HDInsight Hive ODBC navigator](./media/hdinsight-connect-excel-hive-ODBC-driver/hdinsight.hive.odbc.navigator.png "öppna guiden")</span><span class="sxs-lookup"><span data-stu-id="7aff2-178">![HDInsight Hive ODBC navigator](./media/hdinsight-connect-excel-hive-ODBC-driver/hdinsight.hive.odbc.navigator.png "Open data connection wizard")</span></span>


## <a name="next-steps"></a><span data-ttu-id="7aff2-179">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7aff2-179">Next steps</span></span>
<span data-ttu-id="7aff2-180">I den här artikeln har du lärt dig hur du använder Microsoft Hive ODBC-drivrutinen för att hämta data från HDInsight Service till Excel.</span><span class="sxs-lookup"><span data-stu-id="7aff2-180">In this article, you learned how to use the Microsoft Hive ODBC driver to retrieve data from the HDInsight Service into Excel.</span></span> <span data-ttu-id="7aff2-181">På liknande sätt kan du hämta data från HDInsight Service i SQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="7aff2-181">Similarly, you can retrieve data from the HDInsight Service into SQL Database.</span></span> <span data-ttu-id="7aff2-182">Det är också möjligt att överföra data till ett HDInsight Service.</span><span class="sxs-lookup"><span data-stu-id="7aff2-182">It is also possible to upload data into an HDInsight Service.</span></span> <span data-ttu-id="7aff2-183">Du kan läsa mer här:</span><span class="sxs-lookup"><span data-stu-id="7aff2-183">To learn more, see:</span></span>

* <span data-ttu-id="7aff2-184">[Analysera svarta fördröjning data med HDInsight][hdinsight-analyze-flight-data]</span><span class="sxs-lookup"><span data-stu-id="7aff2-184">[Analyze flight delay data using HDInsight][hdinsight-analyze-flight-data]</span></span>
* <span data-ttu-id="7aff2-185">[Överföra Data till HDInsight][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="7aff2-185">[Upload Data to HDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="7aff2-186">[Använda Sqoop med HDInsight][hdinsight-use-sqoop]</span><span class="sxs-lookup"><span data-stu-id="7aff2-186">[Use Sqoop with HDInsight][hdinsight-use-sqoop]</span></span>

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-get-started]: hdinsight-hadoop-tutorial-get-started-windows.md

[hive-odbc-driver-download]: http://go.microsoft.com/fwlink/?LinkID=286698


