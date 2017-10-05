---
title: "Hantera Azure Data Lake Analytics med hjälp av Azure PowerShell | Microsoft Docs"
description: "Lär dig hur du hanterar Data Lake Analytics-konton, datakällor, jobb och katalogobjekt. "
services: data-lake-analytics
documentationcenter: 
author: matt1883
manager: jhubbard
editor: cgronlun
ms.assetid: ad14d53c-fed4-478d-ab4b-6d2e14ff2097
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/23/2017
ms.author: mahi
ms.openlocfilehash: 862e9551f1e129b7bba06651fbae94e337c92dcb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="manage-azure-data-lake-analytics-using-azure-powershell"></a><span data-ttu-id="08b5d-103">Hantera Azure Data Lake Analytics med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="08b5d-103">Manage Azure Data Lake Analytics using Azure PowerShell</span></span>
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

<span data-ttu-id="08b5d-104">Lär dig mer om att hantera Azure Data Lake Analytics-konton, datakällor, jobb och katalogobjekt med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="08b5d-104">Learn how to manage Azure Data Lake Analytics accounts, data sources, jobs, and catalog items using Azure PowerShell.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="08b5d-105">Krav</span><span class="sxs-lookup"><span data-stu-id="08b5d-105">Prerequisites</span></span>

<span data-ttu-id="08b5d-106">När du skapar ett Data Lake Analytics-konto som du behöver veta:</span><span class="sxs-lookup"><span data-stu-id="08b5d-106">When creating a Data Lake Analytics account, you need to know:</span></span>

