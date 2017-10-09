---
title: "aaaAzure kvotgränserna för Data Lake Analytics | Microsoft Docs"
description: "Lär dig hur tooadjust och öka kvoten begränsar i Azure Data Lake Analytics (ADLA)-konton."
services: data-lake-analytics
keywords: Azure Data Lake Analytics
documentationcenter: 
author: omidm1
editor: omidm1
ms.assetid: 49416f38-fcc7-476f-a55e-d67f3f9c1d34
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/18/2017
ms.author: omidm
ms.openlocfilehash: 875c4d00e0c57414031e50754495c02162bdca48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-lake-analytics-quota-limits"></a><span data-ttu-id="db037-104">Kvotgränserna för Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="db037-104">Azure Data Lake Analytics Quota Limits</span></span>

<span data-ttu-id="db037-105">Lär dig hur tooadjust och öka kvoten begränsar i Azure Data Lake Analytics (ADLA)-konton.</span><span class="sxs-lookup"><span data-stu-id="db037-105">Learn how tooadjust and increase quota limits in Azure Data Lake Analytics (ADLA) accounts.</span></span> <span data-ttu-id="db037-106">Känna till dessa begränsningar kan hjälpa dig förstå din U-SQL-jobbet beteende.</span><span class="sxs-lookup"><span data-stu-id="db037-106">Knowing these limits may help you understand your U-SQL job behavior.</span></span> <span data-ttu-id="db037-107">Alla kvotgränserna är mjuka, så du kan öka hello maximala gränsen genom att nå ut toous.</span><span class="sxs-lookup"><span data-stu-id="db037-107">All quota limits are soft, so you can increase hello maximum limits by reaching out toous.</span></span>

## <a name="azure-subscriptions-limits"></a><span data-ttu-id="db037-108">Azure-prenumerationer gränser</span><span class="sxs-lookup"><span data-stu-id="db037-108">Azure subscriptions limits</span></span>

