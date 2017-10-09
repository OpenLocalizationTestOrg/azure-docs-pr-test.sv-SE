---
title: "aaaGet igång med Azure Data Lake Analytics med hjälp av Azure portal | Microsoft Docs"
description: "Lär dig hur toouse hello Azure portal toocreate ett Data Lake Analytics-konto, skapa ett Data Lake Analytics-jobb med hjälp av U-SQL och skicka hello-jobbet. "
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
ms.openlocfilehash: 6bb54404fa42cfed25b18bc2bfb7c72e6c361149
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-portal"></a><span data-ttu-id="2c007-103">Kom igång med Azure Data Lake Analytics med hjälp av Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2c007-103">Get started with Azure Data Lake Analytics using Azure portal</span></span>
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

<span data-ttu-id="2c007-104">Lär dig hur toouse hello Azure portal toocreate Azure Data Lake Analytics-konton, definiera jobb i [U-SQL](data-lake-analytics-u-sql-get-started.md), och skicka jobb toohello Data Lake Analytics-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="2c007-104">Learn how toouse hello Azure portal toocreate Azure Data Lake Analytics accounts, define jobs in [U-SQL](data-lake-analytics-u-sql-get-started.md), and submit jobs toohello Data Lake Analytics service.</span></span> <span data-ttu-id="2c007-105">Mer information om Data Lake Analytics finns i [Översikt över Azure Data Lake Analytics](data-lake-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2c007-105">For more information about Data Lake Analytics, see [Azure Data Lake Analytics overview](data-lake-analytics-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2c007-106">Krav</span><span class="sxs-lookup"><span data-stu-id="2c007-106">Prerequisites</span></span>

<span data-ttu-id="2c007-107">Innan du börjar följa de här självstudierna måste du ha en **Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="2c007-107">Before you begin this tutorial, you must have an **Azure subscription**.</span></span> <span data-ttu-id="2c007-108">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2c007-108">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="create-a-data-lake-analytics-account"></a><span data-ttu-id="2c007-109">Skapa ett Data Lake Analytics-konto</span><span class="sxs-lookup"><span data-stu-id="2c007-109">Create a Data Lake Analytics account</span></span>

<span data-ttu-id="2c007-110">Nu ska du skapa ett Data Lake Analytics och ett Data Lake Store-konto på hello samma tid.</span><span class="sxs-lookup"><span data-stu-id="2c007-110">Now, you will create a Data Lake Analytics and a Data Lake Store account at hello same time.</span></span>  <span data-ttu-id="2c007-111">Det här steget är enkel och tar bara om toofinish 60 sekunder.</span><span class="sxs-lookup"><span data-stu-id="2c007-111">This step is simple and only takes about 60 seconds toofinish.</span></span>

1. <span data-ttu-id="2c007-112">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2c007-112">Sign on toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="2c007-113">Klicka på **Nytt** >  **Data och analys** > **Data Lake Analytics**.</span><span class="sxs-lookup"><span data-stu-id="2c007-113">Click **New** >  **Data + Analytics** > **Data Lake Analytics**.</span></span>
3. <span data-ttu-id="2c007-114">Välj värden för hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="2c007-114">Select values for hello following items:</span></span>
   * <span data-ttu-id="2c007-115">**Namn**: Ange ett namn på ditt Data Lake Analytics-konto (endast gemena bokstäver och siffror tillåts).</span><span class="sxs-lookup"><span data-stu-id="2c007-115">**Name**: Name your Data Lake Analytics account (Only lower case letters and numbers allowed).</span></span>
   * <span data-ttu-id="2c007-116">**Prenumerationen**: Välj hello Azure-prenumeration används för hello Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="2c007-116">**Subscription**: Choose hello Azure subscription used for hello Analytics account.</span></span>
   * <span data-ttu-id="2c007-117">**Resursgrupp**.</span><span class="sxs-lookup"><span data-stu-id="2c007-117">**Resource Group**.</span></span> <span data-ttu-id="2c007-118">Välj en befintlig Azure-resursgrupp eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="2c007-118">Select an existing Azure Resource Group or create a new one.</span></span>
   * <span data-ttu-id="2c007-119">**Plats**.</span><span class="sxs-lookup"><span data-stu-id="2c007-119">**Location**.</span></span> <span data-ttu-id="2c007-120">Välj en Azure-Datacenter för hello Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="2c007-120">Select an Azure data center for hello Data Lake Analytics account.</span></span>
   * <span data-ttu-id="2c007-121">**Data Lake Store**: följa hello instruktion toocreate ett nytt Data Lake Store-konto eller välj en befintlig.</span><span class="sxs-lookup"><span data-stu-id="2c007-121">**Data Lake Store**: Follow hello instruction toocreate a new Data Lake Store account, or select an existing one.</span></span> 
4. <span data-ttu-id="2c007-122">Alternativt,kan du välja en prisnivå för ditt Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="2c007-122">Optionally, select a pricing tier for your Data Lake Analytics account.</span></span>
5. <span data-ttu-id="2c007-123">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="2c007-123">Click **Create**.</span></span> 


## <a name="your-first-u-sql-script"></a><span data-ttu-id="2c007-124">Skriv ditt första U-SQL-skript</span><span class="sxs-lookup"><span data-stu-id="2c007-124">Your first U-SQL script</span></span>

<span data-ttu-id="2c007-125">hello följande text är ett väldigt enkelt U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="2c007-125">hello following text is a very simple U-SQL script.</span></span> <span data-ttu-id="2c007-126">Allt den gör är att definiera en liten datamängd i hello skript och sedan skriva den dataset ut toohello standard Data Lake Store som en fil med namnet `/data.csv`.</span><span class="sxs-lookup"><span data-stu-id="2c007-126">All it does is define a small dataset within hello script and then write that dataset out toohello default Data Lake Store as a file called `/data.csv`.</span></span>

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    too"/data.csv"
    USING Outputters.Csv();
```

## <a name="submit-a-u-sql-job"></a><span data-ttu-id="2c007-127">Skicka ett U-SQL-jobb</span><span class="sxs-lookup"><span data-stu-id="2c007-127">Submit a U-SQL job</span></span>

1. <span data-ttu-id="2c007-128">Hello Data Lake Analytics-konto, klickar du på **nytt jobb**.</span><span class="sxs-lookup"><span data-stu-id="2c007-128">From hello Data Lake Analytics account, click **New Job**.</span></span>
2. <span data-ttu-id="2c007-129">Klistra in i hello texten i hello U-SQL-skript som visas ovan.</span><span class="sxs-lookup"><span data-stu-id="2c007-129">Paste in hello text of hello U-SQL script shown above.</span></span> 
3. <span data-ttu-id="2c007-130">Klicka på **Skicka jobb**.</span><span class="sxs-lookup"><span data-stu-id="2c007-130">Click **Submit Job**.</span></span>   
4. <span data-ttu-id="2c007-131">Vänta tills hello ändringar av jobbstatus för**lyckades**.</span><span class="sxs-lookup"><span data-stu-id="2c007-131">Wait until hello job status changes too**Succeeded**.</span></span>
5. <span data-ttu-id="2c007-132">Om hello-jobbet misslyckades, se [övervaka och felsöka Data Lake Analytics-jobb](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="2c007-132">If hello job failed, see [Monitor and troubleshoot Data Lake Analytics jobs](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).</span></span>
6. <span data-ttu-id="2c007-133">Klicka på hello **utdata** fliken och klicka sedan på `data.csv`.</span><span class="sxs-lookup"><span data-stu-id="2c007-133">Click hello **Output** tab, and then click `data.csv`.</span></span> 

## <a name="see-also"></a><span data-ttu-id="2c007-134">Se även</span><span class="sxs-lookup"><span data-stu-id="2c007-134">See also</span></span>

* <span data-ttu-id="2c007-135">tooget utveckla U-SQL-program, se [utveckla U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="2c007-135">tooget started developing U-SQL applications, see [Develop U-SQL scripts using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span>
* <span data-ttu-id="2c007-136">toolearn U-SQL finns [Kom igång med Azure Data Lake Analytics U-SQL-språket](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="2c007-136">toolearn U-SQL, see [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md).</span></span>
* <span data-ttu-id="2c007-137">Information om hanteringsuppgifter finns i [Hantera Azure Data Lake Analytics med hjälp av Azure Portal](data-lake-analytics-manage-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="2c007-137">For management tasks, see [Manage Azure Data Lake Analytics using Azure portal](data-lake-analytics-manage-use-portal.md).</span></span>
