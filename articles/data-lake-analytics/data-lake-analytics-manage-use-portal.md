---
title: "Hantera Azure Data Lake Analytics med hjälp av Azure portal | Microsoft Docs"
description: "Lär dig hur du hanterar Data Lake Analytics räkenskaper, datakällor, användare och jobb."
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
ms.openlocfilehash: e49d1a0e0ccc6567d0a6841817667717ff5dba76
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="manage-azure-data-lake-analytics-by-using-the-azure-portal"></a><span data-ttu-id="1b198-103">Hantera Azure Data Lake Analytics med hjälp av Azure portal</span><span class="sxs-lookup"><span data-stu-id="1b198-103">Manage Azure Data Lake Analytics by using the Azure portal</span></span>
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

<span data-ttu-id="1b198-104">Lär dig mer om att hantera Azure Data Lake Analytics-konton, konto datakällor, användare och jobb med hjälp av Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1b198-104">Learn how to manage Azure Data Lake Analytics accounts, account data sources, users, and jobs by using the Azure portal.</span></span> <span data-ttu-id="1b198-105">Om du vill se avsnitt om om hur du använder andra verktyg klickar du på en flik överst på sidan.</span><span class="sxs-lookup"><span data-stu-id="1b198-105">To see management topics about using other tools, click a tab at the top of the page.</span></span>

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-data-lake-analytics-accounts"></a><span data-ttu-id="1b198-106">Hantera Data Lake Analytics-konton</span><span class="sxs-lookup"><span data-stu-id="1b198-106">Manage Data Lake Analytics accounts</span></span>

### <a name="create-an-account"></a><span data-ttu-id="1b198-107">Skapa ett konto</span><span class="sxs-lookup"><span data-stu-id="1b198-107">Create an account</span></span>

1. <span data-ttu-id="1b198-108">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1b198-108">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="1b198-109">Klicka på **Nytt** > **Intelligence + analytics** > **Data Lake Analysis**.</span><span class="sxs-lookup"><span data-stu-id="1b198-109">Click **New** > **Intelligence + analytics** > **Data Lake Analytics**.</span></span>
3. <span data-ttu-id="1b198-110">Välj värden för följande objekt:</span><span class="sxs-lookup"><span data-stu-id="1b198-110">Select values for the following items:</span></span> 
   1. <span data-ttu-id="1b198-111">**Namnet**: namnet på Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="1b198-111">**Name**: The name of the Data Lake Analytics account.</span></span>
   2. <span data-ttu-id="1b198-112">**Prenumerationen**: Azure-prenumeration som används för kontot.</span><span class="sxs-lookup"><span data-stu-id="1b198-112">**Subscription**: The Azure subscription used for the account.</span></span>
   3. <span data-ttu-id="1b198-113">**Resursgruppen**: Azure-resursgrupp att skapa kontot.</span><span class="sxs-lookup"><span data-stu-id="1b198-113">**Resource Group**: The Azure resource group in which to create the account.</span></span> 
   4. <span data-ttu-id="1b198-114">**Plats**: Azure-datacenter för Data Lake Analytics-kontot.</span><span class="sxs-lookup"><span data-stu-id="1b198-114">**Location**: The Azure datacenter for the Data Lake Analytics account.</span></span> 
   5. <span data-ttu-id="1b198-115">**Data Lake Store**: standardlagringsplatsen som ska användas för Data Lake Analytics-kontot.</span><span class="sxs-lookup"><span data-stu-id="1b198-115">**Data Lake Store**: The default store to be used for the Data Lake Analytics account.</span></span> <span data-ttu-id="1b198-116">Azure Data Lake Store-konto och Data Lake Analytics-kontot måste vara på samma plats.</span><span class="sxs-lookup"><span data-stu-id="1b198-116">The Azure Data Lake Store account and the Data Lake Analytics account must be in the same location.</span></span>
4. <span data-ttu-id="1b198-117">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="1b198-117">Click **Create**.</span></span> 

### <a name="delete-a-data-lake-analytics-account"></a><span data-ttu-id="1b198-118">Ta bort ett Data Lake Analytics-konto</span><span class="sxs-lookup"><span data-stu-id="1b198-118">Delete a Data Lake Analytics account</span></span>

<span data-ttu-id="1b198-119">Innan du tar bort ett Data Lake Analytics-konto kan du ta bort dess Data Lake Store-standardkontot.</span><span class="sxs-lookup"><span data-stu-id="1b198-119">Before you delete a Data Lake Analytics account, delete its default Data Lake Store account.</span></span>

1. <span data-ttu-id="1b198-120">Gå till ditt Data Lake Analytics-konto i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1b198-120">In the Azure portal, go to your Data Lake Analytics account.</span></span>
2. <span data-ttu-id="1b198-121">Klicka på **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="1b198-121">Click **Delete**.</span></span>
3. <span data-ttu-id="1b198-122">Skriv namnet på kontot.</span><span class="sxs-lookup"><span data-stu-id="1b198-122">Type the account name.</span></span>
4. <span data-ttu-id="1b198-123">Klicka på **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="1b198-123">Click **Delete**.</span></span>

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-data-sources"></a><span data-ttu-id="1b198-124">Hantera datakällor</span><span class="sxs-lookup"><span data-stu-id="1b198-124">Manage data sources</span></span>

<span data-ttu-id="1b198-125">Data Lake Analytics stöder följande datakällor:</span><span class="sxs-lookup"><span data-stu-id="1b198-125">Data Lake Analytics supports the following data sources:</span></span>

* <span data-ttu-id="1b198-126">Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="1b198-126">Data Lake Store</span></span>
* <span data-ttu-id="1b198-127">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="1b198-127">Azure Storage</span></span>

