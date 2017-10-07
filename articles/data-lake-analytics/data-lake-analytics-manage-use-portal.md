---
title: "aaaManage Azure Data Lake Analytics med hjälp av hello Azure-portalen | Microsoft Docs"
description: "Lär dig hur toomanage Data Lake Analytics räkenskaper, data datakällor, användare, och jobb."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: a0e045f1-73d6-427f-868d-7b55c10f811b
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.openlocfilehash: f63ccdfae79772c92e92462194e8cdc636a73dc6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-data-lake-analytics-by-using-hello-azure-portal"></a><span data-ttu-id="c1c70-103">Hantera Azure Data Lake Analytics med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c1c70-103">Manage Azure Data Lake Analytics by using hello Azure portal</span></span>
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

<span data-ttu-id="c1c70-104">Lär dig hur toomanage Azure Data Lake Analytics-konton, konto datakällor, användare och jobb med hjälp av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c1c70-104">Learn how toomanage Azure Data Lake Analytics accounts, account data sources, users, and jobs by using hello Azure portal.</span></span> <span data-ttu-id="c1c70-105">avsnitt om toosee om hur du använder andra verktyg, klicka på fliken hello överst på hello sidan.</span><span class="sxs-lookup"><span data-stu-id="c1c70-105">toosee management topics about using other tools, click a tab at hello top of hello page.</span></span>

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-data-lake-analytics-accounts"></a><span data-ttu-id="c1c70-106">Hantera Data Lake Analytics-konton</span><span class="sxs-lookup"><span data-stu-id="c1c70-106">Manage Data Lake Analytics accounts</span></span>

### <a name="create-an-account"></a><span data-ttu-id="c1c70-107">Skapa ett konto</span><span class="sxs-lookup"><span data-stu-id="c1c70-107">Create an account</span></span>

1. <span data-ttu-id="c1c70-108">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c1c70-108">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="c1c70-109">Klicka på **Nytt** > **Intelligence + analytics** > **Data Lake Analysis**.</span><span class="sxs-lookup"><span data-stu-id="c1c70-109">Click **New** > **Intelligence + analytics** > **Data Lake Analytics**.</span></span>
3. <span data-ttu-id="c1c70-110">Välj värden för hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="c1c70-110">Select values for hello following items:</span></span> 
   1. <span data-ttu-id="c1c70-111">**Namnet**: hello namnet på hello Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="c1c70-111">**Name**: hello name of hello Data Lake Analytics account.</span></span>
   2. <span data-ttu-id="c1c70-112">**Prenumerationen**: hello Azure-prenumeration som används för hello-kontot.</span><span class="sxs-lookup"><span data-stu-id="c1c70-112">**Subscription**: hello Azure subscription used for hello account.</span></span>
   3. <span data-ttu-id="c1c70-113">**Resursgruppen**: hello Azure-resursgrupp i vilket toocreate hello-konto.</span><span class="sxs-lookup"><span data-stu-id="c1c70-113">**Resource Group**: hello Azure resource group in which toocreate hello account.</span></span> 
   4. <span data-ttu-id="c1c70-114">**Plats**: hello Azure-datacenter för hello Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="c1c70-114">**Location**: hello Azure datacenter for hello Data Lake Analytics account.</span></span> 
   5. <span data-ttu-id="c1c70-115">**Data Lake Store**: hello standard store toobe används för hello Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="c1c70-115">**Data Lake Store**: hello default store toobe used for hello Data Lake Analytics account.</span></span> <span data-ttu-id="c1c70-116">hello Azure Data Lake Store-konto och hello Data Lake Analytics-kontot måste vara i hello samma plats.</span><span class="sxs-lookup"><span data-stu-id="c1c70-116">hello Azure Data Lake Store account and hello Data Lake Analytics account must be in hello same location.</span></span>
4. <span data-ttu-id="c1c70-117">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="c1c70-117">Click **Create**.</span></span> 

### <a name="delete-a-data-lake-analytics-account"></a><span data-ttu-id="c1c70-118">Ta bort ett Data Lake Analytics-konto</span><span class="sxs-lookup"><span data-stu-id="c1c70-118">Delete a Data Lake Analytics account</span></span>

<span data-ttu-id="c1c70-119">Innan du tar bort ett Data Lake Analytics-konto kan du ta bort dess Data Lake Store-standardkontot.</span><span class="sxs-lookup"><span data-stu-id="c1c70-119">Before you delete a Data Lake Analytics account, delete its default Data Lake Store account.</span></span>

1. <span data-ttu-id="c1c70-120">Gå tooyour Data Lake Analytics-konto i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c1c70-120">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>
2. <span data-ttu-id="c1c70-121">Klicka på **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="c1c70-121">Click **Delete**.</span></span>
3. <span data-ttu-id="c1c70-122">Hello konto typnamn.</span><span class="sxs-lookup"><span data-stu-id="c1c70-122">Type hello account name.</span></span>
4. <span data-ttu-id="c1c70-123">Klicka på **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="c1c70-123">Click **Delete**.</span></span>

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-data-sources"></a><span data-ttu-id="c1c70-124">Hantera datakällor</span><span class="sxs-lookup"><span data-stu-id="c1c70-124">Manage data sources</span></span>

<span data-ttu-id="c1c70-125">Data Lake Analytics stöder hello följande datakällor:</span><span class="sxs-lookup"><span data-stu-id="c1c70-125">Data Lake Analytics supports hello following data sources:</span></span>

* <span data-ttu-id="c1c70-126">Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="c1c70-126">Data Lake Store</span></span>
* <span data-ttu-id="c1c70-127">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="c1c70-127">Azure Storage</span></span>

