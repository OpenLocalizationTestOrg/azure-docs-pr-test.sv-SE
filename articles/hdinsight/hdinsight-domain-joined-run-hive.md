---
title: "Konfigurera principer för Hive i HDInsight med domänanslutna - Azure | Microsoft Docs"
description: "Läs mer ..."
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 3fade1e5-c2e1-4ad5-b371-f95caea23f6d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/25/2016
ms.author: saurinsh
ms.openlocfilehash: de537d5e39dd0d3f75ff802948c7372e4d65d127
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-hive-policies-in-domain-joined-hdinsight-preview"></a><span data-ttu-id="bc984-103">Konfigurera Hive-principer i domänanslutna HDInsight (förhandsversion)</span><span class="sxs-lookup"><span data-stu-id="bc984-103">Configure Hive policies in Domain-joined HDInsight (Preview)</span></span>
<span data-ttu-id="bc984-104">Ta reda på mer om hur du konfigurerar Apache Ranger-principer för Hive.</span><span class="sxs-lookup"><span data-stu-id="bc984-104">Learn how to configure Apache Ranger policies for Hive.</span></span> <span data-ttu-id="bc984-105">I den här artikeln skapar du två Ranger-principer för att begränsa åtkomsten till hivesampletable.</span><span class="sxs-lookup"><span data-stu-id="bc984-105">In this article, you create two Ranger policies to restrict access to the hivesampletable.</span></span> <span data-ttu-id="bc984-106">Hivesampletable medföljer HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="bc984-106">The hivesampletable comes with HDInsight clusters.</span></span> <span data-ttu-id="bc984-107">När du har konfigurerat principerna kan du använda Excel och ODBC-drivrutinen för att ansluta till Hive-tabeller i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="bc984-107">After you have configured the policies, you use Excel and ODBC driver to connect to Hive tables in HDInsight.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bc984-108">Krav</span><span class="sxs-lookup"><span data-stu-id="bc984-108">Prerequisites</span></span>
* <span data-ttu-id="bc984-109">Ett domänanslutet HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="bc984-109">A Domain-joined HDInsight cluster.</span></span> <span data-ttu-id="bc984-110">Se [Konfigurera domänanslutna HDInsight-kluster](hdinsight-domain-joined-configure.md).</span><span class="sxs-lookup"><span data-stu-id="bc984-110">See [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md).</span></span>
* <span data-ttu-id="bc984-111">En arbetsstation med Office 2016, Office 2013 Professional Plus, Office 365 Pro Plus, fristående Excel 2013 eller Office 2010 Professional Plus.</span><span class="sxs-lookup"><span data-stu-id="bc984-111">A workstation with Office 2016, Office 2013 Professional Plus, Office 365 Pro Plus, Excel 2013 Standalone, or Office 2010 Professional Plus.</span></span>

## <a name="connect-to-apache-ranger-admin-ui"></a><span data-ttu-id="bc984-112">Anslut till Apache Ranger Admin-gränssnittet</span><span class="sxs-lookup"><span data-stu-id="bc984-112">Connect to Apache Ranger Admin UI</span></span>
<span data-ttu-id="bc984-113">**Så här ansluter du till Ranger Admin-gränssnittet**</span><span class="sxs-lookup"><span data-stu-id="bc984-113">**To connect to Ranger Admin UI**</span></span>

1. <span data-ttu-id="bc984-114">Anslut till Ranger Admin-gränssnittet från en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="bc984-114">From a browser, connect to Ranger Admin UI.</span></span> <span data-ttu-id="bc984-115">Webbadressen är https://&lt;ClusterName>.azurehdinsight.net/Ranger/.</span><span class="sxs-lookup"><span data-stu-id="bc984-115">The URL is https://&lt;ClusterName>.azurehdinsight.net/Ranger/.</span></span>

   > [!NOTE]
   > <span data-ttu-id="bc984-116">Ranger använder andra autentiseringsuppgifter än Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="bc984-116">Ranger uses different credentials than Hadoop cluster.</span></span> <span data-ttu-id="bc984-117">Om du vill förhindra att webbläsare använder cachade Hadoop-autentiseringsuppgifter använder du ett nytt InPrivate-fönster för att ansluta till Ranger Admin-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="bc984-117">To prevent browsers using cached Hadoop credentials, use new inprivate browser window to connect to the Ranger Admin UI.</span></span>
   >
   >