<span data-ttu-id="1b198-128">Du kan använda Data Explorer för att bläddra igenom datakällor och utföra grundläggande filhanteringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="1b198-128">You can use Data Explorer to browse data sources and perform basic file management operations.</span></span> 

### <a name="add-a-data-source"></a><span data-ttu-id="1b198-129">Lägga till en datakälla</span><span class="sxs-lookup"><span data-stu-id="1b198-129">Add a data source</span></span>

1. <span data-ttu-id="1b198-130">Gå till ditt Data Lake Analytics-konto i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1b198-130">In the Azure portal, go to your Data Lake Analytics account.</span></span>
2. <span data-ttu-id="1b198-131">Klicka på **datakällor**.</span><span class="sxs-lookup"><span data-stu-id="1b198-131">Click **Data Sources**.</span></span>
3. <span data-ttu-id="1b198-132">Klicka på **lägga till en datakälla**.</span><span class="sxs-lookup"><span data-stu-id="1b198-132">Click **Add Data Source**.</span></span>
    
   * <span data-ttu-id="1b198-133">Om du vill lägga till ett Data Lake Store-konto, behöver du kontonamnet och åtkomst till kontot för att kunna läsa den.</span><span class="sxs-lookup"><span data-stu-id="1b198-133">To add a Data Lake Store account, you need the account name and access to the account to be able to query it.</span></span>
   * <span data-ttu-id="1b198-134">Om du vill lägga till Azure Blob storage, behöver du lagringskontot och nyckeln för kontot.</span><span class="sxs-lookup"><span data-stu-id="1b198-134">To add Azure Blob storage, you need the storage account and the account key.</span></span> <span data-ttu-id="1b198-135">Du hittar dem genom att gå till lagringskontot på portalen.</span><span class="sxs-lookup"><span data-stu-id="1b198-135">To find them, go to the storage account in the portal.</span></span>

## <a name="set-up-firewall-rules"></a><span data-ttu-id="1b198-136">Konfigurera brandväggsregler</span><span class="sxs-lookup"><span data-stu-id="1b198-136">Set up firewall rules</span></span>

<span data-ttu-id="1b198-137">Du kan använda Data Lake Analytics att ytterligare låsa åtkomst till ditt Data Lake Analytics-konto på nätverksnivån.</span><span class="sxs-lookup"><span data-stu-id="1b198-137">You can use Data Lake Analytics to further lock down access to your Data Lake Analytics account at the network level.</span></span> <span data-ttu-id="1b198-138">Du kan aktivera en brandvägg, ange en IP-adress eller definiera ett intervall med IP-adresser för dina betrodda klienter.</span><span class="sxs-lookup"><span data-stu-id="1b198-138">You can enable a firewall, specify an IP address, or define an IP address range for your trusted clients.</span></span> <span data-ttu-id="1b198-139">När du har aktiverat dessa åtgärder kan endast klienter som har IP-adresser inom det angivna intervallet ansluta till arkivet.</span><span class="sxs-lookup"><span data-stu-id="1b198-139">After you enable these measures, only clients that have the IP addresses within the defined range can connect to the store.</span></span>

<span data-ttu-id="1b198-140">Om andra Azure-tjänster, t.ex. Azure Data Factory eller virtuella datorer kan ansluta till Data Lake Analytics-kontot, kontrollerar du att **Tillåt Azure Services** är aktiverat **på**.</span><span class="sxs-lookup"><span data-stu-id="1b198-140">If other Azure services, like Azure Data Factory or VMs, connect to the Data Lake Analytics account, make sure that **Allow Azure Services** is turned **On**.</span></span> 

### <a name="set-up-a-firewall-rule"></a><span data-ttu-id="1b198-141">Konfigurera en brandväggsregel</span><span class="sxs-lookup"><span data-stu-id="1b198-141">Set up a firewall rule</span></span>

1. <span data-ttu-id="1b198-142">Gå till ditt Data Lake Analytics-konto i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1b198-142">In the Azure portal, go to your Data Lake Analytics account.</span></span>
2. <span data-ttu-id="1b198-143">På menyn till vänster **brandväggen**.</span><span class="sxs-lookup"><span data-stu-id="1b198-143">On the menu on the left, click **Firewall**.</span></span>

## <a name="add-a-new-user"></a><span data-ttu-id="1b198-144">Lägg till en ny användare</span><span class="sxs-lookup"><span data-stu-id="1b198-144">Add a new user</span></span>

<span data-ttu-id="1b198-145">Du kan använda den **guiden Lägg till användare** att enkelt etablera nya Data Lake-användare.</span><span class="sxs-lookup"><span data-stu-id="1b198-145">You can use the **Add User Wizard** to easily provision new Data Lake users.</span></span>

