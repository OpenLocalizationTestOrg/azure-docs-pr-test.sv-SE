---
title: "Kvotgränserna för Azure Data Lake Analytics | Microsoft Docs"
description: "Lär dig hur du justerar och öka kvotgränserna i Azure Data Lake Analytics (ADLA)-konton."
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
ms.openlocfilehash: 957f306ea0e80b5830ad64e5ef06c6d122d9eccc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-data-lake-analytics-quota-limits"></a><span data-ttu-id="4e0fa-104">Kvotgränserna för Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="4e0fa-104">Azure Data Lake Analytics Quota Limits</span></span>

<span data-ttu-id="4e0fa-105">Lär dig hur du justerar och öka kvotgränserna i Azure Data Lake Analytics (ADLA)-konton.</span><span class="sxs-lookup"><span data-stu-id="4e0fa-105">Learn how to adjust and increase quota limits in Azure Data Lake Analytics (ADLA) accounts.</span></span> <span data-ttu-id="4e0fa-106">Känna till dessa begränsningar kan hjälpa dig förstå din U-SQL-jobbet beteende.</span><span class="sxs-lookup"><span data-stu-id="4e0fa-106">Knowing these limits may help you understand your U-SQL job behavior.</span></span> <span data-ttu-id="4e0fa-107">Alla kvotgränserna är mjuka, så du kan öka de maximala gränserna genom att nå ut till oss.</span><span class="sxs-lookup"><span data-stu-id="4e0fa-107">All quota limits are soft, so you can increase the maximum limits by reaching out to us.</span></span>

## <a name="azure-subscriptions-limits"></a><span data-ttu-id="4e0fa-108">Azure-prenumerationer gränser</span><span class="sxs-lookup"><span data-stu-id="4e0fa-108">Azure subscriptions limits</span></span>

<span data-ttu-id="4e0fa-109">**Maximalt antal ADLA konton per prenumeration:** 5</span><span class="sxs-lookup"><span data-stu-id="4e0fa-109">**Maximum number of ADLA accounts per subscription:**  5</span></span>

 <span data-ttu-id="4e0fa-110">Detta är det maximala antalet ADLA konton som du kan skapa per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4e0fa-110">This is the maximum number of ADLA accounts you can create per subscription.</span></span> <span data-ttu-id="4e0fa-111">Om du försöker skapa ett sjätte ADLA konto får du felmeddelandet ”du har nått det maximala antalet Data Lake Analytics-konton tillåts (5) i regionen under prenumerationsnamn”.</span><span class="sxs-lookup"><span data-stu-id="4e0fa-111">If you try to create a sixth ADLA account, you will get an error "You have reached the maximum number of Data Lake Analytics accounts allowed (5) in region under subscription name".</span></span> <span data-ttu-id="4e0fa-112">I det här fallet antingen ta bort alla oanvända ADLA konton eller nå oss av [öppna ett supportärende](#increase-maximum-quota-limits).</span><span class="sxs-lookup"><span data-stu-id="4e0fa-112">In this case, either delete any unused ADLA accounts, or reach out to us by [opening a support ticket](#increase-maximum-quota-limits).</span></span>

## <a name="adla-account-limits"></a><span data-ttu-id="4e0fa-113">ADLA gränser</span><span class="sxs-lookup"><span data-stu-id="4e0fa-113">ADLA account limits</span></span>

<span data-ttu-id="4e0fa-114">**Maximalt antal Analytics (Australien) per konto:** 250</span><span class="sxs-lookup"><span data-stu-id="4e0fa-114">**Maximum number of Analytics Units (AUs) per account:** 250</span></span>

<span data-ttu-id="4e0fa-115">Detta är det maximala antalet Australien som kan köras samtidigt i ditt konto.</span><span class="sxs-lookup"><span data-stu-id="4e0fa-115">This is the maximum number of AUs that can run concurrently in your account.</span></span> <span data-ttu-id="4e0fa-116">Om din totala kör Australien över alla jobb som överskrider denna gräns, nya jobb ställs i kö automatiskt.</span><span class="sxs-lookup"><span data-stu-id="4e0fa-116">If your total running AUs across all jobs exceeds this limit, newer jobs are queued automatically.</span></span> <span data-ttu-id="4e0fa-117">Exempel:</span><span class="sxs-lookup"><span data-stu-id="4e0fa-117">For example:</span></span>

* <span data-ttu-id="4e0fa-118">Om du har endast ett jobb körs med 250 Australien när du skickar en andra jobb den kommer att vänta i jobbkön tills det första jobbet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="4e0fa-118">If you have only one job running with 250 AUs, when you submit a second job it will wait in the job queue until the first job completes.</span></span>
* <span data-ttu-id="4e0fa-119">Om du redan har fem jobb som körs och var och en med 50 Australien när du skickar ett sjätte jobb som behöver 20 Australien den väntar i jobbkön tills det finns 20 Australien som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="4e0fa-119">If you already have five jobs running and each is using 50 AUs, when you submit a sixth job that needs 20 AUs it waits in the job queue until there are 20 AUs available.</span></span>

