---
title: "aaaScheduler utgående autentisering"
description: "Schemaläggaren utgående autentisering"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 6707f82b-7e32-401b-a960-02aae7bb59cc
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/15/2016
ms.author: deli
ms.openlocfilehash: ef713f4770b48d0a9176415e87c1042a823582e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scheduler-outbound-authentication"></a>Schemaläggaren utgående autentisering
Schemaläggare behöva toocall ut tooservices som kräver autentisering. På så sätt kan en kallas tjänsten kan avgöra om hello jobb i Schemaläggaren kan komma åt sina resurser. Dessa tjänster bland andra Azure-tjänster, Salesforce.com, Facebook och säker anpassade webbplatser.

## <a name="adding-and-removing-authentication"></a>Lägga till och ta bort autentisering
Lägga till autentisering tooa jobb i Schemaläggaren är enkel – Lägg till ett underordnat element JSON `authentication` toohello `request` elementet när du skapar eller uppdaterar ett jobb. Hemligheter som en del av hello skickades toohello Schemaläggaren i en PUT, KORRIGERINGSFIL eller POST-begäran – `authentication` objekt – aldrig returneras i svar. Hemlig information anges toonull i svar eller kan ha en token för offentlig som representerar hello autentiserad entitet.

tooremove autentisering, SPÄRRA eller KORRIGERA hello jobbet explicit ange hello `authentication` objekt toonull. Alla egenskaper för autentisering av tillbaka som svar visas inte.

Hello stöds endast autentiseringstyper är för närvarande hello `ClientCertificate` modell (för att använda klientcertifikat för hello SSL/TLS), hello `Basic` modell (för grundläggande autentisering) och hello `ActiveDirectoryOAuth` modellen (för Active Directory-OAuth autentisering.)

## <a name="request-body-for-clientcertificate-authentication"></a>Begäran för ClientCertificate autentisering
När du lägger till autentisering med hjälp av hello `ClientCertificate` modellen, ange följande ytterligare element i begärandetexten för hello hello.  

| Element | Beskrivning |
|:--- |:--- |
| *autentisering (överordnat element)* |Autentiseringsobjekt för att använda ett SSL-klientcertifikat. |
| *typ* |Krävs. Typ av autentisering. För SSL-klientcertifikat, hello-värdet måste vara `ClientCertificate`. |
| *pfx* |Krävs. Base64-kodade innehåll hello PFX-filen. |
| *lösenord* |Krävs. Lösenordet tooaccess hello PFX-filen. |

## <a name="response-body-for-clientcertificate-authentication"></a>Svarstexten för ClientCertificate autentisering
När en begäran skickas med autentisering information innehåller hello svaret hello efter autentisering-relaterade element.

| Element | Beskrivning |
|:--- |:--- |
| *autentisering (överordnat element)* |Autentiseringsobjekt för att använda ett SSL-klientcertifikat. |
| *typ* |Typ av autentisering. Hello-värdet är för SSL-klientcertifikat, `ClientCertificate`. |
| *certificateThumbprint* |hello tumavtrycket för certifikatet för hello. |
| *certificateSubjectName* |hello unika ämnesnamnet för certifikatet hello. |
| *certificateExpiration* |hello utgångsdatum för hello certifikat. |

## <a name="sample-rest-request-for-clientcertificate-authentication"></a>Exempel på REST-begäran för ClientCertificate autentisering
```
PUT https://management.azure.com/subscriptions/1fe0abdf-581e-4dfe-9ec7-e5cb8e7b205e/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "clientcertificate",
          "password": "password",
          "pfx": "pfx key"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-clientcertificate-authentication"></a>RESTEN av Exempelsvar för ClientCertificate autentisering
```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 858
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 56c7b40e-721a-437e-88e6-f68562a73aa8
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 1075219e-e879-4030-bc81-094e54fbabce
x-ms-routing-request-id: WESTUS:20160316T190424Z:1075219e-e879-4030-bc81-094e54fbabce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:04:23 GMT