1. <span data-ttu-id="1b198-146">Gå till ditt Data Lake Analytics-konto i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1b198-146">In the Azure portal, go to your Data Lake Analytics account.</span></span>
2. <span data-ttu-id="1b198-147">Till vänster under **komma igång**, klickar du på **guiden Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="1b198-147">On the left, under **Getting Started**, click **Add User Wizard**.</span></span>
3. <span data-ttu-id="1b198-148">Välj en användare och klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="1b198-148">Select a user, and then click **Select**.</span></span>
4. <span data-ttu-id="1b198-149">Välj en roll och klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="1b198-149">Select a role, and then click **Select**.</span></span> <span data-ttu-id="1b198-150">Skapa en ny utvecklare att använda Azure Data Lake, välja den **Data Lake Analytics Developer** roll.</span><span class="sxs-lookup"><span data-stu-id="1b198-150">To set up a new developer to use Azure Data Lake, select the **Data Lake Analytics Developer** role.</span></span>
5. <span data-ttu-id="1b198-151">Välj åtkomstkontrollistor (ACL) för U-SQL-databaser.</span><span class="sxs-lookup"><span data-stu-id="1b198-151">Select the access control lists (ACLs) for the U-SQL databases.</span></span> <span data-ttu-id="1b198-152">När du är nöjd med dina val klickar du på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="1b198-152">When you're satisfied with your choices, click **Select**.</span></span>
6. <span data-ttu-id="1b198-153">Välj ACL: er för filer.</span><span class="sxs-lookup"><span data-stu-id="1b198-153">Select the ACLs for files.</span></span> <span data-ttu-id="1b198-154">Inte ändra ACL: er för rotmappen för arkivet standard ”/” och för mappen/System.</span><span class="sxs-lookup"><span data-stu-id="1b198-154">For the default store, don't change the ACLs for the root folder "/" and for the /system folder.</span></span> <span data-ttu-id="1b198-155">Klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="1b198-155">Click **Select**.</span></span>
7. <span data-ttu-id="1b198-156">Granska alla valda ändringarna och klicka sedan på **kör**.</span><span class="sxs-lookup"><span data-stu-id="1b198-156">Review all your selected changes, and then click **Run**.</span></span>
8. <span data-ttu-id="1b198-157">När guiden är klar klickar du på **klar**.</span><span class="sxs-lookup"><span data-stu-id="1b198-157">When the wizard is finished, click **Done**.</span></span>

## <a name="manage-role-based-access-control"></a><span data-ttu-id="1b198-158">Hantera rollbaserad åtkomstkontroll</span><span class="sxs-lookup"><span data-stu-id="1b198-158">Manage Role-Based Access Control</span></span>

<span data-ttu-id="1b198-159">Du kan använda rollbaserad åtkomstkontroll (RBAC) som andra Azure-tjänster för att styra hur användarna samverkar med tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1b198-159">Like other Azure services, you can use Role-Based Access Control (RBAC) to control how users interact with the service.</span></span>

<span data-ttu-id="1b198-160">Standard RBAC-roller har följande möjligheter:</span><span class="sxs-lookup"><span data-stu-id="1b198-160">The standard RBAC roles have the following capabilities:</span></span>
* <span data-ttu-id="1b198-161">**Ägare**: kan skicka jobb, övervaka jobb, avbryta jobb från alla användare och konfigurera kontot.</span><span class="sxs-lookup"><span data-stu-id="1b198-161">**Owner**: Can submit jobs, monitor jobs, cancel jobs from any user, and configure the account.</span></span>
* <span data-ttu-id="1b198-162">**Deltagare**: kan skicka jobb, övervaka jobb, avbryta jobb från alla användare och konfigurera kontot.</span><span class="sxs-lookup"><span data-stu-id="1b198-162">**Contributor**: Can submit jobs, monitor jobs, cancel jobs from any user, and configure the account.</span></span>
* <span data-ttu-id="1b198-163">**Läsaren**: övervaka jobb.</span><span class="sxs-lookup"><span data-stu-id="1b198-163">**Reader**: Can monitor jobs.</span></span>

<span data-ttu-id="1b198-164">Använd Data Lake Analytics Developer-rollen för att aktivera U-SQL-utvecklare kan använda Data Lake Analytics-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1b198-164">Use the Data Lake Analytics Developer role to enable U-SQL developers to use the Data Lake Analytics service.</span></span> <span data-ttu-id="1b198-165">Du kan använda rolltjänsten Data Lake Analytics utvecklare att:</span><span class="sxs-lookup"><span data-stu-id="1b198-165">You can use the Data Lake Analytics Developer role to:</span></span>
* <span data-ttu-id="1b198-166">Skicka jobb.</span><span class="sxs-lookup"><span data-stu-id="1b198-166">Submit jobs.</span></span>
* <span data-ttu-id="1b198-167">Övervaka jobbstatus och förlopp för jobb som skickats av någon användare.</span><span class="sxs-lookup"><span data-stu-id="1b198-167">Monitor job status and the progress of jobs submitted by any user.</span></span>
* <span data-ttu-id="1b198-168">Se U-SQL-skript från jobb som skickats av någon användare.</span><span class="sxs-lookup"><span data-stu-id="1b198-168">See the U-SQL scripts from jobs submitted by any user.</span></span>
* <span data-ttu-id="1b198-169">Avbryt endast egna jobb.</span><span class="sxs-lookup"><span data-stu-id="1b198-169">Cancel only your own jobs.</span></span>

### <a name="add-users-or-security-groups-to-a-data-lake-analytics-account"></a><span data-ttu-id="1b198-170">Lägga till användare eller säkerhetsgrupper till ett Data Lake Analytics-konto</span><span class="sxs-lookup"><span data-stu-id="1b198-170">Add users or security groups to a Data Lake Analytics account</span></span>

1. <span data-ttu-id="1b198-171">Gå till ditt Data Lake Analytics-konto i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1b198-171">In the Azure portal, go to your Data Lake Analytics account.</span></span>
2. <span data-ttu-id="1b198-172">Klicka på **åtkomstkontroll (IAM)** > **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="1b198-172">Click **Access control (IAM)** > **Add**.</span></span>
3. <span data-ttu-id="1b198-173">Välj en roll.</span><span class="sxs-lookup"><span data-stu-id="1b198-173">Select a role.</span></span>
4. <span data-ttu-id="1b198-174">Lägga till en användare.</span><span class="sxs-lookup"><span data-stu-id="1b198-174">Add a user.</span></span>
5. <span data-ttu-id="1b198-175">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="1b198-175">Click **OK**.</span></span>

