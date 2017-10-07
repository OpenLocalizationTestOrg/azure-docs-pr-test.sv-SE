---
title: "aaaGet igång med Azure Data Lake Analytics med hjälp av Azure PowerShell | Microsoft Docs"
description: "Använda Azure PowerShell toocreate ett Data Lake Analytics-konto, skapa ett Data Lake Analytics-jobb med hjälp av U-SQL och skicka hello-jobbet. "
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 8a4e901e-9656-4a60-90d0-d78ff2f00656
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/04/2017
ms.author: edmaca
ms.openlocfilehash: cb9b35352d1cc9a78337448b1d6835875a212e08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-powershell"></a><span data-ttu-id="af3e8-103">Kom igång med Azure Data Lake Analytics med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="af3e8-103">Get started with Azure Data Lake Analytics using Azure PowerShell</span></span>
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

<span data-ttu-id="af3e8-104">Lär dig hur toouse Azure PowerShell toocreate Azure Data Lake Analytics-konton och sedan skicka och köra U-SQL-jobb.</span><span class="sxs-lookup"><span data-stu-id="af3e8-104">Learn how toouse Azure PowerShell toocreate Azure Data Lake Analytics accounts and then submit and run U-SQL jobs.</span></span> <span data-ttu-id="af3e8-105">Mer information om Data Lake Analytics finns i [Översikt över Azure Data Lake Analytics](data-lake-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="af3e8-105">For more information about Data Lake Analytics, see [Azure Data Lake Analytics overview](data-lake-analytics-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="af3e8-106">Krav</span><span class="sxs-lookup"><span data-stu-id="af3e8-106">Prerequisites</span></span>

<span data-ttu-id="af3e8-107">Innan du påbörjar den här självstudien måste du ha hello följande information:</span><span class="sxs-lookup"><span data-stu-id="af3e8-107">Before you begin this tutorial, you must have hello following information:</span></span>

* <span data-ttu-id="af3e8-108">**Ett Azure Data Lake Analytics-konto**.</span><span class="sxs-lookup"><span data-stu-id="af3e8-108">**An Azure Data Lake Analytics account**.</span></span> <span data-ttu-id="af3e8-109">Se [Kom igång med Data Lake Analytics](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-get-started-portal).</span><span class="sxs-lookup"><span data-stu-id="af3e8-109">See [Get started with Data Lake Analytics](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-get-started-portal).</span></span>
* <span data-ttu-id="af3e8-110">**En arbetsstation med Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="af3e8-110">**A workstation with Azure PowerShell**.</span></span> <span data-ttu-id="af3e8-111">Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="af3e8-111">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <a name="log-in-tooazure"></a><span data-ttu-id="af3e8-112">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="af3e8-112">Log in tooAzure</span></span>

<span data-ttu-id="af3e8-113">Den här kursen förutsätter att du redan är bekant med Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="af3e8-113">This tutorial assumes you are already familiar with using Azure PowerShell.</span></span> <span data-ttu-id="af3e8-114">I synnerhet måste tooknow hur toolog i tooAzure.</span><span class="sxs-lookup"><span data-stu-id="af3e8-114">In particular, you need tooknow how toolog in tooAzure.</span></span> <span data-ttu-id="af3e8-115">Se hello [Kom igång med Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/get-started-azureps) om du behöver hjälp.</span><span class="sxs-lookup"><span data-stu-id="af3e8-115">See hello [Get started with Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/get-started-azureps) if you need help.</span></span>

<span data-ttu-id="af3e8-116">toolog in med ett prenumerationsnamn:</span><span class="sxs-lookup"><span data-stu-id="af3e8-116">toolog in with a subscription name:</span></span>

```
Login-AzureRmAccount -SubscriptionName "ContosoSubscription"
```

<span data-ttu-id="af3e8-117">Du kan också använda en prenumerations-id toolog i i stället för hello prenumerationsnamn:</span><span class="sxs-lookup"><span data-stu-id="af3e8-117">Instead of hello subscription name, you can also use a subscription id toolog in:</span></span>

```
Login-AzureRmAccount -SubscriptionId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
```

<span data-ttu-id="af3e8-118">Om detta lyckas hello utdata från kommandot ser ut så hello följande text:</span><span class="sxs-lookup"><span data-stu-id="af3e8-118">If  successful, hello output of this command looks like hello following text:</span></span>

```
Environment           : AzureCloud
Account               : joe@contoso.com
TenantId              : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
SubscriptionId        : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
SubscriptionName      : ContosoSubscription
CurrentStorageAccount :
```

## <a name="preparing-for-hello-tutorial"></a><span data-ttu-id="af3e8-119">Förbereda hello genomgång</span><span class="sxs-lookup"><span data-stu-id="af3e8-119">Preparing for hello tutorial</span></span>

<span data-ttu-id="af3e8-120">hello PowerShell kodavsnitt i den här kursen använder dessa variabler toostore informationen:</span><span class="sxs-lookup"><span data-stu-id="af3e8-120">hello PowerShell snippets in this tutorial use these variables toostore this information:</span></span>

```
$rg = "<ResourceGroupName>"
$adls = "<DataLakeStoreAccountName>"
$adla = "<DataLakeAnalyticsAccountName>"
$location = "East US 2"
```

## <a name="get-information-about-a-data-lake-analytics-account"></a><span data-ttu-id="af3e8-121">Hämta information om ett Data Lake Analytics-konto</span><span class="sxs-lookup"><span data-stu-id="af3e8-121">Get information about a Data Lake Analytics account</span></span>

```
Get-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla  
```

## <a name="submit-a-u-sql-job"></a><span data-ttu-id="af3e8-122">Skicka ett U-SQL-jobb</span><span class="sxs-lookup"><span data-stu-id="af3e8-122">Submit a U-SQL job</span></span>

<span data-ttu-id="af3e8-123">Skapa ett PowerShell variabeln toohold hello U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="af3e8-123">Create a PowerShell variable toohold hello U-SQL script.</span></span>

```
$script = @"
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

"@
```

<span data-ttu-id="af3e8-124">Skicka hello skript.</span><span class="sxs-lookup"><span data-stu-id="af3e8-124">Submit hello script.</span></span>

```
$job = Submit-AdlJob -AccountName $adla –Script $script
```

<span data-ttu-id="af3e8-125">Du kan också spara hello skript som en fil och skicka med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="af3e8-125">Alternatively, you could save hello script as a file and submit with hello following command:</span></span>

```
$filename = "d:\test.usql"
$script | out-File $filename
$job = Submit-AdlJob -AccountName $adla –ScriptPath $filename
```


<span data-ttu-id="af3e8-126">Hämta hello status för ett visst jobb.</span><span class="sxs-lookup"><span data-stu-id="af3e8-126">Get hello status of a specific job.</span></span> <span data-ttu-id="af3e8-127">Fortsätta att använda denna cmdlet tills du ser hello jobbet är klart.</span><span class="sxs-lookup"><span data-stu-id="af3e8-127">Keep using this cmdlet until you see hello job is done.</span></span>

```
$job = Get-AdlJob -AccountName $adla -JobId $job.JobId
```

<span data-ttu-id="af3e8-128">Du kan använda hello vänta AdlJob cmdlet i stället för att anropa Get-AdlAnalyticsJob flera gånger tills ett jobb har slutförts.</span><span class="sxs-lookup"><span data-stu-id="af3e8-128">Instead of calling Get-AdlAnalyticsJob over and over until a job finishes, you can use hello Wait-AdlJob cmdlet.</span></span>

```
Wait-AdlJob -Account $adla -JobId $job.JobId
```

<span data-ttu-id="af3e8-129">Hämta hello utdatafilen.</span><span class="sxs-lookup"><span data-stu-id="af3e8-129">Download hello output file.</span></span>

```
Export-AdlStoreItem -AccountName $adls -Path "/data.csv" -Destination "C:\data.csv"
```

## <a name="see-also"></a><span data-ttu-id="af3e8-130">Se även</span><span class="sxs-lookup"><span data-stu-id="af3e8-130">See also</span></span>
* <span data-ttu-id="af3e8-131">toosee hello samma självstudier med andra verktyg klickar du på hello flikväljarna överst hello hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="af3e8-131">toosee hello same tutorial using other tools, click hello tab selectors on hello top of hello page.</span></span>
* <span data-ttu-id="af3e8-132">toolearn U-SQL finns [Kom igång med Azure Data Lake Analytics U-SQL-språket](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="af3e8-132">toolearn U-SQL, see [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md).</span></span>
* <span data-ttu-id="af3e8-133">Information om hanteringsuppgifter finns i [Hantera Azure Data Lake Analytics med hjälp av Azure Portal](data-lake-analytics-manage-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="af3e8-133">For management tasks, see [Manage Azure Data Lake Analytics using Azure portal](data-lake-analytics-manage-use-portal.md).</span></span>