<span data-ttu-id="c1c70-128">Du kan använda Data Explorer toobrowse datakällor och utföra grundläggande filhanteringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="c1c70-128">You can use Data Explorer toobrowse data sources and perform basic file management operations.</span></span> 

### <a name="add-a-data-source"></a><span data-ttu-id="c1c70-129">Lägga till en datakälla</span><span class="sxs-lookup"><span data-stu-id="c1c70-129">Add a data source</span></span>

1. <span data-ttu-id="c1c70-130">Gå tooyour Data Lake Analytics-konto i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c1c70-130">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>
2. <span data-ttu-id="c1c70-131">Klicka på **datakällor**.</span><span class="sxs-lookup"><span data-stu-id="c1c70-131">Click **Data Sources**.</span></span>
3. <span data-ttu-id="c1c70-132">Klicka på **lägga till en datakälla**.</span><span class="sxs-lookup"><span data-stu-id="c1c70-132">Click **Add Data Source**.</span></span>
    
   * <span data-ttu-id="c1c70-133">tooadd ett Data Lake Store-konto behöver du hello-konto och toohello konto toobe kan tooquery den.</span><span class="sxs-lookup"><span data-stu-id="c1c70-133">tooadd a Data Lake Store account, you need hello account name and access toohello account toobe able tooquery it.</span></span>
   * <span data-ttu-id="c1c70-134">tooadd Azure Blob storage måste hello storage-konto och hello kontonyckel.</span><span class="sxs-lookup"><span data-stu-id="c1c70-134">tooadd Azure Blob storage, you need hello storage account and hello account key.</span></span> <span data-ttu-id="c1c70-135">toofind dem, gå toohello storage-konto i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="c1c70-135">toofind them, go toohello storage account in hello portal.</span></span>

## <a name="set-up-firewall-rules"></a><span data-ttu-id="c1c70-136">Konfigurera brandväggsregler</span><span class="sxs-lookup"><span data-stu-id="c1c70-136">Set up firewall rules</span></span>

<span data-ttu-id="c1c70-137">Du kan använda Data Lake Analytics toofurther Lås åtkomst tooyour Data Lake Analytics-konto på hello nätverksnivån.</span><span class="sxs-lookup"><span data-stu-id="c1c70-137">You can use Data Lake Analytics toofurther lock down access tooyour Data Lake Analytics account at hello network level.</span></span> <span data-ttu-id="c1c70-138">Du kan aktivera en brandvägg, ange en IP-adress eller definiera ett intervall med IP-adresser för dina betrodda klienter.</span><span class="sxs-lookup"><span data-stu-id="c1c70-138">You can enable a firewall, specify an IP address, or define an IP address range for your trusted clients.</span></span> <span data-ttu-id="c1c70-139">När du har aktiverat dessa åtgärder kan endast klienter som har hello IP-adresser inom intervallet hello definierats ansluta toohello store.</span><span class="sxs-lookup"><span data-stu-id="c1c70-139">After you enable these measures, only clients that have hello IP addresses within hello defined range can connect toohello store.</span></span>

<span data-ttu-id="c1c70-140">Om andra Azure-tjänster, t.ex. Azure Data Factory eller virtuella datorer kan ansluta till toohello Data Lake Analytics-konto, kontrollerar du att **Tillåt Azure Services** är aktiverat **på**.</span><span class="sxs-lookup"><span data-stu-id="c1c70-140">If other Azure services, like Azure Data Factory or VMs, connect toohello Data Lake Analytics account, make sure that **Allow Azure Services** is turned **On**.</span></span> 

### <a name="set-up-a-firewall-rule"></a><span data-ttu-id="c1c70-141">Konfigurera en brandväggsregel</span><span class="sxs-lookup"><span data-stu-id="c1c70-141">Set up a firewall rule</span></span>

1. <span data-ttu-id="c1c70-142">Gå tooyour Data Lake Analytics-konto i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c1c70-142">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>
2. <span data-ttu-id="c1c70-143">På menyn hello hello vänster **brandväggen**.</span><span class="sxs-lookup"><span data-stu-id="c1c70-143">On hello menu on hello left, click **Firewall**.</span></span>

## <a name="add-a-new-user"></a><span data-ttu-id="c1c70-144">Lägg till en ny användare</span><span class="sxs-lookup"><span data-stu-id="c1c70-144">Add a new user</span></span>

<span data-ttu-id="c1c70-145">Du kan använda hello **guiden Lägg till användare** tooeasily etablera nya Data Lake-användare.</span><span class="sxs-lookup"><span data-stu-id="c1c70-145">You can use hello **Add User Wizard** tooeasily provision new Data Lake users.</span></span>

