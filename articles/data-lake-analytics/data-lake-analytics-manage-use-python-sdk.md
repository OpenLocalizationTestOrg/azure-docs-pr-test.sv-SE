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
# <a name="manage-azure-data-lake-analytics-using-python"></a>Hantera Azure Data Lake Analytics använder Python

## <a name="python-versions"></a>Python-versioner

* Använda en 64-bitars version av Python.
* Du kan använda hello standard Python-distribution finns på  **[Python.org hämtar](https://www.python.org/downloads/)**. 
* Många utvecklare hitta den praktiska toouse hello  **[Anaconda Python-distribution](https://www.continuum.io/downloads)**.  
* Den här artikeln skrevs med Python version 3,6 från hello standard Python-distribution

## <a name="install-azure-python-sdk"></a>Installera Azure Python SDK

Installera hello följande moduler:

* Hej **azure-mgmt-resurs** modulen innehåller andra Azure-moduler för Active Directory, osv.
* Hej **azure-mgmt-datalake-store** modulen innehåller hanteringsåtgärder för hello Azure Data Lake Store-konto.
* Hej **azure datalake store** modulen innehåller hello Azure Data Lake Store filsystemsåtgärder. 
* Hej **azure datalake analytics** modulen innehåller hello Azure Data Lake Analytics-åtgärder. 

Kontrollera först att du har hello senaste `pip` genom att köra följande kommando hello:

```
python -m pip install --upgrade pip
```

Det här dokumentet har skrivits `pip version 9.0.1`.

Använder följande hello `pip` kommandon tooinstall hello moduler från hello kommandorad:

```
pip install azure-mgmt-resource
pip install azure-datalake-store
pip install azure-mgmt-datalake-store
pip install azure-mgmt-datalake-analytics
```

## <a name="create-a-new-python-script"></a>Skapa ett nytt Python-skript

Klistra in följande kod i hello skript hello:

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

Kör det här skriptet tooverify som hello moduler kan importeras.

## <a name="authentication"></a>Autentisering

### <a name="interactive-user-authentication-with-a-pop-up"></a>Interaktiv användarautentisering med ett popup-fönster

Den här metoden stöds inte.

### <a name="interactive-user-authentication-with-a-device-code"></a>Interaktiv användarautentisering med en kod för enheten

```python
user = input('Enter hello user tooauthenticate with that has permission toosubscription: ')
password = getpass.getpass()
credentials = UserPassCredentials(user, password)
```

### <a name="noninteractive-authentication-with-spi-and-a-secret"></a>Icke-interaktiv autentisering med SPI och en hemlighet

```python
credentials = ServicePrincipalCredentials(client_id = 'FILL-IN-HERE', secret = 'FILL-IN-HERE', tenant = 'FILL-IN-HERE')
```

### <a name="noninteractive-authentication-with-api-and-a-certificate"></a>Icke-interaktiv autentisering med API: et och ett certifikat

Den här metoden stöds inte.

## <a name="common-script-variables"></a>Vanliga skriptvariabler

Dessa variabler används i hello prover.

```python
subid= '<Azure Subscription ID>'
rg = '<Azure Resource Group Name>'
location = '<Location>' # i.e. 'eastus2'
adls = '<Azure Data Lake Store Account Name>'
adla = '<Azure Data Lake Analytics Account Name>'
```

## <a name="create-hello-clients"></a>Skapa hello klienter

```python
resourceClient = ResourceManagementClient(credentials, subid)
adlaAcctClient = DataLakeAnalyticsAccountManagementClient(credentials, subid)
adlaJobClient = DataLakeAnalyticsJobManagementClient( credentials, 'azuredatalakeanalytics.net')
```

## <a name="create-an-azure-resource-group"></a>Skapa en Azure-resursgrupp

```python
armGroupResult = resourceClient.resource_groups.create_or_update( rg, ResourceGroup( location=location ) )
```

## <a name="create-data-lake-analytics-account"></a>Skapa ett Data Lake Analytics-konto

Skapa först en store-konto.

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
Skapa sedan en ADLA-konto som använder detta Arkiv.

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

## <a name="submit-a-job"></a>Skicka ett jobb

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

## <a name="wait-for-a-job-tooend"></a>Vänta tills en jobbet tooend

```python
jobResult = adlaJobClient.job.get(adla, jobId)
while(jobResult.state != JobState.ended):
    print('Job is not yet done, waiting for 3 seconds. Current state: ' + jobResult.state.value)
    time.sleep(3)
    jobResult = adlaJobClient.job.get(adla, jobId)

print ('Job finished with result: ' + jobResult.result.value)
```

## <a name="list-pipelines-and-recurrences"></a>Lista pipelines och repetitioner
Beroende på om dina jobb har pipeline eller återkommande metadata kopplade kan visa du pipelines och repetitioner.

```python
pipelines = adlaJobClient.pipeline.list(adla)
for p in pipelines:
    print('Pipeline: ' + p.name + ' ' + p.pipelineId)

recurrences = adlaJobClient.recurrence.list(adla)
for r in recurrences:
    print('Recurrence: ' + r.name + ' ' + r.recurrenceId)
```

## <a name="manage-compute-policies"></a>Hantera principer för beräkning

Hej DataLakeAnalyticsAccountManagementClient objektet innehåller metoder för att hantera hello compute principer för ett Data Lake Analytics-konto.

### <a name="list-compute-policies"></a>Lista principer för beräkning

hello följande kod hämtar en lista över beräkning principer för ett Data Lake Analytics-konto.

```python
policies = adlaAccountClient.computePolicies.listByAccount(rg, adla)
for p in policies:
    print('Name: ' + p.name + 'Type: ' + p.objectType + 'Max AUs / job: ' + p.maxDegreeOfParallelismPerJob + 'Min priority / job: ' + p.minPriorityPerJob)
```

### <a name="create-a-new-compute-policy"></a>Skapa en ny princip för beräkning

hello följande kod skapar en ny princip för beräkning för ett Data Lake Analytics-konto, inställningen hello maximala Australien tillgängliga toohello anges användaren too50 och hello minsta jobbet prioritet too250.

```python
userAadObjectId = "3b097601-4912-4d41-b9d2-78672fc2acde"
newPolicyParams = ComputePolicyCreateOrUpdateParameters(userAadObjectId, "User", 50, 250)
adlaAccountClient.computePolicies.createOrUpdate(rg, adla, "GaryMcDaniel", newPolicyParams)
```

## <a name="next-steps"></a>Nästa steg

- toosee hello samma självstudier med andra verktyg klickar du på hello flikväljarna överst hello hello-sidan.
- toolearn U-SQL finns [Kom igång med Azure Data Lake Analytics U-SQL-språket](data-lake-analytics-u-sql-get-started.md).
- Information om hanteringsuppgifter finns i [Hantera Azure Data Lake Analytics med hjälp av Azure Portal](data-lake-analytics-manage-use-portal.md).

