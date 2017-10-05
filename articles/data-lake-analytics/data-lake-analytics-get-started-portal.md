---
title: "Kom igång med Azure Data Lake Analytics med hjälp av Azure Portal | Microsoft Docs"
description: "Lär dig att skapa ett Data Lake Analytics-konto med hjälp av Azure Portal samt skapa ett Data Lake Analytics-jobb med hjälp av U-SQL och skicka jobbet. "
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: b1584d16-e0d2-4019-ad1f-f04be8c5b430
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/21/2017
ms.author: edmaca
ms.openlocfilehash: 2722a2d72ed90ea0005362563ecaee30750c040a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-portal"></a><span data-ttu-id="cd783-103">Kom igång med Azure Data Lake Analytics med hjälp av Azure Portal</span><span class="sxs-lookup"><span data-stu-id="cd783-103">Get started with Azure Data Lake Analytics using Azure portal</span></span>
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

<span data-ttu-id="cd783-104">Lär dig hur du använder Azure Portal för att skapa Azure Data Lake Analytics-konton, definiera jobb i [U-SQL](data-lake-analytics-u-sql-get-started.md) och skicka jobb till Data Lake Analytics-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="cd783-104">Learn how to use the Azure portal to create Azure Data Lake Analytics accounts, define jobs in [U-SQL](data-lake-analytics-u-sql-get-started.md), and submit jobs to the Data Lake Analytics service.</span></span> <span data-ttu-id="cd783-105">Mer information om Data Lake Analytics finns i [Översikt över Azure Data Lake Analytics](data-lake-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="cd783-105">For more information about Data Lake Analytics, see [Azure Data Lake Analytics overview](data-lake-analytics-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cd783-106">Krav</span><span class="sxs-lookup"><span data-stu-id="cd783-106">Prerequisites</span></span>

<span data-ttu-id="cd783-107">Innan du börjar följa de här självstudierna måste du ha en **Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="cd783-107">Before you begin this tutorial, you must have an **Azure subscription**.</span></span> <span data-ttu-id="cd783-108">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cd783-108">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="create-a-data-lake-analytics-account"></a><span data-ttu-id="cd783-109">Skapa ett Data Lake Analytics-konto</span><span class="sxs-lookup"><span data-stu-id="cd783-109">Create a Data Lake Analytics account</span></span>

<span data-ttu-id="cd783-110">Nu ska du skapa ett Data Lake Analytics och ett Data Lake Store-konto på samma gång.</span><span class="sxs-lookup"><span data-stu-id="cd783-110">Now, you will create a Data Lake Analytics and a Data Lake Store account at the same time.</span></span>  <span data-ttu-id="cd783-111">Det här steget är enkelt och tar bara ungefär 60 sekunder att slutföra.</span><span class="sxs-lookup"><span data-stu-id="cd783-111">This step is simple and only takes about 60 seconds to finish.</span></span>

1. <span data-ttu-id="cd783-112">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="cd783-112">Sign on to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="cd783-113">Klicka på **Nytt** >  **Data och analys** > **Data Lake Analytics**.</span><span class="sxs-lookup"><span data-stu-id="cd783-113">Click **New** >  **Data + Analytics** > **Data Lake Analytics**.</span></span>
3. <span data-ttu-id="cd783-114">Välj värden för följande objekt:</span><span class="sxs-lookup"><span data-stu-id="cd783-114">Select values for the following items:</span></span>
   * <span data-ttu-id="cd783-115">**Namn**: Ange ett namn på ditt Data Lake Analytics-konto (endast gemena bokstäver och siffror tillåts).</span><span class="sxs-lookup"><span data-stu-id="cd783-115">**Name**: Name your Data Lake Analytics account (Only lower case letters and numbers allowed).</span></span>
   * <span data-ttu-id="cd783-116">**Prenumeration**: Välj den Azure-prenumeration som används för Analytics-kontot.</span><span class="sxs-lookup"><span data-stu-id="cd783-116">**Subscription**: Choose the Azure subscription used for the Analytics account.</span></span>
   * <span data-ttu-id="cd783-117">**Resursgrupp**.</span><span class="sxs-lookup"><span data-stu-id="cd783-117">**Resource Group**.</span></span> <span data-ttu-id="cd783-118">Välj en befintlig Azure-resursgrupp eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="cd783-118">Select an existing Azure Resource Group or create a new one.</span></span>
   * <span data-ttu-id="cd783-119">**Plats**.</span><span class="sxs-lookup"><span data-stu-id="cd783-119">**Location**.</span></span> <span data-ttu-id="cd783-120">Välj ett Azure-datacenter för Data Lake Analytics-kontot.</span><span class="sxs-lookup"><span data-stu-id="cd783-120">Select an Azure data center for the Data Lake Analytics account.</span></span>
   * <span data-ttu-id="cd783-121">**Data Lake Store**: Följ anvisningarna för att skapa ett nytt Data Lake Store-konto eller välj ett befintligt.</span><span class="sxs-lookup"><span data-stu-id="cd783-121">**Data Lake Store**: Follow the instruction to create a new Data Lake Store account, or select an existing one.</span></span> 
4. <span data-ttu-id="cd783-122">Alternativt,kan du välja en prisnivå för ditt Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="cd783-122">Optionally, select a pricing tier for your Data Lake Analytics account.</span></span>
5. <span data-ttu-id="cd783-123">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="cd783-123">Click **Create**.</span></span> 


## <a name="your-first-u-sql-script"></a><span data-ttu-id="cd783-124">Skriv ditt första U-SQL-skript</span><span class="sxs-lookup"><span data-stu-id="cd783-124">Your first U-SQL script</span></span>

<span data-ttu-id="cd783-125">Följande text är ett enkelt U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="cd783-125">The following text is a very simple U-SQL script.</span></span> <span data-ttu-id="cd783-126">Allt den gör är att definiera en liten datamängd i skriptet och sedan skriva datauppsättningen till standard Data Lake Store som en fil med namnet `/data.csv`.</span><span class="sxs-lookup"><span data-stu-id="cd783-126">All it does is define a small dataset within the script and then write that dataset out to the default Data Lake Store as a file called `/data.csv`.</span></span>

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
```

## <a name="submit-a-u-sql-job"></a><span data-ttu-id="cd783-127">Skicka ett U-SQL-jobb</span><span class="sxs-lookup"><span data-stu-id="cd783-127">Submit a U-SQL job</span></span>

1. <span data-ttu-id="cd783-128">Välj Data Lake Analytics-konto och klicka på **Nytt jobb**.</span><span class="sxs-lookup"><span data-stu-id="cd783-128">From the Data Lake Analytics account, click **New Job**.</span></span>
2. <span data-ttu-id="cd783-129">Klistra in texten med U-SQL-skriptet från tidigare.</span><span class="sxs-lookup"><span data-stu-id="cd783-129">Paste in the text of the U-SQL script shown above.</span></span> 
3. <span data-ttu-id="cd783-130">Klicka på **Skicka jobb**.</span><span class="sxs-lookup"><span data-stu-id="cd783-130">Click **Submit Job**.</span></span>   
4. <span data-ttu-id="cd783-131">Vänta tills jobbstatusen har ändrats till **Klar**.</span><span class="sxs-lookup"><span data-stu-id="cd783-131">Wait until the job status changes to **Succeeded**.</span></span>
5. <span data-ttu-id="cd783-132">Om jobbet misslyckades, se [Övervaka och felsöka Data Lake Analytics-jobb](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="cd783-132">If the job failed, see [Monitor and troubleshoot Data Lake Analytics jobs](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).</span></span>
6. <span data-ttu-id="cd783-133">Klicka på fliken **Utdata** och klicka sedan på `data.csv`.</span><span class="sxs-lookup"><span data-stu-id="cd783-133">Click the **Output** tab, and then click `data.csv`.</span></span> 

## <a name="see-also"></a><span data-ttu-id="cd783-134">Se även</span><span class="sxs-lookup"><span data-stu-id="cd783-134">See also</span></span>

* <span data-ttu-id="cd783-135">Information om att utveckla U-SQL-program finns i [Utveckla U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="cd783-135">To get started developing U-SQL applications, see [Develop U-SQL scripts using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span>
* <span data-ttu-id="cd783-136">Information om U-SQL finns i [Kom igång med U-SQL-språk i Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="cd783-136">To learn U-SQL, see [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md).</span></span>
* <span data-ttu-id="cd783-137">Information om hanteringsuppgifter finns i [Hantera Azure Data Lake Analytics med hjälp av Azure Portal](data-lake-analytics-manage-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="cd783-137">For management tasks, see [Manage Azure Data Lake Analytics using Azure portal](data-lake-analytics-manage-use-portal.md).</span></span>