{
  "id": "/subscriptions/1fe0abdf-581e-4dfe-9ec7-e5cb8e7b205e/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
  "type": "Microsoft.Scheduler/jobCollections/jobs",
  "name": "southeastasiajc/httpjob",
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "certificateThumbprint": "88105CG9DF9ADE75B835711D899296CB217D7055",
          "certificateExpiration": "2021-01-01T07:00:00Z",
          "certificateSubjectName": "CN=Scheduler Mgmt",
          "type": "ClientCertificate"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
    "status": {
      "nextExecutionTime": "2016-03-16T19:05:00Z",
      "executionCount": 0,
      "failureCount": 0,
      "faultedCount": 0
    }
  }
}
```

## <a name="request-body-for-basic-authentication"></a>Begärandetexten för grundläggande autentisering
När du lägger till autentisering med hjälp av hello `Basic` modellen, ange följande ytterligare element i begärandetexten för hello hello.

| Element | Beskrivning |
|:--- |:--- |
| *autentisering (överordnat element)* |Autentiseringsobjekt för att använda grundläggande autentisering. |
| *typ* |Krävs. Typ av autentisering. För grundläggande autentisering hello-värdet måste vara `Basic`. |
| *användarnamn* |Krävs. Användarnamnet tooauthenticate. |
| *lösenord* |Krävs. Lösenordet tooauthenticate. |

## <a name="response-body-for-basic-authentication"></a>Svarstexten för grundläggande autentisering
När en begäran skickas med autentisering information innehåller hello svaret hello efter autentisering-relaterade element.

| Element | Beskrivning |
|:--- |:--- |
| *autentisering (överordnat element)* |Autentiseringsobjekt för att använda grundläggande autentisering. |
| *typ* |Typ av autentisering. Hello-värdet är för grundläggande autentisering, `Basic`. |
| *användarnamn* |hello autentisera användarnamnet. |

## <a name="sample-rest-request-for-basic-authentication"></a>Exempel på REST-begäran för grundläggande autentisering
```
PUT https://management.azure.com/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 562
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "basic",
          "username": "user",
          "password": "password"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-basic-authentication"></a>RESTEN av Exempelsvar för grundläggande autentisering
```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 701
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: a2dcb9cd-1aea-4887-8893-d81273a8cf04
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 7816f222-6ea7-468d-b919-e6ddebbd7e95
x-ms-routing-request-id: WESTUS:20160316T190506Z:7816f222-6ea7-468d-b919-e6ddebbd7e95
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:05:06 GMT

{  
   "id":"/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type":"Microsoft.Scheduler/jobCollections/jobs",
   "name":"southeastasiajc/httpjob",
   "properties":{  
      "startTime":"2015-05-14T14:10:00Z",
      "action":{  
         "request":{  
            "uri":"https://mywebserviceendpoint.com",
            "method":"GET",
            "headers":{  
               "x-ms-version":"2013-03-01"
            },
            "authentication":{  
               "username":"user1",
               "type":"Basic"
            }
         },
         "type":"http"
      },
      "recurrence":{  
         "frequency":"minute",
         "endTime":"2016-04-10T08:00:00Z",
         "interval":1
      },
      "state":"enabled",
      "status":{  
         "nextExecutionTime":"2016-03-16T19:06:00Z",
         "executionCount":0,
         "failureCount":0,
         "faultedCount":0
      }
   }
}
```

## <a name="request-body-for-activedirectoryoauth-authentication"></a>Begäran för ActiveDirectoryOAuth autentisering
När du lägger till autentisering med hjälp av hello `ActiveDirectoryOAuth` modellen, ange följande ytterligare element i begärandetexten för hello hello.

| Element | Beskrivning |
|:--- |:--- |
| *autentisering (överordnat element)* |Autentiseringsobjekt för att använda ActiveDirectoryOAuth autentisering. |
| *typ* |Krävs. Typ av autentisering. För ActiveDirectoryOAuth autentisering hello-värdet måste vara `ActiveDirectoryOAuth`. |
| *klient* |Krävs. hello klient-ID för hello Azure AD-klient. |
| *målgrupp* |Krävs. Detta är inställt toohttps://management.core.windows.net/. |
| *clientId* |Krävs. Ange hello klient-ID för hello Azure AD-program. |
| *hemligt* |Krävs. Hemligheten för hello-klient som begär hello-token. |