2. <span data-ttu-id="bc984-118">Logga in med klusteradministratörens domänanvändarnamn och lösenord:</span><span class="sxs-lookup"><span data-stu-id="bc984-118">Log in using the cluster administrator domain user name and password:</span></span>

    ![Startsida för HDInsight domänanslutet Ranger](./media/hdinsight-domain-joined-run-hive/hdinsight-domain-joined-ranger-home-page.png)

    <span data-ttu-id="bc984-120">För närvarande fungerar Ranger bara med Yarn och Hive.</span><span class="sxs-lookup"><span data-stu-id="bc984-120">Currently, Ranger only works with Yarn and Hive.</span></span>

## <a name="create-domain-users"></a><span data-ttu-id="bc984-121">Skapa domänanvändare</span><span class="sxs-lookup"><span data-stu-id="bc984-121">Create Domain users</span></span>
<span data-ttu-id="bc984-122">I [Konfigurera domänanslutna HDInsight-kluster](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad) har du skapat hiveruser1 och hiveuser2.</span><span class="sxs-lookup"><span data-stu-id="bc984-122">In [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad), you have created hiveruser1 and hiveuser2.</span></span> <span data-ttu-id="bc984-123">Du använder dessa två användarkonton i den här lektionen.</span><span class="sxs-lookup"><span data-stu-id="bc984-123">You will use the two user account in this tutorial.</span></span>

## <a name="create-ranger-policies"></a><span data-ttu-id="bc984-124">Skapa Ranger-principer</span><span class="sxs-lookup"><span data-stu-id="bc984-124">Create Ranger policies</span></span>
<span data-ttu-id="bc984-125">I det här avsnittet skapar du två Ranger-principer för att komma åt hivesampletable.</span><span class="sxs-lookup"><span data-stu-id="bc984-125">In this section, you will create two Ranger policies for accessing hivesampletable.</span></span> <span data-ttu-id="bc984-126">Du kan ge select-behörighet för olika uppsättningar med kolumner.</span><span class="sxs-lookup"><span data-stu-id="bc984-126">You give select permission on different set of columns.</span></span> <span data-ttu-id="bc984-127">Båda användarna skapades i [Konfigurera domänanslutna HDInsight-kluster](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad).</span><span class="sxs-lookup"><span data-stu-id="bc984-127">Both users were created in [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad).</span></span>  <span data-ttu-id="bc984-128">I nästa avsnitt ska du testa två principer i Excel.</span><span class="sxs-lookup"><span data-stu-id="bc984-128">In the next section, you will test the two policies in Excel.</span></span>

<span data-ttu-id="bc984-129">**Skapa Ranger-principer**</span><span class="sxs-lookup"><span data-stu-id="bc984-129">**To create Ranger policies**</span></span>