* <span data-ttu-id="08b5d-107">**Prenumerations-ID**: Azure prenumerations-ID som Data Lake Analytics-kontot finns.</span><span class="sxs-lookup"><span data-stu-id="08b5d-107">**Subscription ID**: The Azure subscription ID under which your Data Lake Analytics account resides.</span></span>
* <span data-ttu-id="08b5d-108">**Resursgruppen**: namnet på Azure-resursgrupp som innehåller ditt Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="08b5d-108">**Resource group**: The name of the Azure resource group that contains your Data Lake Analytics account.</span></span>
* <span data-ttu-id="08b5d-109">**Data Lake Analytics-kontonamn**: kontonamnet får bara innehålla gemena bokstäver och siffror.</span><span class="sxs-lookup"><span data-stu-id="08b5d-109">**Data Lake Analytics account name**: The account name must only contain lowercase letters and numbers.</span></span>
* <span data-ttu-id="08b5d-110">**Data Lake Store-standardkonto**: Varje Data Lake Analytics-konto har ett Data Lake Store-standardkonto.</span><span class="sxs-lookup"><span data-stu-id="08b5d-110">**Default Data Lake Store account**: Each Data Lake Analytics account has a default Data Lake Store account.</span></span> <span data-ttu-id="08b5d-111">Dessa konton måste vara på samma plats.</span><span class="sxs-lookup"><span data-stu-id="08b5d-111">These accounts must be in the same location.</span></span>
* <span data-ttu-id="08b5d-112">**Plats**: platsen för ditt Data Lake Analytics-konto, till exempel ”USA, östra 2” eller andra platser som stöds.</span><span class="sxs-lookup"><span data-stu-id="08b5d-112">**Location**: The location of your Data Lake Analytics account, such as "East US 2" or other supported locations.</span></span> <span data-ttu-id="08b5d-113">Platser som stöds kan visas på vår [sida med priser](https://azure.microsoft.com/pricing/details/data-lake-analytics/).</span><span class="sxs-lookup"><span data-stu-id="08b5d-113">Supported locations can be seen on our [pricing page](https://azure.microsoft.com/pricing/details/data-lake-analytics/).</span></span>

<span data-ttu-id="08b5d-114">PowerShell-kodfragmenten i den här självstudien använder dessa variabler för att lagra informationen</span><span class="sxs-lookup"><span data-stu-id="08b5d-114">The PowerShell snippets in this tutorial use these variables to store this information</span></span>

```powershell
$subId = "<SubscriptionId>"
$rg = "<ResourceGroupName>"
$adla = "<DataLakeAnalyticsAccountName>"
$adls = "<DataLakeStoreAccountName>"
$location = "<Location>"
```

## <a name="log-in"></a><span data-ttu-id="08b5d-115">Logga in</span><span class="sxs-lookup"><span data-stu-id="08b5d-115">Log in</span></span>

<span data-ttu-id="08b5d-116">Logga in med ett prenumerations-id.</span><span class="sxs-lookup"><span data-stu-id="08b5d-116">Log in using a subscription id.</span></span>

```powershell
Login-AzureRmAccount -SubscriptionId $subId
```

<span data-ttu-id="08b5d-117">Logga in med ett prenumerationsnamn.</span><span class="sxs-lookup"><span data-stu-id="08b5d-117">Log in using a subscription name.</span></span>

```
Login-AzureRmAccount -SubscriptionName $subname 
```

<span data-ttu-id="08b5d-118">Den `Login-AzureRmAccount` cmdlet efterfrågar alltid autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="08b5d-118">The `Login-AzureRmAccount` cmdlet  always prompts for credentials.</span></span> <span data-ttu-id="08b5d-119">Du kan undvika uppmanas med hjälp av följande cmdlets:</span><span class="sxs-lookup"><span data-stu-id="08b5d-119">You can avoid being prompted by using the following cmdlets:</span></span>

```powershell
# Save login session information
Save-AzureRmProfile -Path D:\profile.json  

# Load login session information
Select-AzureRmProfile -Path D:\profile.json 
```

## <a name="managing-accounts"></a><span data-ttu-id="08b5d-120">Hantera konton</span><span class="sxs-lookup"><span data-stu-id="08b5d-120">Managing accounts</span></span>

### <a name="create-a-data-lake-analytics-account"></a><span data-ttu-id="08b5d-121">Skapa ett Data Lake Analytics-konto</span><span class="sxs-lookup"><span data-stu-id="08b5d-121">Create a Data Lake Analytics account</span></span>

<span data-ttu-id="08b5d-122">Om du inte redan har en [resursgruppen](../azure-resource-manager/resource-group-overview.md#resource-groups) om du vill använda, skapa en.</span><span class="sxs-lookup"><span data-stu-id="08b5d-122">If you don't already have a [resource group](../azure-resource-manager/resource-group-overview.md#resource-groups) to use, create one.</span></span> 

```powershell
New-AzureRmResourceGroup -Name  $rg -Location $location
```

<span data-ttu-id="08b5d-123">Alla Data Lake Analytics-konton kräver ett Data Lake Store-standardkonto som används för att lagra loggar.</span><span class="sxs-lookup"><span data-stu-id="08b5d-123">Every Data Lake Analytics account requires a default Data Lake Store account that it uses for storing logs.</span></span> <span data-ttu-id="08b5d-124">Du kan återanvända ett befintligt konto eller skapa ett konto.</span><span class="sxs-lookup"><span data-stu-id="08b5d-124">You can reuse an existing account or create an account.</span></span> 

```powershell
New-AdlStore -ResourceGroupName $rg -Name $adls -Location $location
```

<span data-ttu-id="08b5d-125">När en resursgrupp och ett Data Lake Store-konto är tillgängligt skapar du ett Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="08b5d-125">Once a Resource Group and Data Lake Store account is available, create a Data Lake Analytics account.</span></span>

```powershell
New-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla -Location $location -DefaultDataLake $adls
```

### <a name="get-information-about-an-account"></a><span data-ttu-id="08b5d-126">Hämta information om ett konto</span><span class="sxs-lookup"><span data-stu-id="08b5d-126">Get information about an account</span></span>

<span data-ttu-id="08b5d-127">Få information om ett konto.</span><span class="sxs-lookup"><span data-stu-id="08b5d-127">Get details about an account.</span></span>

```powershell
Get-AdlAnalyticsAccount -Name $adla
```

<span data-ttu-id="08b5d-128">Kontrollera om finns ett visst Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="08b5d-128">Check the existence of a specific Data Lake Analytics account.</span></span> <span data-ttu-id="08b5d-129">Cmdleten returnerar antingen `True` eller `False`.</span><span class="sxs-lookup"><span data-stu-id="08b5d-129">The cmdlet returns either `True` or `False`.</span></span>

```powershell
Test-AdlAnalyticsAccount -Name $adla
```

<span data-ttu-id="08b5d-130">Kontrollera om finns ett visst Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="08b5d-130">Check the existence of a specific Data Lake Store account.</span></span> <span data-ttu-id="08b5d-131">Cmdleten returnerar antingen `True` eller `False`.</span><span class="sxs-lookup"><span data-stu-id="08b5d-131">The cmdlet returns either `True` or `False`.</span></span>

```powershell
Test-AdlStoreAccount -Name $adls
```

### <a name="listing-accounts"></a><span data-ttu-id="08b5d-132">Visar en lista över konton</span><span class="sxs-lookup"><span data-stu-id="08b5d-132">Listing accounts</span></span>

<span data-ttu-id="08b5d-133">Lista över Data Lake Analytics-konton i den aktuella prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="08b5d-133">List Data Lake Analytics accounts within the current subscription.</span></span>

```powershell
Get-AdlAnalyticsAccount
```

<span data-ttu-id="08b5d-134">Lista över Data Lake Analytics-konton inom en viss resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="08b5d-134">List Data Lake Analytics accounts within a specific resource group.</span></span>

```powershell
Get-AdlAnalyticsAccount -ResourceGroupName $rg
```

## <a name="managing-firewall-rules"></a><span data-ttu-id="08b5d-135">Hantera brandväggsregler</span><span class="sxs-lookup"><span data-stu-id="08b5d-135">Managing firewall rules</span></span>

<span data-ttu-id="08b5d-136">Visa en lista med brandväggsregler.</span><span class="sxs-lookup"><span data-stu-id="08b5d-136">List firewall rules.</span></span>

```powershell
Get-AdlAnalyticsFirewallRule -Account $adla
```

<span data-ttu-id="08b5d-137">Lägg till en brandväggsregel.</span><span class="sxs-lookup"><span data-stu-id="08b5d-137">Add a firewall rule.</span></span>

```powershell
$ruleName = "Allow access from on-prem server"
$startIpAddress = "<start IP address>"
$endIpAddress = "<end IP address>"

Add-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName -StartIpAddress $startIpAddress -EndIpAddress $endIpAddress
```

<span data-ttu-id="08b5d-138">Ändra en brandväggsregel.</span><span class="sxs-lookup"><span data-stu-id="08b5d-138">Change a firewall rule.</span></span>

```powershell
Set-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName -StartIpAddress $startIpAddress -EndIpAddress $endIpAddress
```

<span data-ttu-id="08b5d-139">Ta bort en brandväggsregel.</span><span class="sxs-lookup"><span data-stu-id="08b5d-139">Remove a firewall rule.</span></span>

```powershell
Remove-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName
```



<span data-ttu-id="08b5d-140">Tillåt Azure IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="08b5d-140">Allow Azure IP addresses.</span></span>

```powershell
Set-AdlAnalyticsAccount -Name $adla -AllowAzureIpState Enabled
```

```powershell
Set-AdlAnalyticsAccount -Name $adla -FirewallState Enabled
Set-AdlAnalyticsAccount -Name $adla -FirewallState Disabled
```

## <a name="managing-data-sources"></a><span data-ttu-id="08b5d-141">Hantera datakällor</span><span class="sxs-lookup"><span data-stu-id="08b5d-141">Managing data sources</span></span>
<span data-ttu-id="08b5d-142">Azure Data Lake Analytics stöder för närvarande följande datakällor:</span><span class="sxs-lookup"><span data-stu-id="08b5d-142">Azure Data Lake Analytics currently supports the following data sources:</span></span>

* [<span data-ttu-id="08b5d-143">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="08b5d-143">Azure Data Lake Store</span></span>](../data-lake-store/data-lake-store-overview.md)
* [<span data-ttu-id="08b5d-144">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="08b5d-144">Azure Storage</span></span>](../storage/common/storage-introduction.md)

<span data-ttu-id="08b5d-145">När du skapar ett Analytics-konto, måste du ange ett Data Lake Store-konto för att vara standarddatakälla.</span><span class="sxs-lookup"><span data-stu-id="08b5d-145">When you create an Analytics account, you must designate a Data Lake Store account to be the default data source.</span></span> <span data-ttu-id="08b5d-146">Data Lake Store-standardkontot används för att spara jobbet metadata och jobbet granskningsloggar.</span><span class="sxs-lookup"><span data-stu-id="08b5d-146">The default Data Lake Store account is used to store job metadata and job audit logs.</span></span> <span data-ttu-id="08b5d-147">När du har skapat ett Data Lake Analytics-konto kan du lägga till ytterligare Data Lake Store-konton och/eller Storage-konton.</span><span class="sxs-lookup"><span data-stu-id="08b5d-147">After you have created a Data Lake Analytics account, you can add additional Data Lake Store accounts and/or Storage accounts.</span></span> 

### <a name="find-the-default-data-lake-store-account"></a><span data-ttu-id="08b5d-148">Hitta Data Lake Store-standardkontot</span><span class="sxs-lookup"><span data-stu-id="08b5d-148">Find the default Data Lake Store account</span></span>

```powershell
$adla_acct = Get-AdlAnalyticsAccount -Name $adla
$dataLakeStoreName = $adla_acct.DefaultDataLakeAccount
```

<span data-ttu-id="08b5d-149">Du kan hitta Data Lake Store-standardkontot genom att filtrera listan över datakällor genom den `IsDefault` egenskapen:</span><span class="sxs-lookup"><span data-stu-id="08b5d-149">You can find the default Data Lake Store account by filtering the list of datasources by the `IsDefault` property:</span></span>

```powershell
Get-AdlAnalyticsDataSource -Account $adla  | ? { $_.IsDefault } 
```

### <a name="add-a-data-source"></a><span data-ttu-id="08b5d-150">Lägga till en datakälla</span><span class="sxs-lookup"><span data-stu-id="08b5d-150">Add a data source</span></span>

```powershell

# Add an additional Storage (Blob) account.
$AzureStorageAccountName = "<AzureStorageAccountName>"
$AzureStorageAccountKey = "<AzureStorageAccountKey>"
Add-AdlAnalyticsDataSource -Account $adla -Blob $AzureStorageAccountName -AccessKey $AzureStorageAccountKey

# Add an additional Data Lake Store account.
$AzureDataLakeStoreName = "<AzureDataLakeStoreAccountName"
Add-AdlAnalyticsDataSource -Account $adla -DataLakeStore $AzureDataLakeStoreName 
```

### <a name="list-data-sources"></a><span data-ttu-id="08b5d-151">Lista över datakällor</span><span class="sxs-lookup"><span data-stu-id="08b5d-151">List data sources</span></span>

```powershell
# List all the data sources
Get-AdlAnalyticsDataSource -Name $adla

# List attached Data Lake Store accounts
Get-AdlAnalyticsDataSource -Name $adla | where -Property Type -EQ "DataLakeStore"

# List attached Storage accounts
Get-AdlAnalyticsDataSource -Name $adla | where -Property Type -EQ "Blob"
```

## <a name="submit-u-sql-jobs"></a><span data-ttu-id="08b5d-152">Skicka U-SQL-jobb</span><span class="sxs-lookup"><span data-stu-id="08b5d-152">Submit U-SQL jobs</span></span>

### <a name="submit-a-string-as-a-u-sql-script"></a><span data-ttu-id="08b5d-153">Skicka en sträng som ett U-SQL-skript</span><span class="sxs-lookup"><span data-stu-id="08b5d-153">Submit a string as a U-SQL script</span></span>

```powershell
$script = @"
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
"@

$scriptpath = "d:\test.usql"
$script | Out-File $scriptpath 

Submit-AdlJob -AccountName $adla -Script $script -Name "Demo"
```


### <a name="submit-a-file-as-a-u-sql-script"></a><span data-ttu-id="08b5d-154">Skicka en fil som ett U-SQL-skript</span><span class="sxs-lookup"><span data-stu-id="08b5d-154">Submit a file as a U-SQL script</span></span>

```powershell
$scriptpath = "d:\test.usql"
$script | Out-File $scriptpath 
Submit-AdlJob -AccountName $adla –ScriptPath $scriptpath -Name "Demo"
```

## <a name="list-jobs-in-an-account"></a><span data-ttu-id="08b5d-155">Lista över jobb i ett konto</span><span class="sxs-lookup"><span data-stu-id="08b5d-155">List jobs in an account</span></span>

### <a name="list-all-the-jobs-in-the-account"></a><span data-ttu-id="08b5d-156">Räkna upp alla jobb för kontot.</span><span class="sxs-lookup"><span data-stu-id="08b5d-156">List all the jobs in the account.</span></span> 

<span data-ttu-id="08b5d-157">Resultatet innehåller de jobb som körs för närvarande och de jobb som nyss blev klara.</span><span class="sxs-lookup"><span data-stu-id="08b5d-157">The output includes the currently running jobs and those jobs that have recently completed.</span></span>

```powershell
Get-AdlJob -Account $adla
```


### <a name="list-a-specific-number-of-jobs"></a><span data-ttu-id="08b5d-158">Visa en lista med ett visst antal jobb</span><span class="sxs-lookup"><span data-stu-id="08b5d-158">List a specific number of jobs</span></span>

<span data-ttu-id="08b5d-159">Som standard för jobben i listan sorteras i överföringstid.</span><span class="sxs-lookup"><span data-stu-id="08b5d-159">By default the list of jobs is sorted on submit time.</span></span> <span data-ttu-id="08b5d-160">Så att de nyligen skickade jobb visas först.</span><span class="sxs-lookup"><span data-stu-id="08b5d-160">So the most recently submitted jobs appear first.</span></span> <span data-ttu-id="08b5d-161">Som standard i ADLA konto kommer ihåg jobb i 180 dagar men Ge AdlJob-cmdlet som standard returnerar endast de första 500.</span><span class="sxs-lookup"><span data-stu-id="08b5d-161">By default, The ADLA account remembers jobs for 180 days, but the Ge-AdlJob  cmdlet by default returns only the first 500.</span></span> <span data-ttu-id="08b5d-162">Använd - översta parametern för att visa en lista med ett visst antal jobb.</span><span class="sxs-lookup"><span data-stu-id="08b5d-162">Use -Top parameter to list a specific number of jobs.</span></span>

```powershell
$jobs = Get-AdlJob -Account $adla -Top 10
```


### <a name="list-jobs-based-on-the-value-of-job-property"></a><span data-ttu-id="08b5d-163">Lista över jobb baserat på värdet för egenskapen jobb</span><span class="sxs-lookup"><span data-stu-id="08b5d-163">List jobs based on the value of job property</span></span>

<span data-ttu-id="08b5d-164">Med den `-State` parameter.</span><span class="sxs-lookup"><span data-stu-id="08b5d-164">Using the `-State` parameter.</span></span> <span data-ttu-id="08b5d-165">Du kan kombinera dessa värden:</span><span class="sxs-lookup"><span data-stu-id="08b5d-165">You can combine any of these values:</span></span>

* `Accepted`
* `Compiling`
* `Ended`
* `New`
* `Paused`
* `Queued`
* `Running`
* `Scheduling`
* `Start`

```powershell
# List the running jobs
Get-AdlJob -Account $adla -State Running

# List the jobs that have completed
Get-AdlJob -Account $adla -State Ended

# List the jobs that have not started yet
Get-AdlJob -Account $adla -State Accepted,Compiling,New,Paused,Scheduling,Start
```

<span data-ttu-id="08b5d-166">Använd den `-Result` parametern för att identifiera om avslutades jobben har slutförts.</span><span class="sxs-lookup"><span data-stu-id="08b5d-166">Use the `-Result` parameter to detect whether ended jobs completed successfully.</span></span> <span data-ttu-id="08b5d-167">Det har dessa värden:</span><span class="sxs-lookup"><span data-stu-id="08b5d-167">It has these values:</span></span>

* <span data-ttu-id="08b5d-168">Avbröts</span><span class="sxs-lookup"><span data-stu-id="08b5d-168">Cancelled</span></span>
* <span data-ttu-id="08b5d-169">Det gick inte</span><span class="sxs-lookup"><span data-stu-id="08b5d-169">Failed</span></span>
* <span data-ttu-id="08b5d-170">Ingen</span><span class="sxs-lookup"><span data-stu-id="08b5d-170">None</span></span>
* <span data-ttu-id="08b5d-171">Lyckades</span><span class="sxs-lookup"><span data-stu-id="08b5d-171">Succeeded</span></span>

``` powershell
# List Successful jobs.
Get-AdlJob -Account $adla -State Ended -Result Succeeded

# List Failed jobs.
Get-AdlJob -Account $adla -State Ended -Result Failed
```


<span data-ttu-id="08b5d-172">Den `-Submitter` parametern hjälper dig att identifiera vem som har skickat ett jobb.</span><span class="sxs-lookup"><span data-stu-id="08b5d-172">The `-Submitter` parameter helps you identify who submitted a job.</span></span>

```powershell
Get-AdlJob -Account $adla -Submitter "joe@contoso.com"
```

<span data-ttu-id="08b5d-173">Den `-SubmittedAfter` är användbart i filtrering för ett tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="08b5d-173">The `-SubmittedAfter` is useful in filtering to a time range.</span></span>


```powershell
# List  jobs submitted in the last day.
$d = [DateTime]::Now.AddDays(-1)
Get-AdlJob -Account $adla -SubmittedAfter $d

# List  jobs submitted in the last seven day.
$d = [DateTime]::Now.AddDays(-7)
Get-AdlJob -Account $adla -SubmittedAfter $d
```

### <a name="common-scenarios-for-listing-jobs"></a><span data-ttu-id="08b5d-174">Vanliga scenarier för att visa en lista över jobb</span><span class="sxs-lookup"><span data-stu-id="08b5d-174">Common scenarios for listing jobs</span></span>


```
# List jobs submitted in the last five days and that successfully completed.
$d = (Get-Date).AddDays(-5)
Get-AdlJob -Account $adla -SubmittedAfter $d -State Ended -Result Succeeded

# List all failed jobs submitted by "joe@contoso.com" within the past seven days.
Get-AdlJob -Account $adla `
    -Submitter "joe@contoso.com" `
    -SubmittedAfter (Get-Date).AddDays(-7) `
    -Result Failed
```

## <a name="filtering-a-list-of-jobs"></a><span data-ttu-id="08b5d-175">Filtrera en lista över jobb</span><span class="sxs-lookup"><span data-stu-id="08b5d-175">Filtering a list of jobs</span></span>

<span data-ttu-id="08b5d-176">När du har en lista över jobb i den aktuella PowerShell-sessionen.</span><span class="sxs-lookup"><span data-stu-id="08b5d-176">Once you have a list of jobs in your current PowerShell session.</span></span> <span data-ttu-id="08b5d-177">Du kan använda vanliga PowerShell-cmdlets för att filtrera listan.</span><span class="sxs-lookup"><span data-stu-id="08b5d-177">You can use normal PowerShell cmdlets to filter the list.</span></span>

<span data-ttu-id="08b5d-178">Filtrera en lista över jobb till jobb som skickats under de senaste 24 timmarna</span><span class="sxs-lookup"><span data-stu-id="08b5d-178">Filter a list of jobs to the jobs submitted in the last 24 hours</span></span>

```
$upperdate = Get-Date
$lowerdate = $upperdate.AddHours(-24)
$jobs | Where-Object { $_.EndTime -ge $lowerdate }
```

<span data-ttu-id="08b5d-179">Filtrera en lista över jobb till jobb som avslutats under de senaste 24 timmarna</span><span class="sxs-lookup"><span data-stu-id="08b5d-179">Filter a list of jobs to the jobs that ended in the last 24 hours</span></span>

```
$upperdate = Get-Date
$lowerdate = $upperdate.AddHours(-24)
$jobs | Where-Object { $_.SubmitTime -ge $lowerdate }
```

<span data-ttu-id="08b5d-180">Filtrera en lista över jobb till de jobb som startats.</span><span class="sxs-lookup"><span data-stu-id="08b5d-180">Filter a list of jobs to the jobs that started running.</span></span> <span data-ttu-id="08b5d-181">Ett jobb kan misslyckas vid kompileringen - och därför aldrig startas.</span><span class="sxs-lookup"><span data-stu-id="08b5d-181">A job might fail at compile time - and so it never starts.</span></span> <span data-ttu-id="08b5d-182">Nu ska vi titta på de jobb som misslyckades som startade körs och sedan misslyckades.</span><span class="sxs-lookup"><span data-stu-id="08b5d-182">Let's look at the failed jobs that actually started running and then failed.</span></span>

```powershell
$jobs | Where-Object { $_.StartTime -ne $null }
```

### <a name="analyzing-a-list-of-jobs"></a><span data-ttu-id="08b5d-183">Analysera en lista över jobb</span><span class="sxs-lookup"><span data-stu-id="08b5d-183">Analyzing a list of jobs</span></span>

<span data-ttu-id="08b5d-184">Använd den `Group-Object` för att analysera en lista över jobb.</span><span class="sxs-lookup"><span data-stu-id="08b5d-184">Use the `Group-Object` cmdlet to analyze a list of jobs.</span></span>

```
# Count the number of jobs by Submitter
$jobs | Group-Object Submitter | Select -Property Count,Name

# Count the number of jobs by Result
$jobs | Group-Object Result | Select -Property Count,Name

# Count the number of jobs by State
$jobs | Group-Object State | Select -Property Count,Name

#  Count the number of jobs by DegreeOfParallelism
$jobs | Group-Object DegreeOfParallelism | Select -Property Count,Name
```
<span data-ttu-id="08b5d-185">När du utför en analys, kan det vara praktiskt att lägga till egenskaper till jobbobjekt för att filtrera och gruppera enklare.</span><span class="sxs-lookup"><span data-stu-id="08b5d-185">When performing an analysis, it can be useful to add properties to the Job objects to make filtering and grouping simpler.</span></span> <span data-ttu-id="08b5d-186">Följande utdrag visar så här kommenterar du en Jobbinformations med beräknade egenskaper.</span><span class="sxs-lookup"><span data-stu-id="08b5d-186">The following  snippet shows how to annotate a JobInfo with calculated properties.</span></span>

```
function annotate_job( $j )
{
    $dic1 = @{
        Label='AUHours';
        Expression={ ($_.DegreeOfParallelism * ($_.EndTime-$_.StartTime).TotalHours)}}
    $dic2 = @{
        Label='DurationSeconds';
        Expression={ ($_.EndTime-$_.StartTime).TotalSeconds}}
    $dic3 = @{
        Label='DidRun';
        Expression={ ($_.StartTime -ne $null)}}

    $j2 = $j | select *, $dic1, $dic2, $dic3
    $j2
}

$jobs = Get-AdlJob -Account $adla -Top 10
$jobs = $jobs | %{ annotate_job( $_ ) }
```

## <a name="get-information-about-pipelines-and-recurrences"></a><span data-ttu-id="08b5d-187">Hämta information om pipelines och repetitioner</span><span class="sxs-lookup"><span data-stu-id="08b5d-187">Get information about pipelines and recurrences</span></span>

<span data-ttu-id="08b5d-188">Använd den `Get-AdlJobPipeline` för att se pipeline-information skickats tidigare jobb.</span><span class="sxs-lookup"><span data-stu-id="08b5d-188">Use the `Get-AdlJobPipeline` cmdlet to see the pipeline information previously submitted jobs.</span></span>

```powershell
$pipelines = Get-AdlJobPipeline -Account $adla

$pipeline = Get-AdlJobPipeline -Account $adla -PipelineId "<pipeline ID>"
```

<span data-ttu-id="08b5d-189">Använd den `Get-AdlJobRecurrence` för att se informationen upprepning för tidigare skickade jobb.</span><span class="sxs-lookup"><span data-stu-id="08b5d-189">Use the `Get-AdlJobRecurrence` cmdlet to see the recurrence information for previously submitted jobs.</span></span>

```powershell
$recurrences = Get-AdlJobRecurrence -Account $adla

$recurrence = Get-AdlJobRecurrence -Account $adla -RecurrenceId "<recurrence ID>"
```

## <a name="get-information-about-a-job"></a><span data-ttu-id="08b5d-190">Hämta information om ett jobb</span><span class="sxs-lookup"><span data-stu-id="08b5d-190">Get information about a job</span></span>

### <a name="get-job-status"></a><span data-ttu-id="08b5d-191">Hämta jobbstatus</span><span class="sxs-lookup"><span data-stu-id="08b5d-191">Get job status</span></span>

<span data-ttu-id="08b5d-192">Hämta status för ett specifikt jobb.</span><span class="sxs-lookup"><span data-stu-id="08b5d-192">Get the status of a specific job.</span></span>

```powershell
Get-AdlJob -AccountName $adla -JobId $job.JobId
```

### <a name="examine-the-job-outputs"></a><span data-ttu-id="08b5d-193">Granska utdata för jobbet</span><span class="sxs-lookup"><span data-stu-id="08b5d-193">Examine the job outputs</span></span>

<span data-ttu-id="08b5d-194">När jobbet är slut kan du kontrollera att filen finns genom att lista filer i en mapp.</span><span class="sxs-lookup"><span data-stu-id="08b5d-194">After the job has ended, check if the output file exists by listing the files in a folder.</span></span>

```powershell
Get-AdlStoreChildItem -Account $adls -Path "/"
```

## <a name="manage-running-jobs"></a><span data-ttu-id="08b5d-195">Hantera jobb som körs</span><span class="sxs-lookup"><span data-stu-id="08b5d-195">Manage running jobs</span></span>

### <a name="cancel-a-job"></a><span data-ttu-id="08b5d-196">Avbryta ett jobb</span><span class="sxs-lookup"><span data-stu-id="08b5d-196">Cancel a job</span></span>

```powershell
Stop-AdlJob -Account $adls -JobID $jobID
```

### <a name="wait-for-a-job-to-finish"></a><span data-ttu-id="08b5d-197">Vänta tills ett jobb ska slutföras</span><span class="sxs-lookup"><span data-stu-id="08b5d-197">Wait for a job to finish</span></span>

<span data-ttu-id="08b5d-198">I stället för att upprepa `Get-AdlAnalyticsJob` tills ett jobb är klar kan du använda den `Wait-AdlJob` för att vänta på att jobbet ska sluta.</span><span class="sxs-lookup"><span data-stu-id="08b5d-198">Instead of repeating `Get-AdlAnalyticsJob` until a job finishes, you can use the `Wait-AdlJob` cmdlet to wait for the job to end.</span></span>

```powershell
Wait-AdlJob -Account $adla -JobId $job.JobId
```

## <a name="manage-compute-policies"></a><span data-ttu-id="08b5d-199">Hantera principer för beräkning</span><span class="sxs-lookup"><span data-stu-id="08b5d-199">Manage compute policies</span></span>

### <a name="list-existing-compute-policies"></a><span data-ttu-id="08b5d-200">Visa en lista med befintliga beräknings-principer</span><span class="sxs-lookup"><span data-stu-id="08b5d-200">List existing compute policies</span></span>

<span data-ttu-id="08b5d-201">Den `Get-AdlAnalyticsComputePolicy` cmdlet hämtar information om beräkning principer för ett Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="08b5d-201">The `Get-AdlAnalyticsComputePolicy` cmdlet retrieves info about compute policies for a Data Lake Analytics account.</span></span>

```powershell
$policies = Get-AdlAnalyticsComputePolicy -Account $adla
```

### <a name="create-a-compute-policy"></a><span data-ttu-id="08b5d-202">Skapa en princip för beräkning</span><span class="sxs-lookup"><span data-stu-id="08b5d-202">Create a compute policy</span></span>

<span data-ttu-id="08b5d-203">Den `New-AdlAnalyticsComputePolicy` cmdlet skapar en ny princip för beräkning för ett Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="08b5d-203">The `New-AdlAnalyticsComputePolicy` cmdlet creates a new compute policy for a Data Lake Analytics account.</span></span> <span data-ttu-id="08b5d-204">Det här exemplet anger maximala Australien som är tillgängliga för den angivna användaren till 50 och minsta jobbets prioritet 250.</span><span class="sxs-lookup"><span data-stu-id="08b5d-204">This example sets  the maximum AUs available to the specified user to 50, and the minimum job priority to 250.</span></span>

```powershell
$userObjectId = (Get-AzureRmAdUser -SearchString "garymcdaniel@contoso.com").Id

New-AdlAnalyticsComputePolicy -Account $adla -Name "GaryMcDaniel" -ObjectId $objectId -ObjectType User -MaxDegreeOfParallelismPerJob 50 -MinPriorityPerJob 250
```

## <a name="check-for-the-existence-of-a-file"></a><span data-ttu-id="08b5d-205">Kontrollera om finns en fil.</span><span class="sxs-lookup"><span data-stu-id="08b5d-205">Check for the existence of a file.</span></span>

```powershell
Test-AdlStoreItem -Account $adls -Path "/data.csv"
```

## <a name="uploading-and-downloading"></a><span data-ttu-id="08b5d-206">Överföra och ladda ned</span><span class="sxs-lookup"><span data-stu-id="08b5d-206">Uploading and downloading</span></span>

<span data-ttu-id="08b5d-207">Överför en fil.</span><span class="sxs-lookup"><span data-stu-id="08b5d-207">Upload a file.</span></span>

```powershell
Import-AdlStoreItem -AccountName $adls -Path "c:\data.tsv" -Destination "/data_copy.csv" 
```

<span data-ttu-id="08b5d-208">Ladda upp en hel mapp rekursivt.</span><span class="sxs-lookup"><span data-stu-id="08b5d-208">Upload an entire folder recursively.</span></span>

```powershell
Import-AdlStoreItem -AccountName $adls -Path "c:\myData\" -Destination "/myData/" -Recurse
```

<span data-ttu-id="08b5d-209">Hämta en fil.</span><span class="sxs-lookup"><span data-stu-id="08b5d-209">Download a file.</span></span>

```powershell
Export-AdlStoreItem -AccountName $adls -Path "/data.csv" -Destination "c:\data.csv"
```

<span data-ttu-id="08b5d-210">Hämta en hel mapp rekursivt.</span><span class="sxs-lookup"><span data-stu-id="08b5d-210">Download an entire folder recursively.</span></span>

```powershell
Export-AdlStoreItem -AccountName $adls -Path "/" -Destination "c:\myData\" -Recurse
```

> [!NOTE]
> <span data-ttu-id="08b5d-211">Om överföring eller hämtning processen avbryts, kan du försöka återuppta processen genom att köra cmdleten igen med den ``-Resume`` flaggan.</span><span class="sxs-lookup"><span data-stu-id="08b5d-211">If the upload or download process is interrupted, you can attempt to resume the process by running the cmdlet again with the ``-Resume`` flag.</span></span>

## <a name="manage-catalog-items"></a><span data-ttu-id="08b5d-212">Hantera katalogobjekt</span><span class="sxs-lookup"><span data-stu-id="08b5d-212">Manage catalog items</span></span>

<span data-ttu-id="08b5d-213">U-SQL-katalogen används för att strukturera data och kod så att de kan delas av U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="08b5d-213">The U-SQL catalog is used to structure data and code so they can be shared by U-SQL scripts.</span></span> <span data-ttu-id="08b5d-214">Katalogen kan möjliga med data i Azure Data Lake högsta prestanda.</span><span class="sxs-lookup"><span data-stu-id="08b5d-214">The catalog enables the highest performance possible with data in Azure Data Lake.</span></span> <span data-ttu-id="08b5d-215">Mer information finns i [Använd U-SQL-katalogen](data-lake-analytics-use-u-sql-catalog.md).</span><span class="sxs-lookup"><span data-stu-id="08b5d-215">For more information, see [Use U-SQL catalog](data-lake-analytics-use-u-sql-catalog.md).</span></span>

### <a name="list-items-in-the-u-sql-catalog"></a><span data-ttu-id="08b5d-216">Listobjekt i U-SQL-katalogen</span><span class="sxs-lookup"><span data-stu-id="08b5d-216">List items in the U-SQL catalog</span></span>

```powershell
# List U-SQL databases
Get-AdlCatalogItem -Account $adla -ItemType Database 

# List tables within a database
Get-AdlCatalogItem -Account $adla -ItemType Table -Path "database"

# List tables within a schema.
Get-AdlCatalogItem -Account $adla -ItemType Table -Path "database.schema"
```

<span data-ttu-id="08b5d-217">Lista över alla sammansättningar i alla databaser i en ADLA.</span><span class="sxs-lookup"><span data-stu-id="08b5d-217">List all the assemblies in all the databases in an ADLA Account.</span></span>

```powershell
$dbs = Get-AdlCatalogItem -Account $adla -ItemType Database

foreach ($db in $dbs)
{
    $asms = Get-AdlCatalogItem -Account $adla -ItemType Assembly -Path $db.Name

    foreach ($asm in $asms)
    {
        $asmname = "[" + $db.Name + "].[" + $asm.Name + "]"
        Write-Host $asmname
    }
}
```

### <a name="get-details-about-a-catalog-item"></a><span data-ttu-id="08b5d-218">Få information om ett katalogobjekt</span><span class="sxs-lookup"><span data-stu-id="08b5d-218">Get details about a catalog item</span></span>

```powershell
# Get details of a table
Get-AdlCatalogItem  -Account $adla -ItemType Table -Path "master.dbo.mytable"

# Test existence of a U-SQL database.
Test-AdlCatalogItem  -Account $adla -ItemType Database -Path "master"
```

### <a name="create-credentials-in-a-catalog"></a><span data-ttu-id="08b5d-219">Skapa autentiseringsuppgifter i en katalog</span><span class="sxs-lookup"><span data-stu-id="08b5d-219">Create credentials in a catalog</span></span>

<span data-ttu-id="08b5d-220">I en U-SQL-databas, skapar du ett autentiseringsuppgiftobjekt för en databas som finns på Azure.</span><span class="sxs-lookup"><span data-stu-id="08b5d-220">Within a U-SQL database, create a credential object for a database hosted in Azure.</span></span> <span data-ttu-id="08b5d-221">U-SQL-autentiseringsuppgifter för närvarande den enda typen av katalogobjekt som du kan skapa med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="08b5d-221">Currently, U-SQL credentials are the only type of catalog item that you can create through PowerShell.</span></span>

```powershell
$dbName = "master"
$credentialName = "ContosoDbCreds"
$dbUri = "https://contoso.database.windows.net:8080"

New-AdlCatalogCredential -AccountName $adla `
          -DatabaseName $db `
          -CredentialName $credentialName `
          -Credential (Get-Credential) `
          -Uri $dbUri
```

### <a name="get-basic-information-about-an-adla-account"></a><span data-ttu-id="08b5d-222">Hämta grundläggande information om ett ADLA konto</span><span class="sxs-lookup"><span data-stu-id="08b5d-222">Get basic information about an ADLA account</span></span>

<span data-ttu-id="08b5d-223">Ges ett kontonamn hämtar följande kod grundläggande information om kontot</span><span class="sxs-lookup"><span data-stu-id="08b5d-223">Given an account name, the following code looks up basic information about the account</span></span>

```
$adla_acct = Get-AdlAnalyticsAccount -Name "saveenrdemoadla"
$adla_name = $adla_acct.Name
$adla_subid = $adla_acct.Id.Split("/")[2]
$adla_sub = Get-AzureRmSubscription -SubscriptionId $adla_subid
$adla_subname = $adla_sub.Name
$adla_defadls_datasource = Get-AdlAnalyticsDataSource -Account $adla_name  | ? { $_.IsDefault } 
$adla_defadlsname = $adla_defadls_datasource.Name

Write-Host "ADLA Account Name" $adla_name
Write-Host "Subscription Id" $adla_subid
Write-Host "Subscription Name" $adla_subname
Write-Host "Defautl ADLS Store" $adla_defadlsname
Write-Host 

Write-Host '$subname' " = ""$adla_subname"" "
Write-Host '$subid' " = ""$adla_subid"" "
Write-Host '$adla' " = ""$adla_name"" "
Write-Host '$adls' " = ""$adla_defadlsname"" "
```

## <a name="working-with-azure"></a><span data-ttu-id="08b5d-224">Arbeta med Azure</span><span class="sxs-lookup"><span data-stu-id="08b5d-224">Working with Azure</span></span>

### <a name="get-details-of-azurerm-errors"></a><span data-ttu-id="08b5d-225">Hämta information om AzureRm fel</span><span class="sxs-lookup"><span data-stu-id="08b5d-225">Get details of AzureRm errors</span></span>

```powershell
Resolve-AzureRmError -Last
```

### <a name="verify-if-you-are-running-as-an-administrator"></a><span data-ttu-id="08b5d-226">Kontrollera om du kör som administratör</span><span class="sxs-lookup"><span data-stu-id="08b5d-226">Verify if you are running as an administrator</span></span>

```powershell
function Test-Administrator  
{  
    $user = [Security.Principal.WindowsIdentity]::GetCurrent();
    $p = New-Object Security.Principal.WindowsPrincipal $user
    $p.IsInRole([Security.Principal.WindowsBuiltinRole]::Administrator)  
}
```

### <a name="find-a-tenantid"></a><span data-ttu-id="08b5d-227">Hitta en TenantID</span><span class="sxs-lookup"><span data-stu-id="08b5d-227">Find a TenantID</span></span>

<span data-ttu-id="08b5d-228">Från ett prenumerationsnamn:</span><span class="sxs-lookup"><span data-stu-id="08b5d-228">From a subscription name:</span></span>

```powershell
function Get-TenantIdFromSubcriptionName( [string] $subname )
{
    $sub = (Get-AzureRmSubscription -SubscriptionName $subname)
    $sub.TenantId
}

Get-TenantIdFromSubcriptionName "ADLTrainingMS"
```

<span data-ttu-id="08b5d-229">Från ett prenumerations-id:</span><span class="sxs-lookup"><span data-stu-id="08b5d-229">From a subscription id:</span></span>

```powershell
function Get-TenantIdFromSubcriptionId( [string] $subid )
{
    $sub = (Get-AzureRmSubscription -SubscriptionId $subid)
    $sub.TenantId
}

$subid = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
Get-TenantIdFromSubcriptionId $subid
```

<span data-ttu-id="08b5d-230">Från en domänadress, till exempel ”contoso.com”</span><span class="sxs-lookup"><span data-stu-id="08b5d-230">From a domain address such as "contoso.com"</span></span>


```powershell
function Get-TenantIdFromDomain( $domain )
{
    $url = "https://login.windows.net/" + $domain + "/.well-known/openid-configuration"
    return (Invoke-WebRequest $url|ConvertFrom-Json).token_endpoint.Split('/')[3]
}

$domain = "contoso.com"
Get-TenantIdFromDomain $domain
```

### <a name="list-all-your-subscriptions-and-tenant-ids"></a><span data-ttu-id="08b5d-231">En lista över alla prenumerationer och klient-ID: n</span><span class="sxs-lookup"><span data-stu-id="08b5d-231">List all your subscriptions and tenant ids</span></span>

```powershell
$subs = Get-AzureRmSubscription
foreach ($sub in $subs)
{
    Write-Host $sub.Name "("  $sub.Id ")"
    Write-Host "`tTenant Id" $sub.TenantId
}
```

## <a name="create-a-data-lake-analytics-account-using-a-template"></a><span data-ttu-id="08b5d-232">Skapa ett Data Lake Analytics-konto med hjälp av en mall</span><span class="sxs-lookup"><span data-stu-id="08b5d-232">Create a Data Lake Analytics account using a template</span></span>

<span data-ttu-id="08b5d-233">Du kan också använda en Azure-resursgrupp-mallen med följande PowerShell-skript:</span><span class="sxs-lookup"><span data-stu-id="08b5d-233">You can also use an Azure Resource Group template using the following  PowerShell script:</span></span>

```powershell
$subId = "<Your Azure Subscription ID>"

$rg = "<New Azure Resource Group Name>"
$location = "<Location (such as East US 2)>"
$adls = "<New Data Lake Store Account Name>"
$adla = "<New Data Lake Analytics Account Name>"

$deploymentName = "MyDataLakeAnalyticsDeployment"
$armTemplateFile = "<LocalFolderPath>\azuredeploy.json"  # update the JSON template path 

# Log in to Azure
Login-AzureRmAccount -SubscriptionId $subId

# Create the resource group
New-AzureRmResourceGroup -Name $rg -Location $location

# Create the Data Lake Analytics account with the default Data Lake Store account.
$parameters = @{"adlAnalyticsName"=$adla; "adlStoreName"=$adls}
New-AzureRmResourceGroupDeployment -Name $deploymentName -ResourceGroupName $rg -TemplateFile $armTemplateFile -TemplateParameterObject $parameters 
```

<span data-ttu-id="08b5d-234">Mer information finns i [distribuera ett program med Azure Resource Manager-mall](../azure-resource-manager/resource-group-template-deploy.md) och [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="08b5d-234">For more information, see [Deploy an application with Azure Resource Manager template](../azure-resource-manager/resource-group-template-deploy.md) and [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="08b5d-235">**Exempelmall**</span><span class="sxs-lookup"><span data-stu-id="08b5d-235">**Example template**</span></span>

<span data-ttu-id="08b5d-236">Spara följande text som en `.json` filen och sedan använda föregående PowerShell-skriptet för att använda mallen.</span><span class="sxs-lookup"><span data-stu-id="08b5d-236">Save the following text as a `.json` file, and then use the preceding PowerShell script to use the template.</span></span> 

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adlAnalyticsName": {
      "type": "string",
      "metadata": {
        "description": "The name of the Data Lake Analytics account to create."
      }
    },
    "adlStoreName": {
      "type": "string",
      "metadata": {
        "description": "The name of the Data Lake Store account to create."
      }
    }
  },
  "resources": [
    {
      "name": "[parameters('adlStoreName')]",
      "type": "Microsoft.DataLakeStore/accounts",
      "location": "East US 2",
      "apiVersion": "2015-10-01-preview",
      "dependsOn": [ ],
      "tags": { }
    },
    {
      "name": "[parameters('adlAnalyticsName')]",
      "type": "Microsoft.DataLakeAnalytics/accounts",
      "location": "East US 2",
      "apiVersion": "2015-10-01-preview",
      "dependsOn": [ "[concat('Microsoft.DataLakeStore/accounts/',parameters('adlStoreName'))]" ],
      "tags": { },
      "properties": {
        "defaultDataLakeStoreAccount": "[parameters('adlStoreName')]",
        "dataLakeStoreAccounts": [
          { "name": "[parameters('adlStoreName')]" }
        ]
      }
    }
  ],
  "outputs": {
    "adlAnalyticsAccount": {
      "type": "object",
      "value": "[reference(resourceId('Microsoft.DataLakeAnalytics/accounts',parameters('adlAnalyticsName')))]"
    },
    "adlStoreAccount": {
      "type": "object",
      "value": "[reference(resourceId('Microsoft.DataLakeStore/accounts',parameters('adlStoreName')))]"
    }
  }
}
```

## <a name="next-steps"></a><span data-ttu-id="08b5d-237">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="08b5d-237">Next steps</span></span>
* [<span data-ttu-id="08b5d-238">Översikt över Microsoft Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="08b5d-238">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* <span data-ttu-id="08b5d-239">Kom igång med Data Lake Analytics med hjälp av [Azure-portalen](data-lake-analytics-get-started-portal.md) | [Azure PowerShell](data-lake-analytics-get-started-powershell.md) | [CLI 2.0](data-lake-analytics-get-started-cli2.md)</span><span class="sxs-lookup"><span data-stu-id="08b5d-239">Get started with Data Lake Analytics using [Azure portal](data-lake-analytics-get-started-portal.md) | [Azure PowerShell](data-lake-analytics-get-started-powershell.md) | [CLI 2.0](data-lake-analytics-get-started-cli2.md)</span></span>
* <span data-ttu-id="08b5d-240">Hantera Azure Data Lake Analytics med hjälp av [Azure-portalen](data-lake-analytics-manage-use-portal.md) | [Azure PowerShell](data-lake-analytics-manage-use-powershell.md) | [CLI](data-lake-analytics-manage-use-cli.md)</span><span class="sxs-lookup"><span data-stu-id="08b5d-240">Manage Azure Data Lake Analytics using [Azure portal](data-lake-analytics-manage-use-portal.md) | [Azure PowerShell](data-lake-analytics-manage-use-powershell.md) | [CLI](data-lake-analytics-manage-use-cli.md)</span></span> 