1. <span data-ttu-id="c1c70-146">Gå tooyour Data Lake Analytics-konto i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c1c70-146">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>
2. <span data-ttu-id="c1c70-147">På vänster under hello **komma igång**, klickar du på **guiden Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="c1c70-147">On hello left, under **Getting Started**, click **Add User Wizard**.</span></span>
3. <span data-ttu-id="c1c70-148">Välj en användare och klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="c1c70-148">Select a user, and then click **Select**.</span></span>
4. <span data-ttu-id="c1c70-149">Välj en roll och klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="c1c70-149">Select a role, and then click **Select**.</span></span> <span data-ttu-id="c1c70-150">tooset upp en ny developer toouse Azure Data Lake, Välj hello **Data Lake Analytics Developer** roll.</span><span class="sxs-lookup"><span data-stu-id="c1c70-150">tooset up a new developer toouse Azure Data Lake, select hello **Data Lake Analytics Developer** role.</span></span>
5. <span data-ttu-id="c1c70-151">Välj hello åtkomstkontrollistor (ACL) för hello U-SQL-databaser.</span><span class="sxs-lookup"><span data-stu-id="c1c70-151">Select hello access control lists (ACLs) for hello U-SQL databases.</span></span> <span data-ttu-id="c1c70-152">När du är nöjd med dina val klickar du på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="c1c70-152">When you're satisfied with your choices, click **Select**.</span></span>
6. <span data-ttu-id="c1c70-153">Välj hello ACL: er för filer.</span><span class="sxs-lookup"><span data-stu-id="c1c70-153">Select hello ACLs for files.</span></span> <span data-ttu-id="c1c70-154">Hello standardlagringsplatsen inte ändrar hello ACL: er för rotmappen för hello ”/” och för hello/system mapp.</span><span class="sxs-lookup"><span data-stu-id="c1c70-154">For hello default store, don't change hello ACLs for hello root folder "/" and for hello /system folder.</span></span> <span data-ttu-id="c1c70-155">Klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="c1c70-155">Click **Select**.</span></span>
7. <span data-ttu-id="c1c70-156">Granska alla valda ändringarna och klicka sedan på **kör**.</span><span class="sxs-lookup"><span data-stu-id="c1c70-156">Review all your selected changes, and then click **Run**.</span></span>
8. <span data-ttu-id="c1c70-157">När hello guiden är klar klickar du på **klar**.</span><span class="sxs-lookup"><span data-stu-id="c1c70-157">When hello wizard is finished, click **Done**.</span></span>

## <a name="manage-role-based-access-control"></a><span data-ttu-id="c1c70-158">Hantera rollbaserad åtkomstkontroll</span><span class="sxs-lookup"><span data-stu-id="c1c70-158">Manage Role-Based Access Control</span></span>

<span data-ttu-id="c1c70-159">Du kan använda rollbaserad åtkomstkontroll (RBAC) toocontrol hur användarna samverkar med hello-tjänsten som andra Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="c1c70-159">Like other Azure services, you can use Role-Based Access Control (RBAC) toocontrol how users interact with hello service.</span></span>

<span data-ttu-id="c1c70-160">hello standard RBAC-roller har hello följande funktioner:</span><span class="sxs-lookup"><span data-stu-id="c1c70-160">hello standard RBAC roles have hello following capabilities:</span></span>
* <span data-ttu-id="c1c70-161">**Ägare**: kan skicka jobb, övervaka jobb, avbryta jobb från alla användare och konfigurera hello-konto.</span><span class="sxs-lookup"><span data-stu-id="c1c70-161">**Owner**: Can submit jobs, monitor jobs, cancel jobs from any user, and configure hello account.</span></span>
* <span data-ttu-id="c1c70-162">**Deltagare**: kan skicka jobb, övervaka jobb, avbryta jobb från alla användare och konfigurera hello-konto.</span><span class="sxs-lookup"><span data-stu-id="c1c70-162">**Contributor**: Can submit jobs, monitor jobs, cancel jobs from any user, and configure hello account.</span></span>
* <span data-ttu-id="c1c70-163">**Läsaren**: övervaka jobb.</span><span class="sxs-lookup"><span data-stu-id="c1c70-163">**Reader**: Can monitor jobs.</span></span>

<span data-ttu-id="c1c70-164">Använda hello Data Lake Analytics Developer tooenable U-SQL-utvecklare toouse hello Data Lake Analytics rolltjänsten.</span><span class="sxs-lookup"><span data-stu-id="c1c70-164">Use hello Data Lake Analytics Developer role tooenable U-SQL developers toouse hello Data Lake Analytics service.</span></span> <span data-ttu-id="c1c70-165">Du kan använda hello Data Lake Analytics Developer roll:</span><span class="sxs-lookup"><span data-stu-id="c1c70-165">You can use hello Data Lake Analytics Developer role to:</span></span>
* <span data-ttu-id="c1c70-166">Skicka jobb.</span><span class="sxs-lookup"><span data-stu-id="c1c70-166">Submit jobs.</span></span>
* <span data-ttu-id="c1c70-167">Övervaka jobb status och hello jobb som skickats av någon användare.</span><span class="sxs-lookup"><span data-stu-id="c1c70-167">Monitor job status and hello progress of jobs submitted by any user.</span></span>
* <span data-ttu-id="c1c70-168">Se hello U-SQL-skript från jobb som skickats av någon användare.</span><span class="sxs-lookup"><span data-stu-id="c1c70-168">See hello U-SQL scripts from jobs submitted by any user.</span></span>
* <span data-ttu-id="c1c70-169">Avbryt endast egna jobb.</span><span class="sxs-lookup"><span data-stu-id="c1c70-169">Cancel only your own jobs.</span></span>

### <a name="add-users-or-security-groups-tooa-data-lake-analytics-account"></a><span data-ttu-id="c1c70-170">Lägga till användare eller säkerhet grupper tooa Data Lake Analytics-konto</span><span class="sxs-lookup"><span data-stu-id="c1c70-170">Add users or security groups tooa Data Lake Analytics account</span></span>

1. <span data-ttu-id="c1c70-171">Gå tooyour Data Lake Analytics-konto i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c1c70-171">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>
2. <span data-ttu-id="c1c70-172">Klicka på **åtkomstkontroll (IAM)** > **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="c1c70-172">Click **Access control (IAM)** > **Add**.</span></span>
3. <span data-ttu-id="c1c70-173">Välj en roll.</span><span class="sxs-lookup"><span data-stu-id="c1c70-173">Select a role.</span></span>
4. <span data-ttu-id="c1c70-174">Lägga till en användare.</span><span class="sxs-lookup"><span data-stu-id="c1c70-174">Add a user.</span></span>
5. <span data-ttu-id="c1c70-175">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c1c70-175">Click **OK**.</span></span>

