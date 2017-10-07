---
title: "principer för aaaConfigure Hive i HDInsight med domänanslutna - Azure | Microsoft Docs"
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
ms.openlocfilehash: 56f2bf9d872abc5f772b886fcf91c2e2422092f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hive-policies-in-domain-joined-hdinsight-preview"></a><span data-ttu-id="a195a-103">Konfigurera Hive-principer i domänanslutna HDInsight (förhandsversion)</span><span class="sxs-lookup"><span data-stu-id="a195a-103">Configure Hive policies in Domain-joined HDInsight (Preview)</span></span>
<span data-ttu-id="a195a-104">Lär dig hur tooconfigure Apache Ranger principer för Hive.</span><span class="sxs-lookup"><span data-stu-id="a195a-104">Learn how tooconfigure Apache Ranger policies for Hive.</span></span> <span data-ttu-id="a195a-105">I den här artikeln får skapa du två Ranger principer toorestrict åtkomst toohello hivesampletable.</span><span class="sxs-lookup"><span data-stu-id="a195a-105">In this article, you create two Ranger policies toorestrict access toohello hivesampletable.</span></span> <span data-ttu-id="a195a-106">Hej hivesampletable medföljer HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="a195a-106">hello hivesampletable comes with HDInsight clusters.</span></span> <span data-ttu-id="a195a-107">När du har konfigurerat hello principer kan använda du Excel och ODBC-drivrutinen tooconnect tooHive tabeller i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a195a-107">After you have configured hello policies, you use Excel and ODBC driver tooconnect tooHive tables in HDInsight.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a195a-108">Krav</span><span class="sxs-lookup"><span data-stu-id="a195a-108">Prerequisites</span></span>
* <span data-ttu-id="a195a-109">Ett domänanslutet HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="a195a-109">A Domain-joined HDInsight cluster.</span></span> <span data-ttu-id="a195a-110">Se [Konfigurera domänanslutna HDInsight-kluster](hdinsight-domain-joined-configure.md).</span><span class="sxs-lookup"><span data-stu-id="a195a-110">See [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md).</span></span>
* <span data-ttu-id="a195a-111">En arbetsstation med Office 2016, Office 2013 Professional Plus, Office 365 Pro Plus, fristående Excel 2013 eller Office 2010 Professional Plus.</span><span class="sxs-lookup"><span data-stu-id="a195a-111">A workstation with Office 2016, Office 2013 Professional Plus, Office 365 Pro Plus, Excel 2013 Standalone, or Office 2010 Professional Plus.</span></span>

## <a name="connect-tooapache-ranger-admin-ui"></a><span data-ttu-id="a195a-112">Ansluta tooApache Ranger Admin UI</span><span class="sxs-lookup"><span data-stu-id="a195a-112">Connect tooApache Ranger Admin UI</span></span>
<span data-ttu-id="a195a-113">**tooconnect tooRanger Admin UI**</span><span class="sxs-lookup"><span data-stu-id="a195a-113">**tooconnect tooRanger Admin UI**</span></span>

1. <span data-ttu-id="a195a-114">Ansluta tooRanger Admin UI från en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="a195a-114">From a browser, connect tooRanger Admin UI.</span></span> <span data-ttu-id="a195a-115">hello URL: en är https://&lt;klusternamn >.azurehdinsight.net/Ranger/.</span><span class="sxs-lookup"><span data-stu-id="a195a-115">hello URL is https://&lt;ClusterName>.azurehdinsight.net/Ranger/.</span></span>

   > [!NOTE]
   > <span data-ttu-id="a195a-116">Ranger använder andra autentiseringsuppgifter än Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="a195a-116">Ranger uses different credentials than Hadoop cluster.</span></span> <span data-ttu-id="a195a-117">tooprevent webbläsare med den cachelagrade Hadoop autentiseringsuppgifter använder nya InPrivate-webbläsaren fönstret tooconnect toohello Ranger Admin-Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="a195a-117">tooprevent browsers using cached Hadoop credentials, use new inprivate browser window tooconnect toohello Ranger Admin UI.</span></span>
   >
   >