<span data-ttu-id="4e0fa-120">**Maximalt antal samtidiga U-SQL-jobb per konto:** 20</span><span class="sxs-lookup"><span data-stu-id="4e0fa-120">**Maximum number of concurrent U-SQL jobs per account:** 20</span></span>

<span data-ttu-id="4e0fa-121">Detta är det maximala antalet jobb som kan köras samtidigt i ditt konto.</span><span class="sxs-lookup"><span data-stu-id="4e0fa-121">This is the maximum number of jobs that can run concurrently in your account.</span></span> <span data-ttu-id="4e0fa-122">Ovanför det här värdet senare jobb ställs i kö automatiskt.</span><span class="sxs-lookup"><span data-stu-id="4e0fa-122">Above this value, newer jobs are queued automatically.</span></span>

## <a name="adjust-adla-quota-limits-per-account"></a><span data-ttu-id="4e0fa-123">Justera ADLA kvotgränserna per konto</span><span class="sxs-lookup"><span data-stu-id="4e0fa-123">Adjust ADLA quota limits per account</span></span>

1. <span data-ttu-id="4e0fa-124">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4e0fa-124">Sign on to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="4e0fa-125">Välj ett befintligt ADLA-konto.</span><span class="sxs-lookup"><span data-stu-id="4e0fa-125">Choose an existing ADLA account.</span></span>
3. <span data-ttu-id="4e0fa-126">Klicka på **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="4e0fa-126">Click **Properties**.</span></span>
4. <span data-ttu-id="4e0fa-127">Justera **parallellitet** och **samtidiga jobb** som passar dina behov.</span><span class="sxs-lookup"><span data-stu-id="4e0fa-127">Adjust **Parallelism** and **Concurrent Jobs** to suit your needs.</span></span>

    ![Azure Data Lake Analytics-portalblad](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-properties.png)

## <a name="increase-maximum-quota-limits"></a><span data-ttu-id="4e0fa-129">Öka maximal kvotgränser</span><span class="sxs-lookup"><span data-stu-id="4e0fa-129">Increase maximum quota limits</span></span>

1. <span data-ttu-id="4e0fa-130">Öppna ett supportärende i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4e0fa-130">Open a support request in Azure Portal.</span></span>

    ![Azure Data Lake Analytics-portalblad](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-help-support.png)

    ![Azure Data Lake Analytics-portalblad](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request.png)
2. <span data-ttu-id="4e0fa-133">Välj typ av problem **kvot**.</span><span class="sxs-lookup"><span data-stu-id="4e0fa-133">Select the issue type **Quota**.</span></span>
3. <span data-ttu-id="4e0fa-134">Välj din **prenumeration** (Kontrollera att den inte är en ”” utvärderingsprenumeration).</span><span class="sxs-lookup"><span data-stu-id="4e0fa-134">Select your **Subscription** (make sure it is not a "trial" subscription).</span></span>
4. <span data-ttu-id="4e0fa-135">Välj typ av kvot **Datasjöanalys**.</span><span class="sxs-lookup"><span data-stu-id="4e0fa-135">Select quota type **Data Lake Analytics**.</span></span>

    ![Azure Data Lake Analytics-portalblad](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request-basics.png)

5. <span data-ttu-id="4e0fa-137">I bladet problemet förklarar begärda öka gränsen med **information** av varför du behöver den här extra kapacitet.</span><span class="sxs-lookup"><span data-stu-id="4e0fa-137">In the problem blade, explain your requested increase limit with **Details** of why you need this extra capacity.</span></span>

    ![Azure Data Lake Analytics-portalblad](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request-details.png)

6. <span data-ttu-id="4e0fa-139">Kontrollera din kontaktinformation och skapa supportbegäran.</span><span class="sxs-lookup"><span data-stu-id="4e0fa-139">Verify your contact information and create the support request.</span></span>

<span data-ttu-id="4e0fa-140">Microsoft har granskat din begäran och ett försök att passa dina behov så snart som möjligt.</span><span class="sxs-lookup"><span data-stu-id="4e0fa-140">Microsoft reviews your request and tries to accommodate your business needs as soon as possible.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4e0fa-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4e0fa-141">Next steps</span></span>

* [<span data-ttu-id="4e0fa-142">Översikt över Microsoft Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="4e0fa-142">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="4e0fa-143">Hantera Azure Data Lake Analytics med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="4e0fa-143">Manage Azure Data Lake Analytics using Azure PowerShell</span></span>](data-lake-analytics-manage-use-powershell.md)
* [<span data-ttu-id="4e0fa-144">Övervaka och felsök Azure Data Lake Analytics-jobb med hjälp av Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4e0fa-144">Monitor and troubleshoot Azure Data Lake Analytics jobs using Azure portal</span></span>](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