>[!NOTE]
><span data-ttu-id="c1c70-176">Om en användare eller en säkerhetsgrupp måste toosubmit jobb, måste de också behörighet till hello store-konto.</span><span class="sxs-lookup"><span data-stu-id="c1c70-176">If a user or a security group needs toosubmit jobs, they also need permission on hello store account.</span></span> <span data-ttu-id="c1c70-177">Mer information finns i [skyddar data som lagras i Data Lake Store](../data-lake-store/data-lake-store-secure-data.md).</span><span class="sxs-lookup"><span data-stu-id="c1c70-177">For more information, see [Secure data stored in Data Lake Store](../data-lake-store/data-lake-store-secure-data.md).</span></span>
>

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-jobs"></a><span data-ttu-id="c1c70-178">Hantera jobb</span><span class="sxs-lookup"><span data-stu-id="c1c70-178">Manage jobs</span></span>

### <a name="submit-a-job"></a><span data-ttu-id="c1c70-179">Skicka ett jobb</span><span class="sxs-lookup"><span data-stu-id="c1c70-179">Submit a job</span></span>

1. <span data-ttu-id="c1c70-180">Gå tooyour Data Lake Analytics-konto i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c1c70-180">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>

2. <span data-ttu-id="c1c70-181">Klicka på **nytt jobb**.</span><span class="sxs-lookup"><span data-stu-id="c1c70-181">Click **New Job**.</span></span> <span data-ttu-id="c1c70-182">Konfigurera följande för varje jobb:</span><span class="sxs-lookup"><span data-stu-id="c1c70-182">For each job,  configure:</span></span>

    1. <span data-ttu-id="c1c70-183">**Jobbnamnet**: hello namnet på hello jobb.</span><span class="sxs-lookup"><span data-stu-id="c1c70-183">**Job Name**: hello name of hello job.</span></span>
    2. <span data-ttu-id="c1c70-184">**Prioritet**: lägre nummer har högre prioritet.</span><span class="sxs-lookup"><span data-stu-id="c1c70-184">**Priority**: Lower numbers have higher priority.</span></span> <span data-ttu-id="c1c70-185">Om två jobb i kö, körs hello ett lägre värde för prioritet först.</span><span class="sxs-lookup"><span data-stu-id="c1c70-185">If two jobs are queued, hello one with lower priority value runs first.</span></span>
    3. <span data-ttu-id="c1c70-186">**Parallellitet**: hello maxantalet beräkning bearbetar tooreserve för det här jobbet.</span><span class="sxs-lookup"><span data-stu-id="c1c70-186">**Parallelism**: hello maximum number of compute processes tooreserve for this job.</span></span>

3. <span data-ttu-id="c1c70-187">Klicka på **Skicka jobb**.</span><span class="sxs-lookup"><span data-stu-id="c1c70-187">Click **Submit Job**.</span></span>

### <a name="monitor-jobs"></a><span data-ttu-id="c1c70-188">Övervaka jobb</span><span class="sxs-lookup"><span data-stu-id="c1c70-188">Monitor jobs</span></span>

1. <span data-ttu-id="c1c70-189">Gå tooyour Data Lake Analytics-konto i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c1c70-189">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>
2. <span data-ttu-id="c1c70-190">Klicka på **visa alla jobb**.</span><span class="sxs-lookup"><span data-stu-id="c1c70-190">Click **View All Jobs**.</span></span> <span data-ttu-id="c1c70-191">En lista över alla hello-aktiva och nyligen klar jobb i hello-konto visas.</span><span class="sxs-lookup"><span data-stu-id="c1c70-191">A list of all hello active and recently finished jobs in hello account is shown.</span></span>
3. <span data-ttu-id="c1c70-192">Du kan också klicka på **Filter** toohelp som du hittar hello jobb efter **tidsintervall**, **jobbnamn**, och **författare** värden.</span><span class="sxs-lookup"><span data-stu-id="c1c70-192">Optionally, click **Filter** toohelp you find hello jobs by **Time Range**, **Job Name**, and **Author** values.</span></span> 

### <a name="monitoring-pipeline-jobs"></a><span data-ttu-id="c1c70-193">Övervaka pipeline-jobb</span><span class="sxs-lookup"><span data-stu-id="c1c70-193">Monitoring pipeline jobs</span></span>
<span data-ttu-id="c1c70-194">Jobb som är en del av en pipeline fungerar tillsammans, vanligtvis sekventiellt tooaccomplish ett specifikt scenario.</span><span class="sxs-lookup"><span data-stu-id="c1c70-194">Jobs that are part of a pipeline work together, usually sequentially, tooaccomplish a specific scenario.</span></span> <span data-ttu-id="c1c70-195">Du kan till exempel ha en pipeline som rensar, extraherar, omvandlar, aggregerar användning för kunden insikter.</span><span class="sxs-lookup"><span data-stu-id="c1c70-195">For example, you can have a pipeline that cleans, extracts, transforms, aggregates usage for customer insights.</span></span> <span data-ttu-id="c1c70-196">Pipeline-jobb identifieras med hello ”Pipeline” egenskapen när hello jobbet har skickats.</span><span class="sxs-lookup"><span data-stu-id="c1c70-196">Pipeline jobs are identified using hello "Pipeline" property when hello job was submitted.</span></span> <span data-ttu-id="c1c70-197">Jobb som schemalagts med ADF V2 har automatiskt den här egenskapen fylls i.</span><span class="sxs-lookup"><span data-stu-id="c1c70-197">Jobs scheduled using ADF V2 will automatically have this property populated.</span></span> 

<span data-ttu-id="c1c70-198">tooview en lista över U-SQL-jobb som är del av pipelines:</span><span class="sxs-lookup"><span data-stu-id="c1c70-198">tooview a list of U-SQL jobs that are part of pipelines:</span></span> 

