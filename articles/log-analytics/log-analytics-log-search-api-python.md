---
title: "aaaPython skript tooretrieve data från Azure Log Analytics | Microsoft Docs"
description: "hello Log Analytics loggen Sök-API kan REST API-klient tooretrieve data från en logganalys-arbetsytan.  Den här artikeln innehåller ett exempelskript för Python med hello loggen Sök-API."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/28/2017
ms.author: bwren
ms.openlocfilehash: a45693b04cd388301b859e7186ca671786d0229e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="retrieve-data-from-log-analytics-with-a-python-script"></a>Hämta data från logganalys med Python-skriptet
Hej [Log Analytics loggen Sök API](log-analytics-log-search-api.md) gör REST API-klient tooretrieve data från en logganalys-arbetsytan.  Den här artikeln beskriver en Python-skriptet som använder hello Log Analytics loggen Sök-API.  

## <a name="authentication"></a>Autentisering
Det här skriptet använder ett huvudnamn för tjänsten i Azure Active Directory tooauthenticate toohello arbetsyta.  Tjänstens huvudnamn att en klient programmet toorequest som hello service autentisera ett konto även om hello-klienten inte har hello kontonamn. Innan du kör skriptet måste du skapa ett huvudnamn för tjänsten med hjälp av hello process på [använda portalen toocreate ett Azure Active Directory-program och tjänstens huvudnamn som kan komma åt resurser](../azure-resource-manager/resource-group-create-service-principal-portal.md).  Du behöver tooprovide hello program-ID, klient-ID och autentiseringsnyckel toohello skript. 

> [!NOTE]
> När du [skapa ett Azure Automation-konto](../automation/automation-create-standalone-account.md), ett huvudnamn för tjänsten skapas som är lämplig toouse med det här skriptet.  Om du redan har ett huvudnamn för tjänsten som skapats av Azure Automation så bör du kunna toouse den i stället för att skapa en ny, även om du behöver för[skapa en autentiseringsnyckel](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key) om den inte redan har en.

## <a name="script"></a>Skript
``` python
import adal
import requests
import json
import datetime
from pprint import pprint

# Details of workspace.  Fill in details for your workspace.
resource_group = 'xxxxxxxx'
workspace = 'xxxxxxxx'

# Details of query.  Modify these tooyour requirements.
query = "Type=Event"
end_time = datetime.datetime.utcnow()
start_time = end_time - datetime.timedelta(hours=24)
num_results = 100  # If not provided, a default of 10 results will be used.

# IDs for authentication.  Fill in values for your service principal.
subscription_id = 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'
tenant_id = 'xxxxxxxx-xxxx-xxxx-xxx-xxxxxxxxxxxx'
application_id = 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx'
application_key = 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'

# URLs for authentication
authentication_endpoint = 'https://login.microsoftonline.com/'
resource  = 'https://management.core.windows.net/'

# Get access token
context = adal.AuthenticationContext('https://login.microsoftonline.com/' + tenant_id)
token_response = context.acquire_token_with_client_credentials('https://management.core.windows.net/', application_id, application_key)
access_token = token_response.get('accessToken')

# Add token tooheader
headers = {
    "Authorization": 'Bearer ' + access_token,
    "Content-Type":'application/json'
}

# URLs for retrieving data
uri_base = 'https://management.azure.com'
uri_api = 'api-version=2015-11-01-preview'
uri_subscription = 'https://management.azure.com/subscriptions/' + subscription_id
uri_resourcegroup = uri_subscription + '/resourcegroups/'+ resource_group
uri_workspace = uri_resourcegroup + '/providers/Microsoft.OperationalInsights/workspaces/' + workspace
uri_search = uri_workspace + '/search'

# Build search parameters from query details
search_params = {
        "query": query,
        "top": num_results,
        "start": start_time.strftime('%Y-%m-%dT%H:%M:%S'),
        "end": end_time.strftime('%Y-%m-%dT%H:%M:%S')
        }

# Build URL and send post request
uri = uri_search + '?' + uri_api
response = requests.post(uri,json=search_params,headers=headers)

# Response of 200 if successful
if response.status_code == 200:

    # Parse hello response tooget hello ID and status
    data = response.json()
    search_id = data["id"].split("/")
    id = search_id[len(search_id)-1]
    status = data["__metadata"]["Status"]

    # If status is pending, then keep checking until complete
    while status == "Pending":

        # Build URL tooget search from ID and send request
        uri_search = uri_search + '/' + id
        uri = uri_search + '?' + uri_api
        response = requests.get(uri,headers=headers)

        # Parse hello response tooget hello status
        data = response.json()
        status = data["__metadata"]["Status"]

else:

    # Request failed
    print (response.status_code)
    response.raise_for_status()

print ("Total records:" + str(data["__metadata"]["total"]))
print ("Returned top:" + str(data["__metadata"]["top"]))
pprint (data["value"])
```
## <a name="next-steps"></a>Nästa steg
- Mer information om hello [Log Analytics loggen Sök API](log-analytics-log-search-api.md).
