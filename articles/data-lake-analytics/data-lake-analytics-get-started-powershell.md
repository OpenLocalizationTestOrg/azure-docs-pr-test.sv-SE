---
title: "Kom igång med Azure Data Lake Analytics med hjälp av Azure PowerShell | Microsoft Docs"
description: "Skapa ett Data Lake Analytics-konto med hjälp av Azure PowerShell, hur du skapar ett Data Lake Analytics-jobb med hjälp av U-SQL och hur du skickar jobbet. "
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
ms.openlocfilehash: 4f73e27c733edae658d1ea3bdabe48076328279b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-powershell"></a><span data-ttu-id="cf98c-103">Kom igång med Azure Data Lake Analytics med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="cf98c-103">Get started with Azure Data Lake Analytics using Azure PowerShell</span></span>
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

<span data-ttu-id="cf98c-104">Lär dig hur du använder Azure PowerShell för att skapa Azure Data Lake Analytics-konton och sedan skicka och köra U-SQL-jobb.</span><span class="sxs-lookup"><span data-stu-id="cf98c-104">Learn how to use Azure PowerShell to create Azure Data Lake Analytics accounts and then submit and run U-SQL jobs.</span></span> <span data-ttu-id="cf98c-105">Mer information om Data Lake Analytics finns i [Översikt över Azure Data Lake Analytics](data-lake-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="cf98c-105">For more information about Data Lake Analytics, see [Azure Data Lake Analytics overview](data-lake-analytics-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cf98c-106">Krav</span><span class="sxs-lookup"><span data-stu-id="cf98c-106">Prerequisites</span></span>

<span data-ttu-id="cf98c-107">Innan du börjar den här självstudiekursen behöver du följande information:</span><span class="sxs-lookup"><span data-stu-id="cf98c-107">Before you begin this tutorial, you must have the following information:</span></span>

* <span data-ttu-id="cf98c-108">**Ett Azure Data Lake Analytics-konto**.</span><span class="sxs-lookup"><span data-stu-id="cf98c-108">**An Azure Data Lake Analytics account**.</span></span> <span data-ttu-id="cf98c-109">Se [Kom igång med Data Lake Analytics](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-get-started-portal).</span><span class="sxs-lookup"><span data-stu-id="cf98c-109">See [Get started with Data Lake Analytics](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-get-started-portal).</span></span>
* <span data-ttu-id="cf98c-110">**En arbetsstation med Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="cf98c-110">**A workstation with Azure PowerShell**.</span></span> <span data-ttu-id="cf98c-111">Se [Så här installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="cf98c-111">See [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <a name="log-in-to-azure"></a><span data-ttu-id="cf98c-112">Logga in på Azure</span><span class="sxs-lookup"><span data-stu-id="cf98c-112">Log in to Azure</span></span>

<span data-ttu-id="cf98c-113">Den här kursen förutsätter att du redan är bekant med Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cf98c-113">This tutorial assumes you are already familiar with using Azure PowerShell.</span></span> <span data-ttu-id="cf98c-114">Framför allt behöver du veta hur du loggar in på Azure.</span><span class="sxs-lookup"><span data-stu-id="cf98c-114">In particular, you need to know how to log in to Azure.</span></span> <span data-ttu-id="cf98c-115">Läs [Kom igång med Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/get-started-azureps) om du behöver hjälp.</span><span class="sxs-lookup"><span data-stu-id="cf98c-115">See the [Get started with Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/get-started-azureps) if you need help.</span></span>

<span data-ttu-id="cf98c-116">Logga in med ett prenumerationsnamn:</span><span class="sxs-lookup"><span data-stu-id="cf98c-116">To log in with a subscription name:</span></span>

```
Login-AzureRmAccount -SubscriptionName "ContosoSubscription"
```

<span data-ttu-id="cf98c-117">Du kan också använda ett prenumerations-id för att logga in i stället för prenumerationsnamnet:</span><span class="sxs-lookup"><span data-stu-id="cf98c-117">Instead of the subscription name, you can also use a subscription id to log in:</span></span>

```
Login-AzureRmAccount -SubscriptionId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
```

<span data-ttu-id="cf98c-118">Om detta lyckas ser kommandots utdata ut som följande text:</span><span class="sxs-lookup"><span data-stu-id="cf98c-118">If  successful, the output of this command looks like the following text:</span></span>

```
Environment           : AzureCloud
Account               : joe@contoso.com
TenantId              : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
SubscriptionId        : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
SubscriptionName      : ContosoSubscription
CurrentStorageAccount :
```

## <a name="preparing-for-the-tutorial"></a><span data-ttu-id="cf98c-119">Förbereda för självstudier</span><span class="sxs-lookup"><span data-stu-id="cf98c-119">Preparing for the tutorial</span></span>

<span data-ttu-id="cf98c-120">PowerShell-kodfragmenten i den här självstudien använder dessa variabler för att lagra informationen:</span><span class="sxs-lookup"><span data-stu-id="cf98c-120">The PowerShell snippets in this tutorial use these variables to store this information:</span></span>

```
$rg = "<ResourceGroupName>"
$adls = "<DataLakeStoreAccountName>"
$adla = "<DataLakeAnalyticsAccountName>"
$location = "East US 2"
```

## <a name="get-information-about-a-data-lake-analytics-account"></a><span data-ttu-id="cf98c-121">Hämta information om ett Data Lake Analytics-konto</span><span class="sxs-lookup"><span data-stu-id="cf98c-121">Get information about a Data Lake Analytics account</span></span>

```
Get-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla  
```

## <a name="submit-a-u-sql-job"></a><span data-ttu-id="cf98c-122">Skicka ett U-SQL-jobb</span><span class="sxs-lookup"><span data-stu-id="cf98c-122">Submit a U-SQL job</span></span>

<span data-ttu-id="cf98c-123">Skapa en PowerShell-variabel för att lagra U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="cf98c-123">Create a PowerShell variable to hold the U-SQL script.</span></span>

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
    TO "/data.csv"
    USING Outputters.Csv();

"@
```

<span data-ttu-id="cf98c-124">Skicka jobbet.</span><span class="sxs-lookup"><span data-stu-id="cf98c-124">Submit the script.</span></span>

```
$job = Submit-AdlJob -AccountName $adla –Script $script
```

<span data-ttu-id="cf98c-125">Du kan också spara skriptet som en fil och skicka med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cf98c-125">Alternatively, you could save the script as a file and submit with the following command:</span></span>

```
$filename = "d:\test.usql"
$script | out-File $filename
$job = Submit-AdlJob -AccountName $adla –ScriptPath $filename
```


<span data-ttu-id="cf98c-126">Hämta status för ett specifikt jobb.</span><span class="sxs-lookup"><span data-stu-id="cf98c-126">Get the status of a specific job.</span></span> <span data-ttu-id="cf98c-127">Fortsätta att använda denna cmdlet tills jobbet är klart.</span><span class="sxs-lookup"><span data-stu-id="cf98c-127">Keep using this cmdlet until you see the job is done.</span></span>

```
$job = Get-AdlJob -AccountName $adla -JobId $job.JobId
```

<span data-ttu-id="cf98c-128">Istället för att anropa Get-AdlAnalyticsJob om och om igen tills ett jobb blir klart kan du använda cmdleten Wait-AdlJob.</span><span class="sxs-lookup"><span data-stu-id="cf98c-128">Instead of calling Get-AdlAnalyticsJob over and over until a job finishes, you can use the Wait-AdlJob cmdlet.</span></span>

```
Wait-AdlJob -Account $adla -JobId $job.JobId
```

<span data-ttu-id="cf98c-129">Hämta utdatafilen.</span><span class="sxs-lookup"><span data-stu-id="cf98c-129">Download the output file.</span></span>

```
Export-AdlStoreItem -AccountName $adls -Path "/data.csv" -Destination "C:\data.csv"
```

## <a name="see-also"></a><span data-ttu-id="cf98c-130">Se även</span><span class="sxs-lookup"><span data-stu-id="cf98c-130">See also</span></span>
* <span data-ttu-id="cf98c-131">Klicka på flikväljarna överst på sidan om du vill se samma självstudier med andra verktyg.</span><span class="sxs-lookup"><span data-stu-id="cf98c-131">To see the same tutorial using other tools, click the tab selectors on the top of the page.</span></span>
* <span data-ttu-id="cf98c-132">Information om U-SQL finns i [Kom igång med U-SQL-språk i Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="cf98c-132">To learn U-SQL, see [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md).</span></span>
* <span data-ttu-id="cf98c-133">Information om hanteringsuppgifter finns i [Hantera Azure Data Lake Analytics med hjälp av Azure Portal](data-lake-analytics-manage-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="cf98c-133">For management tasks, see [Manage Azure Data Lake Analytics using Azure portal](data-lake-analytics-manage-use-portal.md).</span></span>