2. <span data-ttu-id="a195a-118">Logga in med hello klustret administratör domän, användarnamn och lösenord:</span><span class="sxs-lookup"><span data-stu-id="a195a-118">Log in using hello cluster administrator domain user name and password:</span></span>

    ![Startsida för HDInsight domänanslutet Ranger](./media/hdinsight-domain-joined-run-hive/hdinsight-domain-joined-ranger-home-page.png)

    <span data-ttu-id="a195a-120">För närvarande fungerar Ranger bara med Yarn och Hive.</span><span class="sxs-lookup"><span data-stu-id="a195a-120">Currently, Ranger only works with Yarn and Hive.</span></span>

## <a name="create-domain-users"></a><span data-ttu-id="a195a-121">Skapa domänanvändare</span><span class="sxs-lookup"><span data-stu-id="a195a-121">Create Domain users</span></span>
<span data-ttu-id="a195a-122">I [Konfigurera domänanslutna HDInsight-kluster](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad) har du skapat hiveruser1 och hiveuser2.</span><span class="sxs-lookup"><span data-stu-id="a195a-122">In [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad), you have created hiveruser1 and hiveuser2.</span></span> <span data-ttu-id="a195a-123">Du använder hello två användarkonto i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="a195a-123">You will use hello two user account in this tutorial.</span></span>

## <a name="create-ranger-policies"></a><span data-ttu-id="a195a-124">Skapa Ranger-principer</span><span class="sxs-lookup"><span data-stu-id="a195a-124">Create Ranger policies</span></span>
<span data-ttu-id="a195a-125">I det här avsnittet skapar du två Ranger-principer för att komma åt hivesampletable.</span><span class="sxs-lookup"><span data-stu-id="a195a-125">In this section, you will create two Ranger policies for accessing hivesampletable.</span></span> <span data-ttu-id="a195a-126">Du kan ge select-behörighet för olika uppsättningar med kolumner.</span><span class="sxs-lookup"><span data-stu-id="a195a-126">You give select permission on different set of columns.</span></span> <span data-ttu-id="a195a-127">Båda användarna skapades i [Konfigurera domänanslutna HDInsight-kluster](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad).</span><span class="sxs-lookup"><span data-stu-id="a195a-127">Both users were created in [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad).</span></span>  <span data-ttu-id="a195a-128">I nästa avsnitt om hello, ska du testa hello två principer i Excel.</span><span class="sxs-lookup"><span data-stu-id="a195a-128">In hello next section, you will test hello two policies in Excel.</span></span>

<span data-ttu-id="a195a-129">**toocreate Ranger principer**</span><span class="sxs-lookup"><span data-stu-id="a195a-129">**toocreate Ranger policies**</span></span>