1. <span data-ttu-id="c1c70-199">Gå tooyour Data Lake Analytics-konton i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c1c70-199">In hello Azure portal, go tooyour Data Lake Analytics accounts.</span></span>
2. <span data-ttu-id="c1c70-200">Klicka på **jobbet insikter**.</span><span class="sxs-lookup"><span data-stu-id="c1c70-200">Click **Job Insights**.</span></span> <span data-ttu-id="c1c70-201">Hej ”alla” jobbfliken kommer att återställas, visar en lista över körs, i kö och avslutats jobb.</span><span class="sxs-lookup"><span data-stu-id="c1c70-201">hello "All Jobs" tab will be defaulted, showing a list of running, queued, and ended jobs.</span></span>
3. <span data-ttu-id="c1c70-202">Klicka på hello **Pipeline jobb** fliken. En lista över pipeline jobb visas tillsammans med statistik för varje pipelinen.</span><span class="sxs-lookup"><span data-stu-id="c1c70-202">Click hello **Pipeline Jobs** tab. A list of pipeline jobs will be shown along with aggregated statistics for each pipeline.</span></span>

### <a name="monitoring-recurring-jobs"></a><span data-ttu-id="c1c70-203">Övervaka återkommande jobb</span><span class="sxs-lookup"><span data-stu-id="c1c70-203">Monitoring recurring jobs</span></span>
<span data-ttu-id="c1c70-204">Ett återkommande jobb är en hello samma affärslogik men använder olika indata varje gång det körs.</span><span class="sxs-lookup"><span data-stu-id="c1c70-204">A recurring job is one that has hello same business logic but uses different input data every time it runs.</span></span> <span data-ttu-id="c1c70-205">Vi rekommenderar bör återkommande jobb alltid lyckas och har stabilt körningstid; övervakning av dessa beteenden säkerställer hello jobbet är felfri.</span><span class="sxs-lookup"><span data-stu-id="c1c70-205">Ideally, recurring jobs should always succeed, and have relatively stable execution time; monitoring these behaviors will help ensure hello job is healthy.</span></span> <span data-ttu-id="c1c70-206">Återkommande jobb identifieras med hello ”återkommande”-egenskap.</span><span class="sxs-lookup"><span data-stu-id="c1c70-206">Recurring jobs are identified using hello "Recurrence" property.</span></span> <span data-ttu-id="c1c70-207">Jobb som schemalagts med ADF V2 har automatiskt den här egenskapen fylls i.</span><span class="sxs-lookup"><span data-stu-id="c1c70-207">Jobs scheduled using ADF V2 will automatically have this property populated.</span></span>

<span data-ttu-id="c1c70-208">tooview en lista över U-SQL-jobb som är återkommande:</span><span class="sxs-lookup"><span data-stu-id="c1c70-208">tooview a list of U-SQL jobs that are recurring:</span></span> 

1. <span data-ttu-id="c1c70-209">Gå tooyour Data Lake Analytics-konton i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c1c70-209">In hello Azure portal, go tooyour Data Lake Analytics accounts.</span></span>
2. <span data-ttu-id="c1c70-210">Klicka på **jobbet insikter**.</span><span class="sxs-lookup"><span data-stu-id="c1c70-210">Click **Job Insights**.</span></span> <span data-ttu-id="c1c70-211">Hej ”alla” jobbfliken kommer att återställas, visar en lista över körs, i kö och avslutats jobb.</span><span class="sxs-lookup"><span data-stu-id="c1c70-211">hello "All Jobs" tab will be defaulted, showing a list of running, queued, and ended jobs.</span></span>
3. <span data-ttu-id="c1c70-212">Klicka på hello **återkommande jobb** fliken. En lista över återkommande jobb visas tillsammans med statistik för alla återkommande jobb.</span><span class="sxs-lookup"><span data-stu-id="c1c70-212">Click hello **Recurring Jobs** tab. A list of recurring jobs will be shown along with aggregated statistics for each recurring job.</span></span>

## <a name="manage-policies"></a><span data-ttu-id="c1c70-213">Hantera principer</span><span class="sxs-lookup"><span data-stu-id="c1c70-213">Manage policies</span></span>

### <a name="account-level-policies"></a><span data-ttu-id="c1c70-214">Kontonivå principer</span><span class="sxs-lookup"><span data-stu-id="c1c70-214">Account-level policies</span></span>

<span data-ttu-id="c1c70-215">Dessa principer tillämpas tooall jobb i ett Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="c1c70-215">These policies apply tooall jobs in a Data Lake Analytics account.</span></span>

#### <a name="maximum-number-of-aus-in-a-data-lake-analytics-account"></a><span data-ttu-id="c1c70-216">Maximalt antal Australien i ett Data Lake Analytics-konto</span><span class="sxs-lookup"><span data-stu-id="c1c70-216">Maximum number of AUs in a Data Lake Analytics account</span></span>
<span data-ttu-id="c1c70-217">En princip styr hello totala antalet Analytics-enheter (Australien) kan använda ditt Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="c1c70-217">A policy controls hello total number of Analytics Units (AUs) your Data Lake Analytics account can use.</span></span> <span data-ttu-id="c1c70-218">Hello-värdet är som standard too250.</span><span class="sxs-lookup"><span data-stu-id="c1c70-218">By default, hello value is set too250.</span></span> <span data-ttu-id="c1c70-219">Till exempel om det här värdet anges too250 Australien, du kan ha ett jobb som körs med 250 Australien tilldelade tooit eller 10 jobb som körs med 25 Australien varje.</span><span class="sxs-lookup"><span data-stu-id="c1c70-219">For example, if this value is set too250 AUs, you can have one job running with 250 AUs assigned tooit, or 10 jobs running with 25 AUs each.</span></span> <span data-ttu-id="c1c70-220">Ytterligare jobb som skickats köas förrän hello jobb som körs är klar.</span><span class="sxs-lookup"><span data-stu-id="c1c70-220">Additional jobs that are submitted are queued until hello running jobs are finished.</span></span> <span data-ttu-id="c1c70-221">När jobb som körs är slutförda Australien är frigörs för hello köade jobb toorun.</span><span class="sxs-lookup"><span data-stu-id="c1c70-221">When running jobs are finished, AUs are freed up for hello queued jobs toorun.</span></span>