>[!NOTE]
><span data-ttu-id="1b198-176">Om en användare eller en säkerhetsgrupp måste du skicka jobb, måste de också behörighet till store-konto.</span><span class="sxs-lookup"><span data-stu-id="1b198-176">If a user or a security group needs to submit jobs, they also need permission on the store account.</span></span> <span data-ttu-id="1b198-177">Mer information finns i [skyddar data som lagras i Data Lake Store](../data-lake-store/data-lake-store-secure-data.md).</span><span class="sxs-lookup"><span data-stu-id="1b198-177">For more information, see [Secure data stored in Data Lake Store](../data-lake-store/data-lake-store-secure-data.md).</span></span>
>

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-jobs"></a><span data-ttu-id="1b198-178">Hantera jobb</span><span class="sxs-lookup"><span data-stu-id="1b198-178">Manage jobs</span></span>

### <a name="submit-a-job"></a><span data-ttu-id="1b198-179">Skicka ett jobb</span><span class="sxs-lookup"><span data-stu-id="1b198-179">Submit a job</span></span>

1. <span data-ttu-id="1b198-180">Gå till ditt Data Lake Analytics-konto i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1b198-180">In the Azure portal, go to your Data Lake Analytics account.</span></span>

2. <span data-ttu-id="1b198-181">Klicka på **nytt jobb**.</span><span class="sxs-lookup"><span data-stu-id="1b198-181">Click **New Job**.</span></span> <span data-ttu-id="1b198-182">Konfigurera följande för varje jobb:</span><span class="sxs-lookup"><span data-stu-id="1b198-182">For each job,  configure:</span></span>

    1. <span data-ttu-id="1b198-183">**Jobbnamnet**: namnet på jobbet.</span><span class="sxs-lookup"><span data-stu-id="1b198-183">**Job Name**: The name of the job.</span></span>
    2. <span data-ttu-id="1b198-184">**Prioritet**: lägre nummer har högre prioritet.</span><span class="sxs-lookup"><span data-stu-id="1b198-184">**Priority**: Lower numbers have higher priority.</span></span> <span data-ttu-id="1b198-185">Om två jobb i kö, körs med lägre prioritetsvärde första.</span><span class="sxs-lookup"><span data-stu-id="1b198-185">If two jobs are queued, the one with lower priority value runs first.</span></span>
    3. <span data-ttu-id="1b198-186">**Parallellitet**: det maximala antalet beräknings-processer att reservera för det här jobbet.</span><span class="sxs-lookup"><span data-stu-id="1b198-186">**Parallelism**: The maximum number of compute processes to reserve for this job.</span></span>

3. <span data-ttu-id="1b198-187">Klicka på **Skicka jobb**.</span><span class="sxs-lookup"><span data-stu-id="1b198-187">Click **Submit Job**.</span></span>

### <a name="monitor-jobs"></a><span data-ttu-id="1b198-188">Övervaka jobb</span><span class="sxs-lookup"><span data-stu-id="1b198-188">Monitor jobs</span></span>

1. <span data-ttu-id="1b198-189">Gå till ditt Data Lake Analytics-konto i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1b198-189">In the Azure portal, go to your Data Lake Analytics account.</span></span>
2. <span data-ttu-id="1b198-190">Klicka på **visa alla jobb**.</span><span class="sxs-lookup"><span data-stu-id="1b198-190">Click **View All Jobs**.</span></span> <span data-ttu-id="1b198-191">En lista över alla aktiva och nyligen klar jobb i kontot visas.</span><span class="sxs-lookup"><span data-stu-id="1b198-191">A list of all the active and recently finished jobs in the account is shown.</span></span>
3. <span data-ttu-id="1b198-192">Du kan också klicka på **Filter** för att hitta jobb efter **tidsintervall**, **jobbnamn**, och **författare** värden.</span><span class="sxs-lookup"><span data-stu-id="1b198-192">Optionally, click **Filter** to help you find the jobs by **Time Range**, **Job Name**, and **Author** values.</span></span> 

### <a name="monitoring-pipeline-jobs"></a><span data-ttu-id="1b198-193">Övervaka pipeline-jobb</span><span class="sxs-lookup"><span data-stu-id="1b198-193">Monitoring pipeline jobs</span></span>
<span data-ttu-id="1b198-194">Jobb som är en del av en pipeline fungerar tillsammans, vanligtvis sekventiellt för att uppnå ett specifikt scenario.</span><span class="sxs-lookup"><span data-stu-id="1b198-194">Jobs that are part of a pipeline work together, usually sequentially, to accomplish a specific scenario.</span></span> <span data-ttu-id="1b198-195">Du kan till exempel ha en pipeline som rensar, extraherar, omvandlar, aggregerar användning för kunden insikter.</span><span class="sxs-lookup"><span data-stu-id="1b198-195">For example, you can have a pipeline that cleans, extracts, transforms, aggregates usage for customer insights.</span></span> <span data-ttu-id="1b198-196">Pipeline-jobb identifieras med hjälp av egenskapen ”Pipeline” när jobbet har skickats.</span><span class="sxs-lookup"><span data-stu-id="1b198-196">Pipeline jobs are identified using the "Pipeline" property when the job was submitted.</span></span> <span data-ttu-id="1b198-197">Jobb som schemalagts med ADF V2 har automatiskt den här egenskapen fylls i.</span><span class="sxs-lookup"><span data-stu-id="1b198-197">Jobs scheduled using ADF V2 will automatically have this property populated.</span></span> 