1. <span data-ttu-id="a195a-130">Öppna Ranger Admin-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="a195a-130">Open Ranger Admin UI.</span></span> <span data-ttu-id="a195a-131">Se [ansluta tooApache Ranger Admin UI](#connect-to-apache-ranager-admin-ui).</span><span class="sxs-lookup"><span data-stu-id="a195a-131">See [Connect tooApache Ranger Admin UI](#connect-to-apache-ranager-admin-ui).</span></span>
2. <span data-ttu-id="a195a-132">Klicka på **&lt;Klusternamn>_hive** under **Hive**.</span><span class="sxs-lookup"><span data-stu-id="a195a-132">Click **&lt;ClusterName>_hive**, under **Hive**.</span></span> <span data-ttu-id="a195a-133">Du bör se två förkonfigurerade principer.</span><span class="sxs-lookup"><span data-stu-id="a195a-133">You shall see two pre-configure policies.</span></span>
3. <span data-ttu-id="a195a-134">Klicka på **Lägg till ny princip**, och ange sedan hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="a195a-134">Click **Add New Policy**, and then enter hello following values:</span></span>

   * <span data-ttu-id="a195a-135">Principnamn: read-hivesampletable-all</span><span class="sxs-lookup"><span data-stu-id="a195a-135">Policy name: read-hivesampletable-all</span></span>
   * <span data-ttu-id="a195a-136">Hive-database: standard</span><span class="sxs-lookup"><span data-stu-id="a195a-136">Hive Database: default</span></span>
   * <span data-ttu-id="a195a-137">tabell: hivesampletable</span><span class="sxs-lookup"><span data-stu-id="a195a-137">table: hivesampletable</span></span>
   * <span data-ttu-id="a195a-138">Hive-kolumn: *</span><span class="sxs-lookup"><span data-stu-id="a195a-138">Hive column: *</span></span>
   * <span data-ttu-id="a195a-139">Välj användare: hiveuser1</span><span class="sxs-lookup"><span data-stu-id="a195a-139">Select User: hiveuser1</span></span>
   * <span data-ttu-id="a195a-140">Behörigheter: select</span><span class="sxs-lookup"><span data-stu-id="a195a-140">Permissions: select</span></span>

     ![Konfigurera Ranger Hive-princip för domänansluten HDInsight](./media/hdinsight-domain-joined-run-hive/hdinsight-domain-joined-configure-ranger-policy.png)<span data-ttu-id="a195a-142">.</span><span class="sxs-lookup"><span data-stu-id="a195a-142">.</span></span>

     > [!NOTE]
     > <span data-ttu-id="a195a-143">Om en domänanvändare inte har fyllts i i Välj användare, vänta en stund för Ranger toosync med AAD.</span><span class="sxs-lookup"><span data-stu-id="a195a-143">If a domain user is not populated in Select User, wait a few moments for Ranger toosync with AAD.</span></span>
     >
     >
4. <span data-ttu-id="a195a-144">Klicka på **Lägg till** toosave hello princip.</span><span class="sxs-lookup"><span data-stu-id="a195a-144">Click **Add** toosave hello policy.</span></span>
5. <span data-ttu-id="a195a-145">Upprepa hello två sista stegen toocreate en annan princip med hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="a195a-145">Repeat hello last two steps toocreate another policy with hello following properties:</span></span>

   * <span data-ttu-id="a195a-146">Principnamn: read-hivesampletable-devicemake</span><span class="sxs-lookup"><span data-stu-id="a195a-146">Policy name: read-hivesampletable-devicemake</span></span>
   * <span data-ttu-id="a195a-147">Hive-database: standard</span><span class="sxs-lookup"><span data-stu-id="a195a-147">Hive Database: default</span></span>
   * <span data-ttu-id="a195a-148">tabell: hivesampletable</span><span class="sxs-lookup"><span data-stu-id="a195a-148">table: hivesampletable</span></span>
   * <span data-ttu-id="a195a-149">Hive-kolumn: clientid, devicemake</span><span class="sxs-lookup"><span data-stu-id="a195a-149">Hive column: clientid, devicemake</span></span>
   * <span data-ttu-id="a195a-150">Välj användare: hiveuser2</span><span class="sxs-lookup"><span data-stu-id="a195a-150">Select User: hiveuser2</span></span>
   * <span data-ttu-id="a195a-151">Behörigheter: select</span><span class="sxs-lookup"><span data-stu-id="a195a-151">Permissions: select</span></span>

## <a name="create-hive-odbc-data-source"></a><span data-ttu-id="a195a-152">Skapa Hive ODBC-datakälla</span><span class="sxs-lookup"><span data-stu-id="a195a-152">Create Hive ODBC data source</span></span>
<span data-ttu-id="a195a-153">hello anvisningar finns i [skapa Hive ODBC-datakällan](hdinsight-connect-excel-hive-odbc-driver.md).</span><span class="sxs-lookup"><span data-stu-id="a195a-153">hello instructions can be found in [Create Hive ODBC data source](hdinsight-connect-excel-hive-odbc-driver.md).</span></span>  

    <span data-ttu-id="a195a-154">Egenskap</span><span class="sxs-lookup"><span data-stu-id="a195a-154">Property</span></span>|<span data-ttu-id="a195a-155">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a195a-155">Description</span></span>
    ---|---
    <span data-ttu-id="a195a-156">Namn på datakälla</span><span class="sxs-lookup"><span data-stu-id="a195a-156">Data Source Name</span></span>|<span data-ttu-id="a195a-157">Ge en namnet tooyour-datakälla</span><span class="sxs-lookup"><span data-stu-id="a195a-157">Give a name tooyour data source</span></span>
    <span data-ttu-id="a195a-158">Värd</span><span class="sxs-lookup"><span data-stu-id="a195a-158">Host</span></span>|<span data-ttu-id="a195a-159">Ange &lt;HDInsightClusterName>.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="a195a-159">Enter &lt;HDInsightClusterName>.azurehdinsight.net.</span></span> <span data-ttu-id="a195a-160">Till exempel myHDICluster.azurehdinsight.net</span><span class="sxs-lookup"><span data-stu-id="a195a-160">For example, myHDICluster.azurehdinsight.net</span></span>
    <span data-ttu-id="a195a-161">Port</span><span class="sxs-lookup"><span data-stu-id="a195a-161">Port</span></span>|<span data-ttu-id="a195a-162">Använd <strong>443</strong>.</span><span class="sxs-lookup"><span data-stu-id="a195a-162">Use <strong>443</strong>.</span></span> <span data-ttu-id="a195a-163">(Den här porten har ändrats från 563 too443.)</span><span class="sxs-lookup"><span data-stu-id="a195a-163">(This port has been changed from 563 too443.)</span></span>
    <span data-ttu-id="a195a-164">Databas</span><span class="sxs-lookup"><span data-stu-id="a195a-164">Database</span></span>|<span data-ttu-id="a195a-165">Använd <strong>Standard</strong>.</span><span class="sxs-lookup"><span data-stu-id="a195a-165">Use <strong>Default</strong>.</span></span>
    <span data-ttu-id="a195a-166">Hive-servertyp</span><span class="sxs-lookup"><span data-stu-id="a195a-166">Hive Server Type</span></span>|<span data-ttu-id="a195a-167">Välj <strong>Hive Server 2</strong></span><span class="sxs-lookup"><span data-stu-id="a195a-167">Select <strong>Hive Server 2</strong></span></span>
    <span data-ttu-id="a195a-168">Mekanism</span><span class="sxs-lookup"><span data-stu-id="a195a-168">Mechanism</span></span>|<span data-ttu-id="a195a-169">Välj <strong>Azure HDInsight-tjänst</strong></span><span class="sxs-lookup"><span data-stu-id="a195a-169">Select <strong>Azure HDInsight Service</strong></span></span>
    <span data-ttu-id="a195a-170">HTTP-sökväg</span><span class="sxs-lookup"><span data-stu-id="a195a-170">HTTP Path</span></span>|<span data-ttu-id="a195a-171">Lämna tomt.</span><span class="sxs-lookup"><span data-stu-id="a195a-171">Leave it blank.</span></span>
    <span data-ttu-id="a195a-172">Användarnamn</span><span class="sxs-lookup"><span data-stu-id="a195a-172">User Name</span></span>|<span data-ttu-id="a195a-173">Ange hiveuser1@contoso158.onmicrosoft.com. Uppdatera hello domännamnet om det är olika.</span><span class="sxs-lookup"><span data-stu-id="a195a-173">Enter hiveuser1@contoso158.onmicrosoft.com. Update hello domain name if it is different.</span></span>
    <span data-ttu-id="a195a-174">Lösenord</span><span class="sxs-lookup"><span data-stu-id="a195a-174">Password</span></span>|<span data-ttu-id="a195a-175">Ange hello lösenord för hiveuser1.</span><span class="sxs-lookup"><span data-stu-id="a195a-175">Enter hello password for hiveuser1.</span></span>
    </table>

<span data-ttu-id="a195a-176">Se till att tooclick **Test** innan du sparar hello-datakälla.</span><span class="sxs-lookup"><span data-stu-id="a195a-176">Make sure tooclick **Test** before saving hello data source.</span></span>

## <a name="import-data-into-excel-from-hdinsight"></a><span data-ttu-id="a195a-177">Importera data till Excel från HDInsight</span><span class="sxs-lookup"><span data-stu-id="a195a-177">Import data into Excel from HDInsight</span></span>
<span data-ttu-id="a195a-178">Du har konfigurerat två principer under hello senaste.</span><span class="sxs-lookup"><span data-stu-id="a195a-178">In hello last section, you have configured two policies.</span></span>  <span data-ttu-id="a195a-179">hiveuser1 har hello markerar behörighet på alla hello kolumner och hiveuser2 har hello markerar behörighet på två kolumner.</span><span class="sxs-lookup"><span data-stu-id="a195a-179">hiveuser1 has hello select permission on all hello columns, and hiveuser2 has hello select permission on two columns.</span></span> <span data-ttu-id="a195a-180">I det här avsnittet personifiera hello två användare tooimport data till Excel.</span><span class="sxs-lookup"><span data-stu-id="a195a-180">In this section, you impersonate hello two users tooimport data into Excel.</span></span>

1. <span data-ttu-id="a195a-181">Öppna en ny eller befintlig arbetsbok i Excel.</span><span class="sxs-lookup"><span data-stu-id="a195a-181">Open a new or existing workbook in Excel.</span></span>
2. <span data-ttu-id="a195a-182">Från hello **Data** klickar du på **från andra datakällor**, och klicka sedan på **från guiden** toolaunch hello **Dataanslutningsguiden**.</span><span class="sxs-lookup"><span data-stu-id="a195a-182">From hello **Data** tab, click **From Other Data Sources**, and then click **From Data Connection Wizard** toolaunch hello **Data Connection Wizard**.</span></span>

    <span data-ttu-id="a195a-183">![Öppna guiden Dataanslutning][img-hdi-simbahiveodbc.excel.dataconnection]</span><span class="sxs-lookup"><span data-stu-id="a195a-183">![Open data connection wizard][img-hdi-simbahiveodbc.excel.dataconnection]</span></span>
3. <span data-ttu-id="a195a-184">Välj **ODBC DSN** som hello datakällan och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="a195a-184">Select **ODBC DSN** as hello data source, and then click **Next**.</span></span>
4. <span data-ttu-id="a195a-185">ODBC-datakällor, Välj hello data datakällans namn som du skapade i föregående steg i hello, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="a195a-185">From ODBC data sources, select hello data source name that you created in hello previous step, and then  click **Next**.</span></span>
5. <span data-ttu-id="a195a-186">Anger hello lösenordet för hello-kluster i hello guiden igen och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="a195a-186">Re-enter hello password for hello cluster in hello wizard, and then click **OK**.</span></span> <span data-ttu-id="a195a-187">Vänta tills hello **Välj databas och tabell** dialogrutan tooopen.</span><span class="sxs-lookup"><span data-stu-id="a195a-187">Wait for hello **Select Database and Table** dialog tooopen.</span></span> <span data-ttu-id="a195a-188">Det kan ta några sekunder.</span><span class="sxs-lookup"><span data-stu-id="a195a-188">This can take a few seconds.</span></span>
6. <span data-ttu-id="a195a-189">Välj **hivesampletable** och klicka sedan på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="a195a-189">Select **hivesampletable**, and then click **Next**.</span></span>
7. <span data-ttu-id="a195a-190">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="a195a-190">Click **Finish**.</span></span>
8. <span data-ttu-id="a195a-191">I hello **importera Data** dialogrutan kan du ändra eller ange hello-fråga.</span><span class="sxs-lookup"><span data-stu-id="a195a-191">In hello **Import Data** dialog, you can change or specify hello query.</span></span> <span data-ttu-id="a195a-192">toodo Klicka på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="a195a-192">toodo so, click **Properties**.</span></span> <span data-ttu-id="a195a-193">Det kan ta några sekunder.</span><span class="sxs-lookup"><span data-stu-id="a195a-193">This can take a few seconds.</span></span>
9. <span data-ttu-id="a195a-194">Klicka på hello **Definition** fliken hello Kommandotext är:</span><span class="sxs-lookup"><span data-stu-id="a195a-194">Click hello **Definition** tab. hello command text is:</span></span>

       SELECT * FROM "HIVE"."default"."hivesampletable"

   <span data-ttu-id="a195a-195">Hello Ranger principer som du har definierat, har hiveuser1 select-behörighet på alla hello-kolumner.</span><span class="sxs-lookup"><span data-stu-id="a195a-195">By hello Ranger policies you defined,  hiveuser1 has select permission on all hello columns.</span></span>  <span data-ttu-id="a195a-196">Så den här frågan fungerar med autentiseringsuppgifterna för hiveuser1, men frågan fungerar inte med autentiseringsuppgifterna för hiveuser2.</span><span class="sxs-lookup"><span data-stu-id="a195a-196">So this query works with hiveuser1's credentials, but this query does not not work with hiveuser2's credentials.</span></span>

   <span data-ttu-id="a195a-197">![Anslutningsegenskaper][img-hdi-simbahiveodbc-excel-connectionproperties]</span><span class="sxs-lookup"><span data-stu-id="a195a-197">![Connection Properties][img-hdi-simbahiveodbc-excel-connectionproperties]</span></span>
10. <span data-ttu-id="a195a-198">Klicka på **OK** tooclose hello anslutningsegenskaper.</span><span class="sxs-lookup"><span data-stu-id="a195a-198">Click **OK** tooclose hello Connection Properties dialog.</span></span>
11. <span data-ttu-id="a195a-199">Klicka på **OK** tooclose hello **importera Data** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a195a-199">Click **OK** tooclose hello **Import Data** dialog.</span></span>  
12. <span data-ttu-id="a195a-200">Ange hello lösenordet för hiveuser1 igen och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="a195a-200">Reenter hello password for hiveuser1, and then click **OK**.</span></span> <span data-ttu-id="a195a-201">Det tar några sekunder innan data hämtar importerade tooExcel.</span><span class="sxs-lookup"><span data-stu-id="a195a-201">It takes a few seconds before data gets imported tooExcel.</span></span> <span data-ttu-id="a195a-202">När det är klart bör du se 11 kolumner med data.</span><span class="sxs-lookup"><span data-stu-id="a195a-202">When it is done, you shall see 11 columns of data.</span></span>

<span data-ttu-id="a195a-203">tootest hello andra princip (Läs-hivesampletable-devicemake) som du skapade i hello sista avsnittet</span><span class="sxs-lookup"><span data-stu-id="a195a-203">tootest hello second policy (read-hivesampletable-devicemake) you created in hello last section</span></span>

1. <span data-ttu-id="a195a-204">Lägg till ett nytt kalkylblad i Excel.</span><span class="sxs-lookup"><span data-stu-id="a195a-204">Add a new sheet in Excel.</span></span>
2. <span data-ttu-id="a195a-205">Följ hello sista proceduren tooimport hello data.</span><span class="sxs-lookup"><span data-stu-id="a195a-205">Follow hello last procedure tooimport hello data.</span></span>  <span data-ttu-id="a195a-206">hello endast ändringar du gör är toouse hiveuser2 autentiseringsuppgifter i stället för hiveuser1's.</span><span class="sxs-lookup"><span data-stu-id="a195a-206">hello only change you will make is toouse hiveuser2's credentials instead of hiveuser1's.</span></span> <span data-ttu-id="a195a-207">Detta kommer att misslyckas eftersom hiveuser2 har endast behörighet toosee två kolumner.</span><span class="sxs-lookup"><span data-stu-id="a195a-207">This will fail because hiveuser2 only has permission toosee two columns.</span></span> <span data-ttu-id="a195a-208">Du bör få hello följande fel:</span><span class="sxs-lookup"><span data-stu-id="a195a-208">You shall get hello following error:</span></span>

        [Microsoft][HiveODBC] (35) Error from Hive: error code: '40000' error message: 'Error while compiling statement: FAILED: HiveAccessControlException Permission denied: user [hiveuser2] does not have [SELECT] privilege on [default/hivesampletable/clientid,country ...]'.
3. <span data-ttu-id="a195a-209">Följ hello samma procedur tooimport data.</span><span class="sxs-lookup"><span data-stu-id="a195a-209">Follow hello same procedure tooimport data.</span></span> <span data-ttu-id="a195a-210">Den här tiden kan använda hiveuser2's autentiseringsuppgifter och även modifiera hello select-instruktion från:</span><span class="sxs-lookup"><span data-stu-id="a195a-210">This time, use hiveuser2's credentials, and also modify hello select statement from:</span></span>

        SELECT * FROM "HIVE"."default"."hivesampletable"

    <span data-ttu-id="a195a-211">till:</span><span class="sxs-lookup"><span data-stu-id="a195a-211">to:</span></span>

        SELECT clientid, devicemake FROM "HIVE"."default"."hivesampletable"

    <span data-ttu-id="a195a-212">När det är klart bör du se två kolumner med importerade data.</span><span class="sxs-lookup"><span data-stu-id="a195a-212">When it is done, you shall see two columns of data imported.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a195a-213">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a195a-213">Next steps</span></span>
* <span data-ttu-id="a195a-214">Om du vill konfigurera ett domänanslutet HDInsight-kluster kan du läsa i [Konfigurera domänanslutna HDInsight-kluster](hdinsight-domain-joined-configure.md).</span><span class="sxs-lookup"><span data-stu-id="a195a-214">For configuring a Domain-joined HDInsight cluster, see [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md).</span></span>
* <span data-ttu-id="a195a-215">Om du vill hantera ett domänanslutet HDInsight-kluster kan du läsa i [Hantera domänanslutna HDInsight-kluster](hdinsight-domain-joined-manage.md).</span><span class="sxs-lookup"><span data-stu-id="a195a-215">For managing a Domain-joined HDInsight clusters, see [Manage Domain-joined HDInsight clusters](hdinsight-domain-joined-manage.md).</span></span>
* <span data-ttu-id="a195a-216">För att köra Hive-frågor med SSH på domänanslutna HDInsight-kluster, se [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).</span><span class="sxs-lookup"><span data-stu-id="a195a-216">For running Hive queries using SSH on Domain-joined HDInsight clusters, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).</span></span>
* <span data-ttu-id="a195a-217">Ansluta Hive med Hive JDBC, finns [ansluta tooHive på Azure HDInsight med hello Hive JDBC-drivrutinen](hdinsight-connect-hive-jdbc-driver.md)</span><span class="sxs-lookup"><span data-stu-id="a195a-217">For Connecting Hive using Hive JDBC, see [Connect tooHive on Azure HDInsight using hello Hive JDBC driver](hdinsight-connect-hive-jdbc-driver.md)</span></span>
* <span data-ttu-id="a195a-218">Ansluta Excel tooHadoop med hjälp av Hive ODBC finns [ansluta Excel tooHadoop med hello Microsoft Hive ODBC-enhet](hdinsight-connect-excel-hive-odbc-driver.md)</span><span class="sxs-lookup"><span data-stu-id="a195a-218">For connecting Excel tooHadoop using Hive ODBC, see [Connect Excel tooHadoop with hello Microsoft Hive ODBC drive](hdinsight-connect-excel-hive-odbc-driver.md)</span></span>
* <span data-ttu-id="a195a-219">Ansluta Excel tooHadoop med Power Query finns [ansluta Excel tooHadoop med Power Query](hdinsight-connect-excel-power-query.md)</span><span class="sxs-lookup"><span data-stu-id="a195a-219">For connecting Excel tooHadoop using Power Query, see [Connect Excel tooHadoop by using Power Query](hdinsight-connect-excel-power-query.md)</span></span>