<span data-ttu-id="c1c70-222">toochange hello antal Australien för ditt Data Lake Analytics-konto:</span><span class="sxs-lookup"><span data-stu-id="c1c70-222">toochange hello number of AUs for your Data Lake Analytics account:</span></span>

1. <span data-ttu-id="c1c70-223">Gå tooyour Data Lake Analytics-konto i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c1c70-223">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>
2. <span data-ttu-id="c1c70-224">Klicka på **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="c1c70-224">Click **Properties**.</span></span>
3. <span data-ttu-id="c1c70-225">Under **maximala Australien**, flytta hello skjutreglaget tooselect ett värde eller ange hello värde i textrutan för hello.</span><span class="sxs-lookup"><span data-stu-id="c1c70-225">Under **Maximum AUs**, move hello slider tooselect a value, or enter hello value in hello text box.</span></span> 
4. <span data-ttu-id="c1c70-226">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="c1c70-226">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="c1c70-227">Om du behöver mer än hello standard (250) Australien, hello-portalen klickar du på **hjälp + Support** toosubmit en supportbegäran.</span><span class="sxs-lookup"><span data-stu-id="c1c70-227">If you need more than hello default (250) AUs, in hello portal, click **Help+Support** toosubmit a support request.</span></span> <span data-ttu-id="c1c70-228">hello antalet Australien som är tillgängliga i ditt Data Lake Analytics-konto kan du öka.</span><span class="sxs-lookup"><span data-stu-id="c1c70-228">hello number of AUs available in your Data Lake Analytics account can be increased.</span></span>
>

#### <a name="maximum-number-of-jobs-that-can-run-simultaneously"></a><span data-ttu-id="c1c70-229">Maximalt antal jobb som kan köras samtidigt</span><span class="sxs-lookup"><span data-stu-id="c1c70-229">Maximum number of jobs that can run simultaneously</span></span>
<span data-ttu-id="c1c70-230">En princip styr hur många jobb kan köras på hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="c1c70-230">A policy controls how many jobs can run at hello same time.</span></span> <span data-ttu-id="c1c70-231">Det här värdet är som standard too20.</span><span class="sxs-lookup"><span data-stu-id="c1c70-231">By default, this value is set too20.</span></span> <span data-ttu-id="c1c70-232">Om Data Lake Analytics har Australien som är tillgängliga, är nya jobb schemalagda toorun omedelbart tills hello Totalt antal jobb som körs hello värdet för den här principen.</span><span class="sxs-lookup"><span data-stu-id="c1c70-232">If your Data Lake Analytics has AUs available, new jobs are scheduled toorun immediately until hello total number of running jobs reaches hello value of this policy.</span></span> <span data-ttu-id="c1c70-233">När du når hello maximalt antal jobb som kan köras samtidigt efterföljande jobb ställs i kö i prioritetsordning tills en eller flera av de jobb som körs är klar (beroende på Australien tillgänglighet).</span><span class="sxs-lookup"><span data-stu-id="c1c70-233">When you reach hello maximum number of jobs that can run simultaneously, subsequent jobs are queued in priority order until one or more running jobs complete (depending on AU availability).</span></span>

<span data-ttu-id="c1c70-234">toochange hello antalet jobb som kan köras samtidigt:</span><span class="sxs-lookup"><span data-stu-id="c1c70-234">toochange hello number of jobs that can run simultaneously:</span></span>

1. <span data-ttu-id="c1c70-235">Gå tooyour Data Lake Analytics-konto i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c1c70-235">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>
2. <span data-ttu-id="c1c70-236">Klicka på **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="c1c70-236">Click **Properties**.</span></span>
3. <span data-ttu-id="c1c70-237">Under **maximala antalet av kör jobb**, flytta hello skjutreglaget tooselect ett värde eller ange hello värde i textrutan för hello.</span><span class="sxs-lookup"><span data-stu-id="c1c70-237">Under **Maximum Number of Running Jobs**, move hello slider tooselect a value, or enter hello value in hello text box.</span></span> 
4. <span data-ttu-id="c1c70-238">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="c1c70-238">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="c1c70-239">Om du behöver mer än hello standard (20) antal jobb i hello-portalen klickar du på toorun **hjälp + Support** toosubmit en supportbegäran.</span><span class="sxs-lookup"><span data-stu-id="c1c70-239">If you need toorun more than hello default (20) number of jobs, in hello portal, click **Help+Support** toosubmit a support request.</span></span> <span data-ttu-id="c1c70-240">hello antalet jobb som kan köras samtidigt i ditt Data Lake Analytics-konto kan du öka.</span><span class="sxs-lookup"><span data-stu-id="c1c70-240">hello number of jobs that can run simultaneously in your Data Lake Analytics account can be increased.</span></span>
>