<span data-ttu-id="1b198-198">Visa en lista över U-SQL-jobb som är del av pipelines:</span><span class="sxs-lookup"><span data-stu-id="1b198-198">To view a list of U-SQL jobs that are part of pipelines:</span></span> 

1. <span data-ttu-id="1b198-199">Gå till din Data Lake Analytics-konton i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1b198-199">In the Azure portal, go to your Data Lake Analytics accounts.</span></span>
2. <span data-ttu-id="1b198-200">Klicka på **jobbet insikter**.</span><span class="sxs-lookup"><span data-stu-id="1b198-200">Click **Job Insights**.</span></span> <span data-ttu-id="1b198-201">På fliken ”alla jobb” blir som standard, visar en lista över körs, i kö och jobb avslutades.</span><span class="sxs-lookup"><span data-stu-id="1b198-201">The "All Jobs" tab will be defaulted, showing a list of running, queued, and ended jobs.</span></span>
3. <span data-ttu-id="1b198-202">Klicka på den **Pipeline jobb** fliken. En lista över pipeline jobb visas tillsammans med statistik för varje pipelinen.</span><span class="sxs-lookup"><span data-stu-id="1b198-202">Click the **Pipeline Jobs** tab. A list of pipeline jobs will be shown along with aggregated statistics for each pipeline.</span></span>

### <a name="monitoring-recurring-jobs"></a><span data-ttu-id="1b198-203">Övervaka återkommande jobb</span><span class="sxs-lookup"><span data-stu-id="1b198-203">Monitoring recurring jobs</span></span>
<span data-ttu-id="1b198-204">Ett återkommande jobb är en som har samma affärslogik men använder olika indata varje gång det körs.</span><span class="sxs-lookup"><span data-stu-id="1b198-204">A recurring job is one that has the same business logic but uses different input data every time it runs.</span></span> <span data-ttu-id="1b198-205">Vi rekommenderar bör återkommande jobb alltid lyckas och har stabilt körningstid; övervakning av dessa beteenden säkerställer jobbet är felfri.</span><span class="sxs-lookup"><span data-stu-id="1b198-205">Ideally, recurring jobs should always succeed, and have relatively stable execution time; monitoring these behaviors will help ensure the job is healthy.</span></span> <span data-ttu-id="1b198-206">Återkommande jobb identifieras med hjälp av egenskapen ”återkommande”.</span><span class="sxs-lookup"><span data-stu-id="1b198-206">Recurring jobs are identified using the "Recurrence" property.</span></span> <span data-ttu-id="1b198-207">Jobb som schemalagts med ADF V2 har automatiskt den här egenskapen fylls i.</span><span class="sxs-lookup"><span data-stu-id="1b198-207">Jobs scheduled using ADF V2 will automatically have this property populated.</span></span>

<span data-ttu-id="1b198-208">Visa en lista över U-SQL-jobb som är återkommande:</span><span class="sxs-lookup"><span data-stu-id="1b198-208">To view a list of U-SQL jobs that are recurring:</span></span> 

1. <span data-ttu-id="1b198-209">Gå till din Data Lake Analytics-konton i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1b198-209">In the Azure portal, go to your Data Lake Analytics accounts.</span></span>
2. <span data-ttu-id="1b198-210">Klicka på **jobbet insikter**.</span><span class="sxs-lookup"><span data-stu-id="1b198-210">Click **Job Insights**.</span></span> <span data-ttu-id="1b198-211">På fliken ”alla jobb” blir som standard, visar en lista över körs, i kö och jobb avslutades.</span><span class="sxs-lookup"><span data-stu-id="1b198-211">The "All Jobs" tab will be defaulted, showing a list of running, queued, and ended jobs.</span></span>
3. <span data-ttu-id="1b198-212">Klicka på den **återkommande jobb** fliken. En lista över återkommande jobb visas tillsammans med statistik för alla återkommande jobb.</span><span class="sxs-lookup"><span data-stu-id="1b198-212">Click the **Recurring Jobs** tab. A list of recurring jobs will be shown along with aggregated statistics for each recurring job.</span></span>

## <a name="manage-policies"></a><span data-ttu-id="1b198-213">Hantera principer</span><span class="sxs-lookup"><span data-stu-id="1b198-213">Manage policies</span></span>

### <a name="account-level-policies"></a><span data-ttu-id="1b198-214">Kontonivå principer</span><span class="sxs-lookup"><span data-stu-id="1b198-214">Account-level policies</span></span>

<span data-ttu-id="1b198-215">Dessa principer som gäller för alla jobb i ett Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="1b198-215">These policies apply to all jobs in a Data Lake Analytics account.</span></span>

#### <a name="maximum-number-of-aus-in-a-data-lake-analytics-account"></a><span data-ttu-id="1b198-216">Maximalt antal Australien i ett Data Lake Analytics-konto</span><span class="sxs-lookup"><span data-stu-id="1b198-216">Maximum number of AUs in a Data Lake Analytics account</span></span>
<span data-ttu-id="1b198-217">En princip styr det totala antalet Analytics-enheter (Australien) kan använda ditt Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="1b198-217">A policy controls the total number of Analytics Units (AUs) your Data Lake Analytics account can use.</span></span> <span data-ttu-id="1b198-218">Värdet är som standard är 250.</span><span class="sxs-lookup"><span data-stu-id="1b198-218">By default, the value is set to 250.</span></span> <span data-ttu-id="1b198-219">Om det här värdet är inställt på 250 exempelvis Australien, kan du ha ett jobb som körs med 250 Australien tilldelade eller 10 jobb som körs med 25 Australien varje.</span><span class="sxs-lookup"><span data-stu-id="1b198-219">For example, if this value is set to 250 AUs, you can have one job running with 250 AUs assigned to it, or 10 jobs running with 25 AUs each.</span></span> <span data-ttu-id="1b198-220">Ytterligare jobb som skickats köas förrän jobb som körs är klar.</span><span class="sxs-lookup"><span data-stu-id="1b198-220">Additional jobs that are submitted are queued until the running jobs are finished.</span></span> <span data-ttu-id="1b198-221">När jobb som körs är slutförda frigörs Australien för att köra köade jobb.</span><span class="sxs-lookup"><span data-stu-id="1b198-221">When running jobs are finished, AUs are freed up for the queued jobs to run.</span></span>