<span data-ttu-id="db037-109">**Maximalt antal ADLA konton per prenumeration:** 5</span><span class="sxs-lookup"><span data-stu-id="db037-109">**Maximum number of ADLA accounts per subscription:**  5</span></span>

 <span data-ttu-id="db037-110">Detta är hello maximalt antal ADLA konton som du kan skapa per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="db037-110">This is hello maximum number of ADLA accounts you can create per subscription.</span></span> <span data-ttu-id="db037-111">Om du försöker toocreate en sjätte ADLA konto, får du felmeddelandet ”du har nått hello maximalt antal Data Lake Analytics-konton tillåts (5) i regionen under prenumerationsnamn”.</span><span class="sxs-lookup"><span data-stu-id="db037-111">If you try toocreate a sixth ADLA account, you will get an error "You have reached hello maximum number of Data Lake Analytics accounts allowed (5) in region under subscription name".</span></span> <span data-ttu-id="db037-112">I det här fallet antingen ta bort alla oanvända ADLA konton eller nå ut toous av [öppna ett supportärende](#increase-maximum-quota-limits).</span><span class="sxs-lookup"><span data-stu-id="db037-112">In this case, either delete any unused ADLA accounts, or reach out toous by [opening a support ticket](#increase-maximum-quota-limits).</span></span>

## <a name="adla-account-limits"></a><span data-ttu-id="db037-113">ADLA gränser</span><span class="sxs-lookup"><span data-stu-id="db037-113">ADLA account limits</span></span>

<span data-ttu-id="db037-114">**Maximalt antal Analytics (Australien) per konto:** 250</span><span class="sxs-lookup"><span data-stu-id="db037-114">**Maximum number of Analytics Units (AUs) per account:** 250</span></span>

<span data-ttu-id="db037-115">Detta är hello maxantalet Australien som kan köras samtidigt i ditt konto.</span><span class="sxs-lookup"><span data-stu-id="db037-115">This is hello maximum number of AUs that can run concurrently in your account.</span></span> <span data-ttu-id="db037-116">Om din totala kör Australien över alla jobb som överskrider denna gräns, nya jobb ställs i kö automatiskt.</span><span class="sxs-lookup"><span data-stu-id="db037-116">If your total running AUs across all jobs exceeds this limit, newer jobs are queued automatically.</span></span> <span data-ttu-id="db037-117">Exempel:</span><span class="sxs-lookup"><span data-stu-id="db037-117">For example:</span></span>

* <span data-ttu-id="db037-118">Om du har endast ett jobb körs med 250 Australien när du skickar en andra jobb den kommer att vänta i jobbkön hello tills hello första jobbet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="db037-118">If you have only one job running with 250 AUs, when you submit a second job it will wait in hello job queue until hello first job completes.</span></span>
* <span data-ttu-id="db037-119">Om du redan har fem jobb som körs och var och en med 50 Australien när du skickar ett sjätte jobb som behöver 20 Australien den väntar i jobbkön hello tills det finns 20 Australien som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="db037-119">If you already have five jobs running and each is using 50 AUs, when you submit a sixth job that needs 20 AUs it waits in hello job queue until there are 20 AUs available.</span></span>

<span data-ttu-id="db037-120">**Maximalt antal samtidiga U-SQL-jobb per konto:** 20</span><span class="sxs-lookup"><span data-stu-id="db037-120">**Maximum number of concurrent U-SQL jobs per account:** 20</span></span>

<span data-ttu-id="db037-121">Detta är hello maximalt antal jobb som kan köras samtidigt i ditt konto.</span><span class="sxs-lookup"><span data-stu-id="db037-121">This is hello maximum number of jobs that can run concurrently in your account.</span></span> <span data-ttu-id="db037-122">Ovanför det här värdet senare jobb ställs i kö automatiskt.</span><span class="sxs-lookup"><span data-stu-id="db037-122">Above this value, newer jobs are queued automatically.</span></span>

## <a name="adjust-adla-quota-limits-per-account"></a><span data-ttu-id="db037-123">Justera ADLA kvotgränserna per konto</span><span class="sxs-lookup"><span data-stu-id="db037-123">Adjust ADLA quota limits per account</span></span>

1. <span data-ttu-id="db037-124">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="db037-124">Sign on toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="db037-125">Välj ett befintligt ADLA-konto.</span><span class="sxs-lookup"><span data-stu-id="db037-125">Choose an existing ADLA account.</span></span>
3. <span data-ttu-id="db037-126">Klicka på **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="db037-126">Click **Properties**.</span></span>
4. <span data-ttu-id="db037-127">Justera **parallellitet** och **samtidiga jobb** toosuit dina behov.</span><span class="sxs-lookup"><span data-stu-id="db037-127">Adjust **Parallelism** and **Concurrent Jobs** toosuit your needs.</span></span>

    ![Azure Data Lake Analytics-portalblad](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-properties.png)

## <a name="increase-maximum-quota-limits"></a><span data-ttu-id="db037-129">Öka maximal kvotgränser</span><span class="sxs-lookup"><span data-stu-id="db037-129">Increase maximum quota limits</span></span>

1. <span data-ttu-id="db037-130">Öppna ett supportärende i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="db037-130">Open a support request in Azure Portal.</span></span>

    ![Azure Data Lake Analytics-portalblad](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-help-support.png)

    ![Azure Data Lake Analytics-portalblad](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request.png)
2. <span data-ttu-id="db037-133">Välj typ av problem hello **kvot**.</span><span class="sxs-lookup"><span data-stu-id="db037-133">Select hello issue type **Quota**.</span></span>
3. <span data-ttu-id="db037-134">Välj din **prenumeration** (Kontrollera att den inte är en ”” utvärderingsprenumeration).</span><span class="sxs-lookup"><span data-stu-id="db037-134">Select your **Subscription** (make sure it is not a "trial" subscription).</span></span>
4. <span data-ttu-id="db037-135">Välj typ av kvot **Datasjöanalys**.</span><span class="sxs-lookup"><span data-stu-id="db037-135">Select quota type **Data Lake Analytics**.</span></span>

    ![Azure Data Lake Analytics-portalblad](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request-basics.png)

5. <span data-ttu-id="db037-137">I bladet problemet hello förklarar begärda öka gränsen med **information** av varför du behöver den här extra kapacitet.</span><span class="sxs-lookup"><span data-stu-id="db037-137">In hello problem blade, explain your requested increase limit with **Details** of why you need this extra capacity.</span></span>

    ![Azure Data Lake Analytics-portalblad](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request-details.png)

6. <span data-ttu-id="db037-139">Kontrollera din kontaktinformation och skapa hello supportförfrågan.</span><span class="sxs-lookup"><span data-stu-id="db037-139">Verify your contact information and create hello support request.</span></span>

<span data-ttu-id="db037-140">Microsoft har granskat din begäran och försöker tooaccommodate verksamheten behöver så snart som möjligt.</span><span class="sxs-lookup"><span data-stu-id="db037-140">Microsoft reviews your request and tries tooaccommodate your business needs as soon as possible.</span></span>

## <a name="next-steps"></a><span data-ttu-id="db037-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="db037-141">Next steps</span></span>

* [<span data-ttu-id="db037-142">Översikt över Microsoft Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="db037-142">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="db037-143">Hantera Azure Data Lake Analytics med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="db037-143">Manage Azure Data Lake Analytics using Azure PowerShell</span></span>](data-lake-analytics-manage-use-powershell.md)
* [<span data-ttu-id="db037-144">Övervaka och felsök Azure Data Lake Analytics-jobb med hjälp av Azure Portal</span><span class="sxs-lookup"><span data-stu-id="db037-144">Monitor and troubleshoot Azure Data Lake Analytics jobs using Azure portal</span></span>](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