#### <a name="how-long-tookeep-job-metadata-and-resources"></a><span data-ttu-id="c1c70-241">Hur länge tookeep jobbet metadata och resurser</span><span class="sxs-lookup"><span data-stu-id="c1c70-241">How long tookeep job metadata and resources</span></span> 
<span data-ttu-id="c1c70-242">När användarna kör U-SQL-jobb, behåller alla relaterade filer hello Data Lake Analytics-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="c1c70-242">When your users run U-SQL jobs, hello Data Lake Analytics service retains all related files.</span></span> <span data-ttu-id="c1c70-243">Relaterade filer inkluderar hello U-SQL-skript, hello DLL-filer som refereras i hello U-SQL-skript, kompilerade resurser och statistik.</span><span class="sxs-lookup"><span data-stu-id="c1c70-243">Related files include hello U-SQL script, hello DLL files referenced in hello U-SQL script, compiled resources, and statistics.</span></span> <span data-ttu-id="c1c70-244">hello-filer finns i hello /system/ mapp för hello standardkontot för lagring av Azure Data Lake.</span><span class="sxs-lookup"><span data-stu-id="c1c70-244">hello files are in hello /system/ folder of hello default Azure Data Lake Storage account.</span></span> <span data-ttu-id="c1c70-245">Den här principen styr hur länge dessa resurser lagras innan de tas bort automatiskt (hello standard är 30 dagar).</span><span class="sxs-lookup"><span data-stu-id="c1c70-245">This policy controls how long these resources are stored before they are automatically deleted (hello default is 30 days).</span></span> <span data-ttu-id="c1c70-246">Du kan använda dessa filer för felsökning och prestandajustering av jobb som du ska köra i hello framtida.</span><span class="sxs-lookup"><span data-stu-id="c1c70-246">You can use these files for debugging, and for performance-tuning of jobs that you'll rerun in hello future.</span></span>

<span data-ttu-id="c1c70-247">toochange hur länge tookeep jobbet metadata och resurser:</span><span class="sxs-lookup"><span data-stu-id="c1c70-247">toochange how long tookeep job metadata and resources:</span></span>

1. <span data-ttu-id="c1c70-248">Gå tooyour Data Lake Analytics-konto i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c1c70-248">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>
2. <span data-ttu-id="c1c70-249">Klicka på **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="c1c70-249">Click **Properties**.</span></span>
3. <span data-ttu-id="c1c70-250">Under **dagar tooRetain Jobbfrågor**, flytta hello skjutreglaget tooselect ett värde eller ange hello värde i textrutan för hello.</span><span class="sxs-lookup"><span data-stu-id="c1c70-250">Under **Days tooRetain Job Queries**, move hello slider tooselect a value, or enter hello value in hello text box.</span></span>  
4. <span data-ttu-id="c1c70-251">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="c1c70-251">Click **Save**.</span></span>

### <a name="job-level-policies"></a><span data-ttu-id="c1c70-252">Nivå principer</span><span class="sxs-lookup"><span data-stu-id="c1c70-252">Job-level policies</span></span>
<span data-ttu-id="c1c70-253">Med principer för jobbet på objektnivå kan du styra hello maximala Australien och hello maximal prioritet som enskilda användare (eller medlemmar i specifika säkerhetsgrupper) kan ställa in för jobb som de skickar.</span><span class="sxs-lookup"><span data-stu-id="c1c70-253">With job-level policies, you can control hello maximum AUs and hello maximum priority that individual users (or members of specific security groups) can set on jobs that they submit.</span></span> <span data-ttu-id="c1c70-254">Detta kan du styra hello kostnader av användare.</span><span class="sxs-lookup"><span data-stu-id="c1c70-254">This lets you control hello costs incurred by users.</span></span> <span data-ttu-id="c1c70-255">Du kan också hello effekt som schemalagda jobb kan ha på hög prioritet produktionsjobb som körs i hello samma Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="c1c70-255">It also lets you control hello effect that scheduled jobs might have on high-priority production jobs that are running in hello same Data Lake Analytics account.</span></span>

<span data-ttu-id="c1c70-256">Data Lake Analytics har två principer som du kan ange nivån för hello jobb:</span><span class="sxs-lookup"><span data-stu-id="c1c70-256">Data Lake Analytics has two policies that you can set at hello job level:</span></span>

* <span data-ttu-id="c1c70-257">**Australien gränsen per jobb**: användare kan bara skicka jobb som har ett toothis antal Australien.</span><span class="sxs-lookup"><span data-stu-id="c1c70-257">**AU limit per job**: Users can only submit jobs that have up toothis number of AUs.</span></span> <span data-ttu-id="c1c70-258">Som standard är den här gränsen hello samma som hello Australien maxgränsen för hello-konto.</span><span class="sxs-lookup"><span data-stu-id="c1c70-258">By default, this limit is hello same as hello maximum AU limit for hello account.</span></span>
* <span data-ttu-id="c1c70-259">**Prioritet**: användare kan bara skicka jobb som har ett värde för prioritet lägre än eller lika med toothis.</span><span class="sxs-lookup"><span data-stu-id="c1c70-259">**Priority**: Users can only submit jobs that have a priority lower than or equal toothis value.</span></span> <span data-ttu-id="c1c70-260">Observera att ett högre värde innebär en lägre prioritet.</span><span class="sxs-lookup"><span data-stu-id="c1c70-260">Note that a higher number means a lower priority.</span></span> <span data-ttu-id="c1c70-261">Detta är som standard too1, vilket är hello högsta möjliga prioritet.</span><span class="sxs-lookup"><span data-stu-id="c1c70-261">By default, this is set too1, which is hello highest possible priority.</span></span>

<span data-ttu-id="c1c70-262">Det finns en standardprincip som anges för varje konto.</span><span class="sxs-lookup"><span data-stu-id="c1c70-262">There is a default policy set on every account.</span></span> <span data-ttu-id="c1c70-263">hello standardprincipen gäller tooall användare av hello-konto.</span><span class="sxs-lookup"><span data-stu-id="c1c70-263">hello default policy applies tooall users of hello account.</span></span> <span data-ttu-id="c1c70-264">Du kan ange ytterligare principer för specifika användare och grupper.</span><span class="sxs-lookup"><span data-stu-id="c1c70-264">You can set additional policies for specific users and groups.</span></span> 