<span data-ttu-id="1b198-222">Ändra antalet Australien för ditt Data Lake Analytics-konto:</span><span class="sxs-lookup"><span data-stu-id="1b198-222">To change the number of AUs for your Data Lake Analytics account:</span></span>

1. <span data-ttu-id="1b198-223">Gå till ditt Data Lake Analytics-konto i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1b198-223">In the Azure portal, go to your Data Lake Analytics account.</span></span>
2. <span data-ttu-id="1b198-224">Klicka på **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="1b198-224">Click **Properties**.</span></span>
3. <span data-ttu-id="1b198-225">Under **maximala Australien**, flytta skjutreglaget för att välja ett värde eller ange värdet i textrutan.</span><span class="sxs-lookup"><span data-stu-id="1b198-225">Under **Maximum AUs**, move the slider to select a value, or enter the value in the text box.</span></span> 
4. <span data-ttu-id="1b198-226">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="1b198-226">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="1b198-227">Om du behöver mer än standardvärdet (250) Australien, i portalen klickar du på **hjälp + Support** att skicka en supportförfrågan.</span><span class="sxs-lookup"><span data-stu-id="1b198-227">If you need more than the default (250) AUs, in the portal, click **Help+Support** to submit a support request.</span></span> <span data-ttu-id="1b198-228">Antalet Australien som är tillgängliga i ditt Data Lake Analytics-konto kan du öka.</span><span class="sxs-lookup"><span data-stu-id="1b198-228">The number of AUs available in your Data Lake Analytics account can be increased.</span></span>
>

#### <a name="maximum-number-of-jobs-that-can-run-simultaneously"></a><span data-ttu-id="1b198-229">Maximalt antal jobb som kan köras samtidigt</span><span class="sxs-lookup"><span data-stu-id="1b198-229">Maximum number of jobs that can run simultaneously</span></span>
<span data-ttu-id="1b198-230">En princip styr hur många jobb kan köras samtidigt.</span><span class="sxs-lookup"><span data-stu-id="1b198-230">A policy controls how many jobs can run at the same time.</span></span> <span data-ttu-id="1b198-231">Det här värdet är som standard till 20.</span><span class="sxs-lookup"><span data-stu-id="1b198-231">By default, this value is set to 20.</span></span> <span data-ttu-id="1b198-232">Om Data Lake Analytics har Australien som är tillgängliga, är nya jobb schemalagda att köras omedelbart tills det totala antalet jobb som körs värdet för den här principen.</span><span class="sxs-lookup"><span data-stu-id="1b198-232">If your Data Lake Analytics has AUs available, new jobs are scheduled to run immediately until the total number of running jobs reaches the value of this policy.</span></span> <span data-ttu-id="1b198-233">När du når det maximala antalet jobb som kan köras samtidigt efterföljande jobb ställs i kö i prioritetsordning tills en eller flera av de jobb som körs är klar (beroende på Australien tillgänglighet).</span><span class="sxs-lookup"><span data-stu-id="1b198-233">When you reach the maximum number of jobs that can run simultaneously, subsequent jobs are queued in priority order until one or more running jobs complete (depending on AU availability).</span></span>

<span data-ttu-id="1b198-234">Ändra antalet jobb som kan köras samtidigt:</span><span class="sxs-lookup"><span data-stu-id="1b198-234">To change the number of jobs that can run simultaneously:</span></span>

1. <span data-ttu-id="1b198-235">Gå till ditt Data Lake Analytics-konto i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1b198-235">In the Azure portal, go to your Data Lake Analytics account.</span></span>
2. <span data-ttu-id="1b198-236">Klicka på **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="1b198-236">Click **Properties**.</span></span>
3. <span data-ttu-id="1b198-237">Under **maximala antalet av kör jobb**, flytta skjutreglaget för att välja ett värde eller ange värdet i textrutan.</span><span class="sxs-lookup"><span data-stu-id="1b198-237">Under **Maximum Number of Running Jobs**, move the slider to select a value, or enter the value in the text box.</span></span> 
4. <span data-ttu-id="1b198-238">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="1b198-238">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="1b198-239">Om du behöver köra fler än antalet jobb, standard (20) i portalen klickar du på **hjälp + Support** att skicka en supportförfrågan.</span><span class="sxs-lookup"><span data-stu-id="1b198-239">If you need to run more than the default (20) number of jobs, in the portal, click **Help+Support** to submit a support request.</span></span> <span data-ttu-id="1b198-240">Antalet jobb som kan köras samtidigt i ditt Data Lake Analytics-konto kan du öka.</span><span class="sxs-lookup"><span data-stu-id="1b198-240">The number of jobs that can run simultaneously in your Data Lake Analytics account can be increased.</span></span>
>