1. <span data-ttu-id="bc984-130">Öppna Ranger Admin-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="bc984-130">Open Ranger Admin UI.</span></span> <span data-ttu-id="bc984-131">Se [Anslut till Apache Ranger Admin-gränssnittet](#connect-to-apache-ranager-admin-ui).</span><span class="sxs-lookup"><span data-stu-id="bc984-131">See [Connect to Apache Ranger Admin UI](#connect-to-apache-ranager-admin-ui).</span></span>
2. <span data-ttu-id="bc984-132">Klicka på **&lt;Klusternamn>_hive** under **Hive**.</span><span class="sxs-lookup"><span data-stu-id="bc984-132">Click **&lt;ClusterName>_hive**, under **Hive**.</span></span> <span data-ttu-id="bc984-133">Du bör se två förkonfigurerade principer.</span><span class="sxs-lookup"><span data-stu-id="bc984-133">You shall see two pre-configure policies.</span></span>
3. <span data-ttu-id="bc984-134">Klicka på **Lägg till ny princip** och ange sedan följande värden:</span><span class="sxs-lookup"><span data-stu-id="bc984-134">Click **Add New Policy**, and then enter the following values:</span></span>

   * <span data-ttu-id="bc984-135">Principnamn: read-hivesampletable-all</span><span class="sxs-lookup"><span data-stu-id="bc984-135">Policy name: read-hivesampletable-all</span></span>
   * <span data-ttu-id="bc984-136">Hive-database: standard</span><span class="sxs-lookup"><span data-stu-id="bc984-136">Hive Database: default</span></span>
   * <span data-ttu-id="bc984-137">tabell: hivesampletable</span><span class="sxs-lookup"><span data-stu-id="bc984-137">table: hivesampletable</span></span>
   * <span data-ttu-id="bc984-138">Hive-kolumn: *</span><span class="sxs-lookup"><span data-stu-id="bc984-138">Hive column: *</span></span>
   * <span data-ttu-id="bc984-139">Välj användare: hiveuser1</span><span class="sxs-lookup"><span data-stu-id="bc984-139">Select User: hiveuser1</span></span>
   * <span data-ttu-id="bc984-140">Behörigheter: select</span><span class="sxs-lookup"><span data-stu-id="bc984-140">Permissions: select</span></span>

     ![Konfigurera Ranger Hive-princip för domänansluten HDInsight](./media/hdinsight-domain-joined-run-hive/hdinsight-domain-joined-configure-ranger-policy.png)<span data-ttu-id="bc984-142">.</span><span class="sxs-lookup"><span data-stu-id="bc984-142">.</span></span>

     > [!NOTE]
     > <span data-ttu-id="bc984-143">Om en domänanvändare inte är ifylld i Välj användare, vänta en stund för att Ranger ska synkronisera med AAD.</span><span class="sxs-lookup"><span data-stu-id="bc984-143">If a domain user is not populated in Select User, wait a few moments for Ranger to sync with AAD.</span></span>
     >
     >
4. <span data-ttu-id="bc984-144">Klicka på **Lägg till** för att spara principen.</span><span class="sxs-lookup"><span data-stu-id="bc984-144">Click **Add** to save the policy.</span></span>
5. <span data-ttu-id="bc984-145">Upprepa de två sista stegen för att skapa en annan princip med följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="bc984-145">Repeat the last two steps to create another policy with the following properties:</span></span>

   * <span data-ttu-id="bc984-146">Principnamn: read-hivesampletable-devicemake</span><span class="sxs-lookup"><span data-stu-id="bc984-146">Policy name: read-hivesampletable-devicemake</span></span>
   * <span data-ttu-id="bc984-147">Hive-database: standard</span><span class="sxs-lookup"><span data-stu-id="bc984-147">Hive Database: default</span></span>
   * <span data-ttu-id="bc984-148">tabell: hivesampletable</span><span class="sxs-lookup"><span data-stu-id="bc984-148">table: hivesampletable</span></span>
   * <span data-ttu-id="bc984-149">Hive-kolumn: clientid, devicemake</span><span class="sxs-lookup"><span data-stu-id="bc984-149">Hive column: clientid, devicemake</span></span>
   * <span data-ttu-id="bc984-150">Välj användare: hiveuser2</span><span class="sxs-lookup"><span data-stu-id="bc984-150">Select User: hiveuser2</span></span>
   * <span data-ttu-id="bc984-151">Behörigheter: select</span><span class="sxs-lookup"><span data-stu-id="bc984-151">Permissions: select</span></span>

## <a name="create-hive-odbc-data-source"></a><span data-ttu-id="bc984-152">Skapa Hive ODBC-datakälla</span><span class="sxs-lookup"><span data-stu-id="bc984-152">Create Hive ODBC data source</span></span>
<span data-ttu-id="bc984-153">Du hittar anvisningarna i [Skapa Hive ODBC-datakällan](hdinsight-connect-excel-hive-odbc-driver.md).</span><span class="sxs-lookup"><span data-stu-id="bc984-153">The instructions can be found in [Create Hive ODBC data source](hdinsight-connect-excel-hive-odbc-driver.md).</span></span>  

    <span data-ttu-id="bc984-154">Egenskap</span><span class="sxs-lookup"><span data-stu-id="bc984-154">Property</span></span>|<span data-ttu-id="bc984-155">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="bc984-155">Description</span></span>
    ---|---
    <span data-ttu-id="bc984-156">Namn på datakälla</span><span class="sxs-lookup"><span data-stu-id="bc984-156">Data Source Name</span></span>|<span data-ttu-id="bc984-157">Namnge din datakälla</span><span class="sxs-lookup"><span data-stu-id="bc984-157">Give a name to your data source</span></span>
    <span data-ttu-id="bc984-158">Värd</span><span class="sxs-lookup"><span data-stu-id="bc984-158">Host</span></span>|<span data-ttu-id="bc984-159">Ange &lt;HDInsightClusterName>.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="bc984-159">Enter &lt;HDInsightClusterName>.azurehdinsight.net.</span></span> <span data-ttu-id="bc984-160">Till exempel myHDICluster.azurehdinsight.net</span><span class="sxs-lookup"><span data-stu-id="bc984-160">For example, myHDICluster.azurehdinsight.net</span></span>
    <span data-ttu-id="bc984-161">Port</span><span class="sxs-lookup"><span data-stu-id="bc984-161">Port</span></span>|<span data-ttu-id="bc984-162">Använd <strong>443</strong>.</span><span class="sxs-lookup"><span data-stu-id="bc984-162">Use <strong>443</strong>.</span></span> <span data-ttu-id="bc984-163">(Den här porten har ändrats från 563 till 443.)</span><span class="sxs-lookup"><span data-stu-id="bc984-163">(This port has been changed from 563 to 443.)</span></span>
    <span data-ttu-id="bc984-164">Databas</span><span class="sxs-lookup"><span data-stu-id="bc984-164">Database</span></span>|<span data-ttu-id="bc984-165">Använd <strong>Standard</strong>.</span><span class="sxs-lookup"><span data-stu-id="bc984-165">Use <strong>Default</strong>.</span></span>
    <span data-ttu-id="bc984-166">Hive-servertyp</span><span class="sxs-lookup"><span data-stu-id="bc984-166">Hive Server Type</span></span>|<span data-ttu-id="bc984-167">Välj <strong>Hive Server 2</strong></span><span class="sxs-lookup"><span data-stu-id="bc984-167">Select <strong>Hive Server 2</strong></span></span>
    <span data-ttu-id="bc984-168">Mekanism</span><span class="sxs-lookup"><span data-stu-id="bc984-168">Mechanism</span></span>|<span data-ttu-id="bc984-169">Välj <strong>Azure HDInsight-tjänst</strong></span><span class="sxs-lookup"><span data-stu-id="bc984-169">Select <strong>Azure HDInsight Service</strong></span></span>
    <span data-ttu-id="bc984-170">HTTP-sökväg</span><span class="sxs-lookup"><span data-stu-id="bc984-170">HTTP Path</span></span>|<span data-ttu-id="bc984-171">Lämna tomt.</span><span class="sxs-lookup"><span data-stu-id="bc984-171">Leave it blank.</span></span>
    <span data-ttu-id="bc984-172">Användarnamn</span><span class="sxs-lookup"><span data-stu-id="bc984-172">User Name</span></span>|<span data-ttu-id="bc984-173">Ange hiveuser1@contoso158.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="bc984-173">Enter hiveuser1@contoso158.onmicrosoft.com.</span></span> <span data-ttu-id="bc984-174">Uppdatera domännamnet om det är olika.</span><span class="sxs-lookup"><span data-stu-id="bc984-174">Update the domain name if it is different.</span></span>
    <span data-ttu-id="bc984-175">Lösenord</span><span class="sxs-lookup"><span data-stu-id="bc984-175">Password</span></span>|<span data-ttu-id="bc984-176">Ange lösenordet för hiveuser1.</span><span class="sxs-lookup"><span data-stu-id="bc984-176">Enter the password for hiveuser1.</span></span>
    </table>

<span data-ttu-id="bc984-177">Se till att klicka på **Test** innan du sparar datakällan.</span><span class="sxs-lookup"><span data-stu-id="bc984-177">Make sure to click **Test** before saving the data source.</span></span>

## <a name="import-data-into-excel-from-hdinsight"></a><span data-ttu-id="bc984-178">Importera data till Excel från HDInsight</span><span class="sxs-lookup"><span data-stu-id="bc984-178">Import data into Excel from HDInsight</span></span>
<span data-ttu-id="bc984-179">I det sista avsnittet konfigurerade du två principer.</span><span class="sxs-lookup"><span data-stu-id="bc984-179">In the last section, you have configured two policies.</span></span>  <span data-ttu-id="bc984-180">hiveuser1 har select-behörighet för alla kolumner och hiveuser2 har select-behörighet på två kolumner.</span><span class="sxs-lookup"><span data-stu-id="bc984-180">hiveuser1 has the select permission on all the columns, and hiveuser2 has the select permission on two columns.</span></span> <span data-ttu-id="bc984-181">I det här avsnittet personifierar du de två användarna för att importera data till Excel.</span><span class="sxs-lookup"><span data-stu-id="bc984-181">In this section, you impersonate the two users to import data into Excel.</span></span>

1. <span data-ttu-id="bc984-182">Öppna en ny eller befintlig arbetsbok i Excel.</span><span class="sxs-lookup"><span data-stu-id="bc984-182">Open a new or existing workbook in Excel.</span></span>
2. <span data-ttu-id="bc984-183">På fliken **Data** klickar du på **From Other Data Sources** (Från andra datakällor) och sedan klickar du på **Från guiden Dataanslutning** för att starta den **guiden Dataanslutning**.</span><span class="sxs-lookup"><span data-stu-id="bc984-183">From the **Data** tab, click **From Other Data Sources**, and then click **From Data Connection Wizard** to launch the **Data Connection Wizard**.</span></span>

    <span data-ttu-id="bc984-184">![Öppna guiden Dataanslutning][img-hdi-simbahiveodbc.excel.dataconnection]</span><span class="sxs-lookup"><span data-stu-id="bc984-184">![Open data connection wizard][img-hdi-simbahiveodbc.excel.dataconnection]</span></span>
3. <span data-ttu-id="bc984-185">Välj **ODBC DSN** som datakälla och klicka sedan på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="bc984-185">Select **ODBC DSN** as the data source, and then click **Next**.</span></span>
4. <span data-ttu-id="bc984-186">Från ODBC-datakällor väljer du datakällsnamnet som du skapade i det föregående steget och sedan klickar du på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="bc984-186">From ODBC data sources, select the data source name that you created in the previous step, and then  click **Next**.</span></span>
5. <span data-ttu-id="bc984-187">Ange lösenordet för klustret i guiden igen och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="bc984-187">Re-enter the password for the cluster in the wizard, and then click **OK**.</span></span> <span data-ttu-id="bc984-188">Vänta tills dialogrutan **Markera databas och tabell** öppnas.</span><span class="sxs-lookup"><span data-stu-id="bc984-188">Wait for the **Select Database and Table** dialog to open.</span></span> <span data-ttu-id="bc984-189">Det kan ta några sekunder.</span><span class="sxs-lookup"><span data-stu-id="bc984-189">This can take a few seconds.</span></span>
6. <span data-ttu-id="bc984-190">Välj **hivesampletable** och klicka sedan på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="bc984-190">Select **hivesampletable**, and then click **Next**.</span></span>
7. <span data-ttu-id="bc984-191">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="bc984-191">Click **Finish**.</span></span>
8. <span data-ttu-id="bc984-192">I dialogrutan **Importera data** kan du ändra eller specificera frågan.</span><span class="sxs-lookup"><span data-stu-id="bc984-192">In the **Import Data** dialog, you can change or specify the query.</span></span> <span data-ttu-id="bc984-193">Gör det genom att klicka på **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="bc984-193">To do so, click **Properties**.</span></span> <span data-ttu-id="bc984-194">Det kan ta några sekunder.</span><span class="sxs-lookup"><span data-stu-id="bc984-194">This can take a few seconds.</span></span>
9. <span data-ttu-id="bc984-195">Klicka på fliken **Definition**.</span><span class="sxs-lookup"><span data-stu-id="bc984-195">Click the **Definition** tab.</span></span> <span data-ttu-id="bc984-196">Kommandotexten är:</span><span class="sxs-lookup"><span data-stu-id="bc984-196">The command text is:</span></span>

       SELECT * FROM "HIVE"."default"."hivesampletable"

   <span data-ttu-id="bc984-197">Enligt Ranger-principerna du definierade har hiveuser1 select-behörighet för alla kolumner.</span><span class="sxs-lookup"><span data-stu-id="bc984-197">By the Ranger policies you defined,  hiveuser1 has select permission on all the columns.</span></span>  <span data-ttu-id="bc984-198">Så den här frågan fungerar med autentiseringsuppgifterna för hiveuser1, men frågan fungerar inte med autentiseringsuppgifterna för hiveuser2.</span><span class="sxs-lookup"><span data-stu-id="bc984-198">So this query works with hiveuser1's credentials, but this query does not not work with hiveuser2's credentials.</span></span>

   <span data-ttu-id="bc984-199">![Anslutningsegenskaper][img-hdi-simbahiveodbc-excel-connectionproperties]</span><span class="sxs-lookup"><span data-stu-id="bc984-199">![Connection Properties][img-hdi-simbahiveodbc-excel-connectionproperties]</span></span>
10. <span data-ttu-id="bc984-200">Klicka på **OK** för att stänga dialogrutan Anslutningsegenskaper.</span><span class="sxs-lookup"><span data-stu-id="bc984-200">Click **OK** to close the Connection Properties dialog.</span></span>
11. <span data-ttu-id="bc984-201">Klicka på **OK** för att stänga dialogrutan **Importera data**.</span><span class="sxs-lookup"><span data-stu-id="bc984-201">Click **OK** to close the **Import Data** dialog.</span></span>  
12. <span data-ttu-id="bc984-202">Ange lösenordet för hiveuser1 igen och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="bc984-202">Reenter the password for hiveuser1, and then click **OK**.</span></span> <span data-ttu-id="bc984-203">Det tar några sekunder innan data har importerats till Excel.</span><span class="sxs-lookup"><span data-stu-id="bc984-203">It takes a few seconds before data gets imported to Excel.</span></span> <span data-ttu-id="bc984-204">När det är klart bör du se 11 kolumner med data.</span><span class="sxs-lookup"><span data-stu-id="bc984-204">When it is done, you shall see 11 columns of data.</span></span>

<span data-ttu-id="bc984-205">Om du vill testa den andra principen (read-hivesampletable-devicemake) du skapade i det sista avsnittet</span><span class="sxs-lookup"><span data-stu-id="bc984-205">To test the second policy (read-hivesampletable-devicemake) you created in the last section</span></span>

1. <span data-ttu-id="bc984-206">Lägg till ett nytt kalkylblad i Excel.</span><span class="sxs-lookup"><span data-stu-id="bc984-206">Add a new sheet in Excel.</span></span>
2. <span data-ttu-id="bc984-207">Följ föregående procedur för att importera data.</span><span class="sxs-lookup"><span data-stu-id="bc984-207">Follow the last procedure to import the data.</span></span>  <span data-ttu-id="bc984-208">Den enda ändringen du gör är att använda autentiseringsuppgifterna för hiveuser2 i stället för autentiseringsuppgifterna för hiveuser1.</span><span class="sxs-lookup"><span data-stu-id="bc984-208">The only change you will make is to use hiveuser2's credentials instead of hiveuser1's.</span></span> <span data-ttu-id="bc984-209">Detta fungerar inte eftersom hiveuser2 endast har behörighet att visa två kolumner.</span><span class="sxs-lookup"><span data-stu-id="bc984-209">This will fail because hiveuser2 only has permission to see two columns.</span></span> <span data-ttu-id="bc984-210">Du bör se följande fel:</span><span class="sxs-lookup"><span data-stu-id="bc984-210">You shall get the following error:</span></span>

        [Microsoft][HiveODBC] (35) Error from Hive: error code: '40000' error message: 'Error while compiling statement: FAILED: HiveAccessControlException Permission denied: user [hiveuser2] does not have [SELECT] privilege on [default/hivesampletable/clientid,country ...]'.
3. <span data-ttu-id="bc984-211">Följ samma procedur för att importera data.</span><span class="sxs-lookup"><span data-stu-id="bc984-211">Follow the same procedure to import data.</span></span> <span data-ttu-id="bc984-212">Den här gången använder du autentiseringsuppgifterna för hiveuser2 och ändrar också select-uttrycket från:</span><span class="sxs-lookup"><span data-stu-id="bc984-212">This time, use hiveuser2's credentials, and also modify the select statement from:</span></span>

        SELECT * FROM "HIVE"."default"."hivesampletable"

    <span data-ttu-id="bc984-213">till:</span><span class="sxs-lookup"><span data-stu-id="bc984-213">to:</span></span>

        SELECT clientid, devicemake FROM "HIVE"."default"."hivesampletable"

    <span data-ttu-id="bc984-214">När det är klart bör du se två kolumner med importerade data.</span><span class="sxs-lookup"><span data-stu-id="bc984-214">When it is done, you shall see two columns of data imported.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bc984-215">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bc984-215">Next steps</span></span>
* <span data-ttu-id="bc984-216">Om du vill konfigurera ett domänanslutet HDInsight-kluster kan du läsa i [Konfigurera domänanslutna HDInsight-kluster](hdinsight-domain-joined-configure.md).</span><span class="sxs-lookup"><span data-stu-id="bc984-216">For configuring a Domain-joined HDInsight cluster, see [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md).</span></span>
* <span data-ttu-id="bc984-217">Om du vill hantera ett domänanslutet HDInsight-kluster kan du läsa i [Hantera domänanslutna HDInsight-kluster](hdinsight-domain-joined-manage.md).</span><span class="sxs-lookup"><span data-stu-id="bc984-217">For managing a Domain-joined HDInsight clusters, see [Manage Domain-joined HDInsight clusters](hdinsight-domain-joined-manage.md).</span></span>
* <span data-ttu-id="bc984-218">För att köra Hive-frågor med SSH på domänanslutna HDInsight-kluster, se [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).</span><span class="sxs-lookup"><span data-stu-id="bc984-218">For running Hive queries using SSH on Domain-joined HDInsight clusters, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).</span></span>
* <span data-ttu-id="bc984-219">Om du vill ansluta Hive med Hive JDBC kan du läsa i [Anslut till Hive på Azure HDInsight med Hive JDBC-drivrutin](hdinsight-connect-hive-jdbc-driver.md)</span><span class="sxs-lookup"><span data-stu-id="bc984-219">For Connecting Hive using Hive JDBC, see [Connect to Hive on Azure HDInsight using the Hive JDBC driver](hdinsight-connect-hive-jdbc-driver.md)</span></span>
* <span data-ttu-id="bc984-220">Om du vill ansluta Excel till Hadoop med Hive ODBC kan du läsa i [Anslut Excel till Hadoop med Microsoft Hive ODBC-drivrutin](hdinsight-connect-excel-hive-odbc-driver.md)</span><span class="sxs-lookup"><span data-stu-id="bc984-220">For connecting Excel to Hadoop using Hive ODBC, see [Connect Excel to Hadoop with the Microsoft Hive ODBC drive](hdinsight-connect-excel-hive-odbc-driver.md)</span></span>
* <span data-ttu-id="bc984-221">Om du vill ansluta Excel till Hadoop med Power Query kan du läsa i [Anslut Excel till Hadoop med hjälp av Power Query](hdinsight-connect-excel-power-query.md)</span><span class="sxs-lookup"><span data-stu-id="bc984-221">For connecting Excel to Hadoop using Power Query, see [Connect Excel to Hadoop by using Power Query](hdinsight-connect-excel-power-query.md)</span></span>