> [!NOTE]
> <span data-ttu-id="c1c70-265">Kontonivå och nivå principer tillämpas samtidigt.</span><span class="sxs-lookup"><span data-stu-id="c1c70-265">Account-level policies and job-level policies apply simultaneously.</span></span>
>

#### <a name="add-a-policy-for-a-specific-user-or-group"></a><span data-ttu-id="c1c70-266">Lägg till en princip för en specifik användare eller grupp</span><span class="sxs-lookup"><span data-stu-id="c1c70-266">Add a policy for a specific user or group</span></span>

1. <span data-ttu-id="c1c70-267">Gå tooyour Data Lake Analytics-konto i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c1c70-267">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>
2. <span data-ttu-id="c1c70-268">Klicka på **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="c1c70-268">Click **Properties**.</span></span>
3. <span data-ttu-id="c1c70-269">Under **jobbet skicka gränser**, klicka på hello **Lägg till princip** knappen.</span><span class="sxs-lookup"><span data-stu-id="c1c70-269">Under **Job Submission Limits**, click hello **Add Policy** button.</span></span> <span data-ttu-id="c1c70-270">Sedan, Välj eller ange hello följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="c1c70-270">Then, select or enter hello following settings:</span></span>
    1. <span data-ttu-id="c1c70-271">**Beräkna principnamn**: Ange ett principnamn, tooremind du hello syftet med hello principen.</span><span class="sxs-lookup"><span data-stu-id="c1c70-271">**Compute Policy Name**: Enter a policy name, tooremind you of hello purpose of hello policy.</span></span>
    2. <span data-ttu-id="c1c70-272">**Välj användare eller grupp**: Välj hello användare eller grupper som principen gäller.</span><span class="sxs-lookup"><span data-stu-id="c1c70-272">**Select User or Group**: Select hello user or group this policy applies to.</span></span>
    3. <span data-ttu-id="c1c70-273">**Ange hello jobbet Australien gränsen**: hello Australien gränsen som gäller toohello valda användare eller grupp.</span><span class="sxs-lookup"><span data-stu-id="c1c70-273">**Set hello Job AU Limit**: Set hello AU limit that applies toohello selected user or group.</span></span>
    4. <span data-ttu-id="c1c70-274">**Ange hello prioritet gränsen**: hello prioritet gränsen som gäller toohello valda användare eller grupp.</span><span class="sxs-lookup"><span data-stu-id="c1c70-274">**Set hello Priority Limit**: Set hello priority limit that applies toohello selected user or group.</span></span>

4. <span data-ttu-id="c1c70-275">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c1c70-275">Click **Ok**.</span></span>

5. <span data-ttu-id="c1c70-276">hello nya principen visas i hello **standard** princip tabell under **jobbet skicka gränser**.</span><span class="sxs-lookup"><span data-stu-id="c1c70-276">hello new policy is listed in hello **Default** policy table, under **Job Submission Limits**.</span></span> 

#### <a name="delete-or-edit-an-existing-policy"></a><span data-ttu-id="c1c70-277">Ta bort eller redigera en befintlig princip</span><span class="sxs-lookup"><span data-stu-id="c1c70-277">Delete or edit an existing policy</span></span>

1. <span data-ttu-id="c1c70-278">Gå tooyour Data Lake Analytics-konto i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c1c70-278">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>
2. <span data-ttu-id="c1c70-279">Klicka på **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="c1c70-279">Click **Properties**.</span></span>
3. <span data-ttu-id="c1c70-280">Under **jobbet skicka gränser**, hitta hello princip som du vill tooedit.</span><span class="sxs-lookup"><span data-stu-id="c1c70-280">Under **Job Submission Limits**, find hello policy you want tooedit.</span></span>
4.  <span data-ttu-id="c1c70-281">toosee hello **ta bort** och **redigera** i hello längst till höger kolumn hello tabell, klicka på **...** .</span><span class="sxs-lookup"><span data-stu-id="c1c70-281">toosee hello **Delete** and **Edit** options, in hello rightmost column of hello table, click **...**.</span></span>

### <a name="additional-resources-for-job-policies"></a><span data-ttu-id="c1c70-282">Ytterligare resurser för jobbet principer</span><span class="sxs-lookup"><span data-stu-id="c1c70-282">Additional resources for job policies</span></span>
* [<span data-ttu-id="c1c70-283">Blogginlägget för principen: översikt</span><span class="sxs-lookup"><span data-stu-id="c1c70-283">Policy overview blog post</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-overview/)
* [<span data-ttu-id="c1c70-284">Principer för kontonivå blogginlägget</span><span class="sxs-lookup"><span data-stu-id="c1c70-284">Account-level policies blog post</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-account-level-policy/)
* [<span data-ttu-id="c1c70-285">Principer jobb på blogginlägget</span><span class="sxs-lookup"><span data-stu-id="c1c70-285">Job-level policies blog post</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-job-level-policy/)

## <a name="next-steps"></a><span data-ttu-id="c1c70-286">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c1c70-286">Next steps</span></span>

* [<span data-ttu-id="c1c70-287">Översikt över Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="c1c70-287">Overview of Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="c1c70-288">Kom igång med Data Lake Analytics med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c1c70-288">Get started with Data Lake Analytics by using hello Azure portal</span></span>](data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="c1c70-289">Hantera Azure Data Lake Analytics med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="c1c70-289">Manage Azure Data Lake Analytics by using Azure PowerShell</span></span>](data-lake-analytics-manage-use-powershell.md)