#### <a name="how-long-to-keep-job-metadata-and-resources"></a><span data-ttu-id="1b198-241">Hur lång tid att behålla jobbet metadata och resurser</span><span class="sxs-lookup"><span data-stu-id="1b198-241">How long to keep job metadata and resources</span></span> 
<span data-ttu-id="1b198-242">När användarna kör U-SQL-jobb, behåller alla relaterade filer i Data Lake Analytics-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1b198-242">When your users run U-SQL jobs, the Data Lake Analytics service retains all related files.</span></span> <span data-ttu-id="1b198-243">Relaterade filer innehåller U-SQL-skript, DLL-filer som refereras i U-SQL-skript, kompilerade resurser och statistik.</span><span class="sxs-lookup"><span data-stu-id="1b198-243">Related files include the U-SQL script, the DLL files referenced in the U-SQL script, compiled resources, and statistics.</span></span> <span data-ttu-id="1b198-244">Filerna som finns i mappen /system/ i Azure Data Lake-standardlagringskontot.</span><span class="sxs-lookup"><span data-stu-id="1b198-244">The files are in the /system/ folder of the default Azure Data Lake Storage account.</span></span> <span data-ttu-id="1b198-245">Den här principen styr hur länge dessa resurser lagras innan de tas bort automatiskt (standardvärdet är 30 dagar).</span><span class="sxs-lookup"><span data-stu-id="1b198-245">This policy controls how long these resources are stored before they are automatically deleted (the default is 30 days).</span></span> <span data-ttu-id="1b198-246">Du kan använda dessa filer för felsökning och prestandajustering av jobb som du ska köra i framtiden.</span><span class="sxs-lookup"><span data-stu-id="1b198-246">You can use these files for debugging, and for performance-tuning of jobs that you'll rerun in the future.</span></span>

<span data-ttu-id="1b198-247">Ändra hur lång tid för att behålla jobbet metadata och resurser:</span><span class="sxs-lookup"><span data-stu-id="1b198-247">To change how long to keep job metadata and resources:</span></span>

1. <span data-ttu-id="1b198-248">Gå till ditt Data Lake Analytics-konto i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1b198-248">In the Azure portal, go to your Data Lake Analytics account.</span></span>
2. <span data-ttu-id="1b198-249">Klicka på **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="1b198-249">Click **Properties**.</span></span>
3. <span data-ttu-id="1b198-250">Under **dagar för att behålla Jobbfrågor**, flytta skjutreglaget för att välja ett värde eller ange värdet i textrutan.</span><span class="sxs-lookup"><span data-stu-id="1b198-250">Under **Days to Retain Job Queries**, move the slider to select a value, or enter the value in the text box.</span></span>  
4. <span data-ttu-id="1b198-251">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="1b198-251">Click **Save**.</span></span>

### <a name="job-level-policies"></a><span data-ttu-id="1b198-252">Nivå principer</span><span class="sxs-lookup"><span data-stu-id="1b198-252">Job-level policies</span></span>
<span data-ttu-id="1b198-253">Du kan styra de maximala Australien och maximal prioritet som enskilda användare (eller medlemmar i specifika säkerhetsgrupper) kan ställa in för jobb som de skickar med principer för jobbet på objektnivå.</span><span class="sxs-lookup"><span data-stu-id="1b198-253">With job-level policies, you can control the maximum AUs and the maximum priority that individual users (or members of specific security groups) can set on jobs that they submit.</span></span> <span data-ttu-id="1b198-254">På så sätt kan du styra kostnader användare.</span><span class="sxs-lookup"><span data-stu-id="1b198-254">This lets you control the costs incurred by users.</span></span> <span data-ttu-id="1b198-255">Du kan också kontrollera att schemalagda jobb kan ha på hög prioritet produktionsjobb som körs i samma Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="1b198-255">It also lets you control the effect that scheduled jobs might have on high-priority production jobs that are running in the same Data Lake Analytics account.</span></span>

<span data-ttu-id="1b198-256">Data Lake Analytics har två principer som du kan ange på jobbnivån:</span><span class="sxs-lookup"><span data-stu-id="1b198-256">Data Lake Analytics has two policies that you can set at the job level:</span></span>

* <span data-ttu-id="1b198-257">**Australien gränsen per jobb**: användare kan bara skicka jobb som behöver det här antalet Australien.</span><span class="sxs-lookup"><span data-stu-id="1b198-257">**AU limit per job**: Users can only submit jobs that have up to this number of AUs.</span></span> <span data-ttu-id="1b198-258">Som standard är den här gränsen detsamma som Australien maxgränsen för kontot.</span><span class="sxs-lookup"><span data-stu-id="1b198-258">By default, this limit is the same as the maximum AU limit for the account.</span></span>
* <span data-ttu-id="1b198-259">**Prioritet**: användare kan bara skicka jobb som har en prioritet lägre än eller lika med det här värdet.</span><span class="sxs-lookup"><span data-stu-id="1b198-259">**Priority**: Users can only submit jobs that have a priority lower than or equal to this value.</span></span> <span data-ttu-id="1b198-260">Observera att ett högre värde innebär en lägre prioritet.</span><span class="sxs-lookup"><span data-stu-id="1b198-260">Note that a higher number means a lower priority.</span></span> <span data-ttu-id="1b198-261">Detta är som standard till 1, vilket är den högsta möjliga prioriteten.</span><span class="sxs-lookup"><span data-stu-id="1b198-261">By default, this is set to 1, which is the highest possible priority.</span></span>

<span data-ttu-id="1b198-262">Det finns en standardprincip som anges för varje konto.</span><span class="sxs-lookup"><span data-stu-id="1b198-262">There is a default policy set on every account.</span></span> <span data-ttu-id="1b198-263">Standardprincipen gäller för alla användare av kontot.</span><span class="sxs-lookup"><span data-stu-id="1b198-263">The default policy applies to all users of the account.</span></span> <span data-ttu-id="1b198-264">Du kan ange ytterligare principer för specifika användare och grupper.</span><span class="sxs-lookup"><span data-stu-id="1b198-264">You can set additional policies for specific users and groups.</span></span> 