### <a name="determining-your-tenant-identifier"></a>Fastställa din klient-ID.
Du kan hitta hello klient-ID för hello Azure AD-klienten genom att köra `Get-AzureAccount` i Azure PowerShell.

## <a name="response-body-for-activedirectoryoauth-authentication"></a>Svarstexten för ActiveDirectoryOAuth autentisering
När en begäran skickas med autentisering information innehåller hello svaret hello efter autentisering-relaterade element.

| Element | Beskrivning |
|:--- |:--- |
| *autentisering (överordnat element)* |Autentiseringsobjekt för att använda ActiveDirectoryOAuth autentisering. |
| *typ* |Typ av autentisering. Hello-värdet är för ActiveDirectoryOAuth autentisering `ActiveDirectoryOAuth`. |
| *klient* |hello klient-ID för hello Azure AD-klient. |
| *målgrupp* |Detta är inställt toohttps://management.core.windows.net/. |
| *clientId* |Hej klient-ID för hello Azure AD-program. |

## <a name="sample-rest-request-for-activedirectoryoauth-authentication"></a>Exempel på REST-begäran för ActiveDirectoryOAuth autentisering
```
PUT https://management.azure.com/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 757
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "tenant":"microsoft.onmicrosoft.com",
          "audience":"https://management.core.windows.net/",
          "clientId":"dc23e764-9be6-4a33-9b9a-c46e36f0c137",
          "secret": "G6u071r8Gjw4V4KSibnb+VK4+tX399hkHaj7LOyHuj5=",
          "type":"ActiveDirectoryOAuth"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-activedirectoryoauth-authentication"></a>RESTEN av Exempelsvar för ActiveDirectoryOAuth autentisering
```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 885
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 86d8e9fd-ac0d-4bed-9420-9baba1af3251
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
x-ms-routing-request-id: WESTUS:20160316T191003Z:5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:10:02 GMT

{  
   "id":"/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type":"Microsoft.Scheduler/jobCollections/jobs",
   "name":"southeastasiajc/httpjob",
   "properties":{  
      "startTime":"2015-05-14T14:10:00Z",
      "action":{  
         "request":{  
            "uri":"https://mywebserviceendpoint.com",
            "method":"GET",
            "headers":{  
               "x-ms-version":"2013-03-01"
            },
            "authentication":{  
               "tenant":"microsoft.onmicrosoft.com",
               "audience":"https://management.core.windows.net/",
               "clientId":"dc23e764-9be6-4a33-9b9a-c46e36f0c137",
               "type":"ActiveDirectoryOAuth"
            }
         },
         "type":"http"
      },
      "recurrence":{  
         "frequency":"minute",
         "endTime":"2016-04-10T08:00:00Z",
         "interval":1
      },
      "state":"enabled",
      "status":{  
         "lastExecutionTime":"2016-03-16T19:10:00.3762123Z",
         "nextExecutionTime":"2016-03-16T19:11:00Z",
         "executionCount":5,
         "failureCount":5,
         "faultedCount":1
      }
   }
}
```

## <a name="see-also"></a>Se även
 [Vad är Scheduler?](scheduler-intro.md)

 [Begrepp, terminologi och entitetshierarki relaterade till Azure Scheduler](scheduler-concepts-terms.md)

 [Komma igång med Schemaläggaren i hello Azure-portalen](scheduler-get-started-portal.md)

 [Prenumerationer och fakturering i Azure Scheduler](scheduler-plans-billing.md)

 [Referens för REST-API:et för Azure Scheduler](https://msdn.microsoft.com/library/mt629143)

 [Referens för PowerShell-cmdlets för Azure Scheduler](scheduler-powershell-reference.md)

 [Hög tillgänglighet och tillförlitlighet i Azure Scheduler](scheduler-high-availability-reliability.md)

 [Gränser, standardinställningar och felkoder i Azure Scheduler](scheduler-limits-defaults-errors.md)

