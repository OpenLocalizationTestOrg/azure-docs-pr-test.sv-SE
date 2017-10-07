---
title: "aaaManage Azure Data Lake Analytics med hjälp av Python | Microsoft Docs"
description: "Lär dig hur toouse Python toocreate ett Data Lake lagrar konto och skicka jobb. "
services: data-lake-analytics
documentationcenter: 
author: matt1883
manager: jhubbard
editor: cgronlun
ms.assetid: d4213a19-4d0f-49c9-871c-9cd6ed7cf731
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/18/2017
ms.author: saveenr
ms.openlocfilehash: 3c0fff155db7c4fd4e84c2562816995eb156be16
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-data-lake-analytics-using-python"></a><span data-ttu-id="425be-103">Hantera Azure Data Lake Analytics använder Python</span><span class="sxs-lookup"><span data-stu-id="425be-103">Manage Azure Data Lake Analytics using Python</span></span>

## <a name="python-versions"></a><span data-ttu-id="425be-104">Python-versioner</span><span class="sxs-lookup"><span data-stu-id="425be-104">Python versions</span></span>

* <span data-ttu-id="425be-105">Använda en 64-bitars version av Python.</span><span class="sxs-lookup"><span data-stu-id="425be-105">Use a 64-bit version of Python.</span></span>
* <span data-ttu-id="425be-106">Du kan använda hello standard Python-distribution finns på  **[Python.org hämtar](https://www.python.org/downloads/)**.</span><span class="sxs-lookup"><span data-stu-id="425be-106">You can use hello standard Python distribution found at **[Python.org downloads](https://www.python.org/downloads/)**.</span></span> 
* <span data-ttu-id="425be-107">Många utvecklare hitta den praktiska toouse hello  **[Anaconda Python-distribution](https://www.continuum.io/downloads)**.</span><span class="sxs-lookup"><span data-stu-id="425be-107">Many developers find it convenient toouse hello **[Anaconda Python distribution](https://www.continuum.io/downloads)**.</span></span>  
* <span data-ttu-id="425be-108">Den här artikeln skrevs med Python version 3,6 från hello standard Python-distribution</span><span class="sxs-lookup"><span data-stu-id="425be-108">This article was written using Python version 3.6 from hello standard Python distribution</span></span>

## <a name="install-azure-python-sdk"></a><span data-ttu-id="425be-109">Installera Azure Python SDK</span><span class="sxs-lookup"><span data-stu-id="425be-109">Install Azure Python SDK</span></span>

<span data-ttu-id="425be-110">Installera hello följande moduler:</span><span class="sxs-lookup"><span data-stu-id="425be-110">Install hello following modules:</span></span>

* <span data-ttu-id="425be-111">Hej **azure-mgmt-resurs** modulen innehåller andra Azure-moduler för Active Directory, osv.</span><span class="sxs-lookup"><span data-stu-id="425be-111">hello **azure-mgmt-resource** module includes other Azure modules for Active Directory, etc.</span></span>
* <span data-ttu-id="425be-112">Hej **azure-mgmt-datalake-store** modulen innehåller hanteringsåtgärder för hello Azure Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="425be-112">hello **azure-mgmt-datalake-store** module includes hello Azure Data Lake Store account management operations.</span></span>
* <span data-ttu-id="425be-113">Hej **azure datalake store** modulen innehåller hello Azure Data Lake Store filsystemsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="425be-113">hello **azure-datalake-store** module includes hello Azure Data Lake Store filesystem operations.</span></span> 
* <span data-ttu-id="425be-114">Hej **azure datalake analytics** modulen innehåller hello Azure Data Lake Analytics-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="425be-114">hello **azure-datalake-analytics** module includes hello Azure Data Lake Analytics operations.</span></span> 

<span data-ttu-id="425be-115">Kontrollera först att du har hello senaste `pip` genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="425be-115">First, ensure you have hello latest `pip` by running hello following command:</span></span>

```
python -m pip install --upgrade pip
```

<span data-ttu-id="425be-116">Det här dokumentet har skrivits `pip version 9.0.1`.</span><span class="sxs-lookup"><span data-stu-id="425be-116">This document was written using `pip version 9.0.1`.</span></span>

<span data-ttu-id="425be-117">Använder följande hello `pip` kommandon tooinstall hello moduler från hello kommandorad:</span><span class="sxs-lookup"><span data-stu-id="425be-117">Use hello following `pip` commands tooinstall hello modules from hello commandline:</span></span>

```
pip install azure-mgmt-resource
pip install azure-datalake-store
pip install azure-mgmt-datalake-store
pip install azure-mgmt-datalake-analytics
```

## <a name="create-a-new-python-script"></a><span data-ttu-id="425be-118">Skapa ett nytt Python-skript</span><span class="sxs-lookup"><span data-stu-id="425be-118">Create a new Python script</span></span>

<span data-ttu-id="425be-119">Klistra in följande kod i hello skript hello:</span><span class="sxs-lookup"><span data-stu-id="425be-119">Paste hello following code into hello script:</span></span>

```python
## Use this only for Azure AD service-to-service authentication
#from azure.common.credentials import ServicePrincipalCredentials

## Use this only for Azure AD end-user authentication
#from azure.common.credentials import UserPassCredentials

## Required for Azure Resource Manager
from azure.mgmt.resource.resources import ResourceManagementClient
from azure.mgmt.resource.resources.models import ResourceGroup

## Required for Azure Data Lake Store account management
from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
from azure.mgmt.datalake.store.models import DataLakeStoreAccount

## Required for Azure Data Lake Store filesystem management
from azure.datalake.store import core, lib, multithread

## Required for Azure Data Lake Analytics account management
from azure.mgmt.datalake.analytics.account import DataLakeAnalyticsAccountManagementClient
from azure.mgmt.datalake.analytics.account.models import DataLakeAnalyticsAccount, DataLakeStoreAccountInfo

## Required for Azure Data Lake Analytics job management
from azure.mgmt.datalake.analytics.job import DataLakeAnalyticsJobManagementClient
from azure.mgmt.datalake.analytics.job.models import JobInformation, JobState, USqlJobProperties

## Required for Azure Data Lake Analytics catalog management
from azure.mgmt.datalake.analytics.catalog import DataLakeAnalyticsCatalogManagementClient

## Use these as needed for your application
import logging, getpass, pprint, uuid, time
```

<span data-ttu-id="425be-120">Kör det här skriptet tooverify som hello moduler kan importeras.</span><span class="sxs-lookup"><span data-stu-id="425be-120">Run this script tooverify that hello modules can be imported.</span></span>

## <a name="authentication"></a><span data-ttu-id="425be-121">Autentisering</span><span class="sxs-lookup"><span data-stu-id="425be-121">Authentication</span></span>

### <a name="interactive-user-authentication-with-a-pop-up"></a><span data-ttu-id="425be-122">Interaktiv användarautentisering med ett popup-fönster</span><span class="sxs-lookup"><span data-stu-id="425be-122">Interactive user authentication with a pop-up</span></span>

<span data-ttu-id="425be-123">Den här metoden stöds inte.</span><span class="sxs-lookup"><span data-stu-id="425be-123">This method is not supported.</span></span>

### <a name="interactive-user-authentication-with-a-device-code"></a><span data-ttu-id="425be-124">Interaktiv användarautentisering med en kod för enheten</span><span class="sxs-lookup"><span data-stu-id="425be-124">Interactive user authentication with a device code</span></span>

```python
user = input('Enter hello user tooauthenticate with that has permission toosubscription: ')
password = getpass.getpass()
credentials = UserPassCredentials(user, password)
```

### <a name="noninteractive-authentication-with-spi-and-a-secret"></a><span data-ttu-id="425be-125">Icke-interaktiv autentisering med SPI och en hemlighet</span><span class="sxs-lookup"><span data-stu-id="425be-125">Noninteractive authentication with SPI and a secret</span></span>

```python
credentials = ServicePrincipalCredentials(client_id = 'FILL-IN-HERE', secret = 'FILL-IN-HERE', tenant = 'FILL-IN-HERE')
```

### <a name="noninteractive-authentication-with-api-and-a-certificate"></a><span data-ttu-id="425be-126">Icke-interaktiv autentisering med API: et och ett certifikat</span><span class="sxs-lookup"><span data-stu-id="425be-126">Noninteractive authentication with API and a certificate</span></span>

<span data-ttu-id="425be-127">Den här metoden stöds inte.</span><span class="sxs-lookup"><span data-stu-id="425be-127">This method is not supported.</span></span>

## <a name="common-script-variables"></a><span data-ttu-id="425be-128">Vanliga skriptvariabler</span><span class="sxs-lookup"><span data-stu-id="425be-128">Common script variables</span></span>

<span data-ttu-id="425be-129">Dessa variabler används i hello prover.</span><span class="sxs-lookup"><span data-stu-id="425be-129">These variables are used in hello samples.</span></span>

```python
subid= '<Azure Subscription ID>'
rg = '<Azure Resource Group Name>'
location = '<Location>' # i.e. 'eastus2'
adls = '<Azure Data Lake Store Account Name>'
adla = '<Azure Data Lake Analytics Account Name>'
```

## <a name="create-hello-clients"></a><span data-ttu-id="425be-130">Skapa hello klienter</span><span class="sxs-lookup"><span data-stu-id="425be-130">Create hello clients</span></span>

```python
resourceClient = ResourceManagementClient(credentials, subid)
adlaAcctClient = DataLakeAnalyticsAccountManagementClient(credentials, subid)
adlaJobClient = DataLakeAnalyticsJobManagementClient( credentials, 'azuredatalakeanalytics.net')
```

## <a name="create-an-azure-resource-group"></a><span data-ttu-id="425be-131">Skapa en Azure-resursgrupp</span><span class="sxs-lookup"><span data-stu-id="425be-131">Create an Azure Resource Group</span></span>

```python
armGroupResult = resourceClient.resource_groups.create_or_update( rg, ResourceGroup( location=location ) )
```

## <a name="create-data-lake-analytics-account"></a><span data-ttu-id="425be-132">Skapa ett Data Lake Analytics-konto</span><span class="sxs-lookup"><span data-stu-id="425be-132">Create Data Lake Analytics account</span></span>

<span data-ttu-id="425be-133">Skapa först en store-konto.</span><span class="sxs-lookup"><span data-stu-id="425be-133">First create a store account.</span></span>

```python
adlaAcctResult = adlaAcctClient.account.create(
    rg,
    adla,
    DataLakeAnalyticsAccount(
        location=location,
        default_data_lake_store_account=adls,
        data_lake_store_accounts=[DataLakeStoreAccountInfo(name=adls)]
    )
).wait()
```
<span data-ttu-id="425be-134">Skapa sedan en ADLA-konto som använder detta Arkiv.</span><span class="sxs-lookup"><span data-stu-id="425be-134">Then create an ADLA account that uses that store.</span></span>

```python
adlaAcctResult = adlaAcctClient.account.create(
    rg,
    adla,
    DataLakeAnalyticsAccount(
        location=location,
        default_data_lake_store_account=adls,
        data_lake_store_accounts=[DataLakeStoreAccountInfo(name=adls)]
    )
).wait()
```

## <a name="submit-a-job"></a><span data-ttu-id="425be-135">Skicka ett jobb</span><span class="sxs-lookup"><span data-stu-id="425be-135">Submit a job</span></span>

```python
script = """
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
"""

jobId = str(uuid.uuid4())
jobResult = adlaJobClient.job.create(
    adla,
    jobId,
    JobInformation(
        name='Sample Job',
        type='USql',
        properties=USqlJobProperties(script=script)
    )
)
```

## <a name="wait-for-a-job-tooend"></a><span data-ttu-id="425be-136">Vänta tills en jobbet tooend</span><span class="sxs-lookup"><span data-stu-id="425be-136">Wait for a job tooend</span></span>

```python
jobResult = adlaJobClient.job.get(adla, jobId)
while(jobResult.state != JobState.ended):
    print('Job is not yet done, waiting for 3 seconds. Current state: ' + jobResult.state.value)
    time.sleep(3)
    jobResult = adlaJobClient.job.get(adla, jobId)

print ('Job finished with result: ' + jobResult.result.value)
```

## <a name="list-pipelines-and-recurrences"></a><span data-ttu-id="425be-137">Lista pipelines och repetitioner</span><span class="sxs-lookup"><span data-stu-id="425be-137">List pipelines and recurrences</span></span>
<span data-ttu-id="425be-138">Beroende på om dina jobb har pipeline eller återkommande metadata kopplade kan visa du pipelines och repetitioner.</span><span class="sxs-lookup"><span data-stu-id="425be-138">Depending whether your jobs have pipeline or recurrence metadata attached, you can list pipelines and recurrences.</span></span>

```python
pipelines = adlaJobClient.pipeline.list(adla)
for p in pipelines:
    print('Pipeline: ' + p.name + ' ' + p.pipelineId)

recurrences = adlaJobClient.recurrence.list(adla)
for r in recurrences:
    print('Recurrence: ' + r.name + ' ' + r.recurrenceId)
```

## <a name="manage-compute-policies"></a><span data-ttu-id="425be-139">Hantera principer för beräkning</span><span class="sxs-lookup"><span data-stu-id="425be-139">Manage compute policies</span></span>

<span data-ttu-id="425be-140">Hej DataLakeAnalyticsAccountManagementClient objektet innehåller metoder för att hantera hello compute principer för ett Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="425be-140">hello DataLakeAnalyticsAccountManagementClient object provides methods for managing hello compute policies for a Data Lake Analytics account.</span></span>

### <a name="list-compute-policies"></a><span data-ttu-id="425be-141">Lista principer för beräkning</span><span class="sxs-lookup"><span data-stu-id="425be-141">List compute policies</span></span>

<span data-ttu-id="425be-142">hello följande kod hämtar en lista över beräkning principer för ett Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="425be-142">hello following code retrieves a list of compute policies for a Data Lake Analytics account.</span></span>

```python
policies = adlaAccountClient.computePolicies.listByAccount(rg, adla)
for p in policies:
    print('Name: ' + p.name + 'Type: ' + p.objectType + 'Max AUs / job: ' + p.maxDegreeOfParallelismPerJob + 'Min priority / job: ' + p.minPriorityPerJob)
```

### <a name="create-a-new-compute-policy"></a><span data-ttu-id="425be-143">Skapa en ny princip för beräkning</span><span class="sxs-lookup"><span data-stu-id="425be-143">Create a new compute policy</span></span>

<span data-ttu-id="425be-144">hello följande kod skapar en ny princip för beräkning för ett Data Lake Analytics-konto, inställningen hello maximala Australien tillgängliga toohello anges användaren too50 och hello minsta jobbet prioritet too250.</span><span class="sxs-lookup"><span data-stu-id="425be-144">hello following code creates a new compute policy for a Data Lake Analytics account, setting hello maximum AUs available toohello specified user too50, and hello minimum job priority too250.</span></span>

```python
userAadObjectId = "3b097601-4912-4d41-b9d2-78672fc2acde"
newPolicyParams = ComputePolicyCreateOrUpdateParameters(userAadObjectId, "User", 50, 250)
adlaAccountClient.computePolicies.createOrUpdate(rg, adla, "GaryMcDaniel", newPolicyParams)
```

## <a name="next-steps"></a><span data-ttu-id="425be-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="425be-145">Next steps</span></span>

- <span data-ttu-id="425be-146">toosee hello samma självstudier med andra verktyg klickar du på hello flikväljarna överst hello hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="425be-146">toosee hello same tutorial using other tools, click hello tab selectors on hello top of hello page.</span></span>
- <span data-ttu-id="425be-147">toolearn U-SQL finns [Kom igång med Azure Data Lake Analytics U-SQL-språket](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="425be-147">toolearn U-SQL, see [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md).</span></span>
- <span data-ttu-id="425be-148">Information om hanteringsuppgifter finns i [Hantera Azure Data Lake Analytics med hjälp av Azure Portal](data-lake-analytics-manage-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="425be-148">For management tasks, see [Manage Azure Data Lake Analytics using Azure portal](data-lake-analytics-manage-use-portal.md).</span></span>