> [!NOTE]
> <span data-ttu-id="1b198-265">Kontonivå och nivå principer tillämpas samtidigt.</span><span class="sxs-lookup"><span data-stu-id="1b198-265">Account-level policies and job-level policies apply simultaneously.</span></span>
>

#### <a name="add-a-policy-for-a-specific-user-or-group"></a><span data-ttu-id="1b198-266">Lägg till en princip för en specifik användare eller grupp</span><span class="sxs-lookup"><span data-stu-id="1b198-266">Add a policy for a specific user or group</span></span>

1. <span data-ttu-id="1b198-267">Gå till ditt Data Lake Analytics-konto i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1b198-267">In the Azure portal, go to your Data Lake Analytics account.</span></span>
2. <span data-ttu-id="1b198-268">Klicka på **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="1b198-268">Click **Properties**.</span></span>
3. <span data-ttu-id="1b198-269">Under **jobbet skicka gränser**, klicka på den **Lägg till princip** knappen.</span><span class="sxs-lookup"><span data-stu-id="1b198-269">Under **Job Submission Limits**, click the **Add Policy** button.</span></span> <span data-ttu-id="1b198-270">Sedan, Välj eller ange följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="1b198-270">Then, select or enter the following settings:</span></span>
    1. <span data-ttu-id="1b198-271">**Beräkna principnamn**: Ange ett principnamn, för att påminna dig om syftet med principen.</span><span class="sxs-lookup"><span data-stu-id="1b198-271">**Compute Policy Name**: Enter a policy name, to remind you of the purpose of the policy.</span></span>
    2. <span data-ttu-id="1b198-272">**Välj användare eller grupp**: Välj den användare eller grupp som den här principen gäller för.</span><span class="sxs-lookup"><span data-stu-id="1b198-272">**Select User or Group**: Select the user or group this policy applies to.</span></span>
    3. <span data-ttu-id="1b198-273">**Ställ in gräns för jobbet Australien**: Ställ in gräns för automatiska uppdateringar som gäller för den markerade användaren eller gruppen.</span><span class="sxs-lookup"><span data-stu-id="1b198-273">**Set the Job AU Limit**: Set the AU limit that applies to the selected user or group.</span></span>
    4. <span data-ttu-id="1b198-274">**Ställ in gräns för prioritet**: Ställ in gräns för prioritet som gäller för den markerade användaren eller gruppen.</span><span class="sxs-lookup"><span data-stu-id="1b198-274">**Set the Priority Limit**: Set the priority limit that applies to the selected user or group.</span></span>

4. <span data-ttu-id="1b198-275">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="1b198-275">Click **Ok**.</span></span>

5. <span data-ttu-id="1b198-276">Den nya principen visas i den **standard** princip tabell under **jobbet skicka gränser**.</span><span class="sxs-lookup"><span data-stu-id="1b198-276">The new policy is listed in the **Default** policy table, under **Job Submission Limits**.</span></span> 

#### <a name="delete-or-edit-an-existing-policy"></a><span data-ttu-id="1b198-277">Ta bort eller redigera en befintlig princip</span><span class="sxs-lookup"><span data-stu-id="1b198-277">Delete or edit an existing policy</span></span>

1. <span data-ttu-id="1b198-278">Gå till ditt Data Lake Analytics-konto i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1b198-278">In the Azure portal, go to your Data Lake Analytics account.</span></span>
2. <span data-ttu-id="1b198-279">Klicka på **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="1b198-279">Click **Properties**.</span></span>
3. <span data-ttu-id="1b198-280">Under **jobbet skicka gränser**, hitta principen du vill redigera.</span><span class="sxs-lookup"><span data-stu-id="1b198-280">Under **Job Submission Limits**, find the policy you want to edit.</span></span>
4.  <span data-ttu-id="1b198-281">Se den **ta bort** och **redigera** , i kolumnen längst till höger i tabellen, klicka på **...** .</span><span class="sxs-lookup"><span data-stu-id="1b198-281">To see the **Delete** and **Edit** options, in the rightmost column of the table, click **...**.</span></span>

### <a name="additional-resources-for-job-policies"></a><span data-ttu-id="1b198-282">Ytterligare resurser för jobbet principer</span><span class="sxs-lookup"><span data-stu-id="1b198-282">Additional resources for job policies</span></span>
* [<span data-ttu-id="1b198-283">Blogginlägget för principen: översikt</span><span class="sxs-lookup"><span data-stu-id="1b198-283">Policy overview blog post</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-overview/)
* [<span data-ttu-id="1b198-284">Principer för kontonivå blogginlägget</span><span class="sxs-lookup"><span data-stu-id="1b198-284">Account-level policies blog post</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-account-level-policy/)
* [<span data-ttu-id="1b198-285">Principer jobb på blogginlägget</span><span class="sxs-lookup"><span data-stu-id="1b198-285">Job-level policies blog post</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-job-level-policy/)

## <a name="next-steps"></a><span data-ttu-id="1b198-286">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1b198-286">Next steps</span></span>

* [<span data-ttu-id="1b198-287">Översikt över Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="1b198-287">Overview of Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="1b198-288">Kom igång med Data Lake Analytics med hjälp av Azure portal</span><span class="sxs-lookup"><span data-stu-id="1b198-288">Get started with Data Lake Analytics by using the Azure portal</span></span>](data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="1b198-289">Hantera Azure Data Lake Analytics med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="1b198-289">Manage Azure Data Lake Analytics by using Azure PowerShell</span></span>](data-lake-analytics-manage-use-powershell.md)

