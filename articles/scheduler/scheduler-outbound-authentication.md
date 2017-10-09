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
# <a name="scheduler-outbound-authentication"></a><span data-ttu-id="fe60c-103">Schemaläggaren utgående autentisering</span><span class="sxs-lookup"><span data-stu-id="fe60c-103">Scheduler Outbound Authentication</span></span>
<span data-ttu-id="fe60c-104">Schemaläggare behöva toocall ut tooservices som kräver autentisering.</span><span class="sxs-lookup"><span data-stu-id="fe60c-104">Scheduler jobs may need toocall out tooservices that require authentication.</span></span> <span data-ttu-id="fe60c-105">På så sätt kan en kallas tjänsten kan avgöra om hello jobb i Schemaläggaren kan komma åt sina resurser.</span><span class="sxs-lookup"><span data-stu-id="fe60c-105">This way, a called service can determine if hello Scheduler job can access its resources.</span></span> <span data-ttu-id="fe60c-106">Dessa tjänster bland andra Azure-tjänster, Salesforce.com, Facebook och säker anpassade webbplatser.</span><span class="sxs-lookup"><span data-stu-id="fe60c-106">Some of these services include other Azure services, Salesforce.com, Facebook, and secure custom websites.</span></span>

## <a name="adding-and-removing-authentication"></a><span data-ttu-id="fe60c-107">Lägga till och ta bort autentisering</span><span class="sxs-lookup"><span data-stu-id="fe60c-107">Adding and Removing Authentication</span></span>
<span data-ttu-id="fe60c-108">Lägga till autentisering tooa jobb i Schemaläggaren är enkel – Lägg till ett underordnat element JSON `authentication` toohello `request` elementet när du skapar eller uppdaterar ett jobb.</span><span class="sxs-lookup"><span data-stu-id="fe60c-108">Adding authentication tooa Scheduler job is simple – add a JSON child element `authentication` toohello `request` element when creating or updating a job.</span></span> <span data-ttu-id="fe60c-109">Hemligheter som en del av hello skickades toohello Schemaläggaren i en PUT, KORRIGERINGSFIL eller POST-begäran – `authentication` objekt – aldrig returneras i svar.</span><span class="sxs-lookup"><span data-stu-id="fe60c-109">Secrets passed toohello Scheduler service in a PUT, PATCH, or POST request – as part of hello `authentication` object – are never returned in responses.</span></span> <span data-ttu-id="fe60c-110">Hemlig information anges toonull i svar eller kan ha en token för offentlig som representerar hello autentiserad entitet.</span><span class="sxs-lookup"><span data-stu-id="fe60c-110">In responses, secret information is set toonull or may have a public token that represents hello authenticated entity.</span></span>

<span data-ttu-id="fe60c-111">tooremove autentisering, SPÄRRA eller KORRIGERA hello jobbet explicit ange hello `authentication` objekt toonull.</span><span class="sxs-lookup"><span data-stu-id="fe60c-111">tooremove authentication, PUT or PATCH hello job explicitly, setting hello `authentication` object toonull.</span></span> <span data-ttu-id="fe60c-112">Alla egenskaper för autentisering av tillbaka som svar visas inte.</span><span class="sxs-lookup"><span data-stu-id="fe60c-112">You will not see any authentication properties back in response.</span></span>

<span data-ttu-id="fe60c-113">Hello stöds endast autentiseringstyper är för närvarande hello `ClientCertificate` modell (för att använda klientcertifikat för hello SSL/TLS), hello `Basic` modell (för grundläggande autentisering) och hello `ActiveDirectoryOAuth` modellen (för Active Directory-OAuth autentisering.)</span><span class="sxs-lookup"><span data-stu-id="fe60c-113">Currently, hello only supported authentication types are hello `ClientCertificate` model (for using hello SSL/TLS client certificates), hello `Basic` model (for Basic authentication), and hello `ActiveDirectoryOAuth` model (for Active Directory OAuth authentication.)</span></span>

## <a name="request-body-for-clientcertificate-authentication"></a><span data-ttu-id="fe60c-114">Begäran för ClientCertificate autentisering</span><span class="sxs-lookup"><span data-stu-id="fe60c-114">Request Body for ClientCertificate Authentication</span></span>
<span data-ttu-id="fe60c-115">När du lägger till autentisering med hjälp av hello `ClientCertificate` modellen, ange följande ytterligare element i begärandetexten för hello hello.</span><span class="sxs-lookup"><span data-stu-id="fe60c-115">When adding authentication using hello `ClientCertificate` model, specify hello following additional elements in hello request body.</span></span>  

| <span data-ttu-id="fe60c-116">Element</span><span class="sxs-lookup"><span data-stu-id="fe60c-116">Element</span></span> | <span data-ttu-id="fe60c-117">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fe60c-117">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="fe60c-118">*autentisering (överordnat element)*</span><span class="sxs-lookup"><span data-stu-id="fe60c-118">*authentication (parent element)*</span></span> |<span data-ttu-id="fe60c-119">Autentiseringsobjekt för att använda ett SSL-klientcertifikat.</span><span class="sxs-lookup"><span data-stu-id="fe60c-119">Authentication object for using an SSL client certificate.</span></span> |
| <span data-ttu-id="fe60c-120">*typ*</span><span class="sxs-lookup"><span data-stu-id="fe60c-120">*type*</span></span> |<span data-ttu-id="fe60c-121">Krävs.</span><span class="sxs-lookup"><span data-stu-id="fe60c-121">Required.</span></span> <span data-ttu-id="fe60c-122">Typ av autentisering. För SSL-klientcertifikat, hello-värdet måste vara `ClientCertificate`.</span><span class="sxs-lookup"><span data-stu-id="fe60c-122">Type of authentication.For SSL client certificates, hello value must be `ClientCertificate`.</span></span> |
| <span data-ttu-id="fe60c-123">*pfx*</span><span class="sxs-lookup"><span data-stu-id="fe60c-123">*pfx*</span></span> |<span data-ttu-id="fe60c-124">Krävs.</span><span class="sxs-lookup"><span data-stu-id="fe60c-124">Required.</span></span> <span data-ttu-id="fe60c-125">Base64-kodade innehåll hello PFX-filen.</span><span class="sxs-lookup"><span data-stu-id="fe60c-125">Base64-encoded contents of hello PFX file.</span></span> |
| <span data-ttu-id="fe60c-126">*lösenord*</span><span class="sxs-lookup"><span data-stu-id="fe60c-126">*password*</span></span> |<span data-ttu-id="fe60c-127">Krävs.</span><span class="sxs-lookup"><span data-stu-id="fe60c-127">Required.</span></span> <span data-ttu-id="fe60c-128">Lösenordet tooaccess hello PFX-filen.</span><span class="sxs-lookup"><span data-stu-id="fe60c-128">Password tooaccess hello PFX file.</span></span> |

## <a name="response-body-for-clientcertificate-authentication"></a><span data-ttu-id="fe60c-129">Svarstexten för ClientCertificate autentisering</span><span class="sxs-lookup"><span data-stu-id="fe60c-129">Response Body for ClientCertificate Authentication</span></span>
<span data-ttu-id="fe60c-130">När en begäran skickas med autentisering information innehåller hello svaret hello efter autentisering-relaterade element.</span><span class="sxs-lookup"><span data-stu-id="fe60c-130">When a request is sent with authentication info, hello response contains hello following authentication-related elements.</span></span>

| <span data-ttu-id="fe60c-131">Element</span><span class="sxs-lookup"><span data-stu-id="fe60c-131">Element</span></span> | <span data-ttu-id="fe60c-132">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fe60c-132">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="fe60c-133">*autentisering (överordnat element)*</span><span class="sxs-lookup"><span data-stu-id="fe60c-133">*authentication (parent element)*</span></span> |<span data-ttu-id="fe60c-134">Autentiseringsobjekt för att använda ett SSL-klientcertifikat.</span><span class="sxs-lookup"><span data-stu-id="fe60c-134">Authentication object for using an SSL client certificate.</span></span> |
| <span data-ttu-id="fe60c-135">*typ*</span><span class="sxs-lookup"><span data-stu-id="fe60c-135">*type*</span></span> |<span data-ttu-id="fe60c-136">Typ av autentisering.</span><span class="sxs-lookup"><span data-stu-id="fe60c-136">Type of authentication.</span></span> <span data-ttu-id="fe60c-137">Hello-värdet är för SSL-klientcertifikat, `ClientCertificate`.</span><span class="sxs-lookup"><span data-stu-id="fe60c-137">For SSL client certificates, hello value is `ClientCertificate`.</span></span> |
| <span data-ttu-id="fe60c-138">*certificateThumbprint*</span><span class="sxs-lookup"><span data-stu-id="fe60c-138">*certificateThumbprint*</span></span> |<span data-ttu-id="fe60c-139">hello tumavtrycket för certifikatet för hello.</span><span class="sxs-lookup"><span data-stu-id="fe60c-139">hello thumbprint of hello certificate.</span></span> |
| <span data-ttu-id="fe60c-140">*certificateSubjectName*</span><span class="sxs-lookup"><span data-stu-id="fe60c-140">*certificateSubjectName*</span></span> |<span data-ttu-id="fe60c-141">hello unika ämnesnamnet för certifikatet hello.</span><span class="sxs-lookup"><span data-stu-id="fe60c-141">hello subject distinguished name of hello certificate.</span></span> |
| <span data-ttu-id="fe60c-142">*certificateExpiration*</span><span class="sxs-lookup"><span data-stu-id="fe60c-142">*certificateExpiration*</span></span> |<span data-ttu-id="fe60c-143">hello utgångsdatum för hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="fe60c-143">hello expiration date of hello certificate.</span></span> |

## <a name="sample-rest-request-for-clientcertificate-authentication"></a><span data-ttu-id="fe60c-144">Exempel på REST-begäran för ClientCertificate autentisering</span><span class="sxs-lookup"><span data-stu-id="fe60c-144">Sample REST Request for ClientCertificate Authentication</span></span>
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

## <a name="sample-rest-response-for-clientcertificate-authentication"></a><span data-ttu-id="fe60c-145">RESTEN av Exempelsvar för ClientCertificate autentisering</span><span class="sxs-lookup"><span data-stu-id="fe60c-145">Sample REST Response for ClientCertificate Authentication</span></span>
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

## <a name="request-body-for-basic-authentication"></a><span data-ttu-id="fe60c-146">Begärandetexten för grundläggande autentisering</span><span class="sxs-lookup"><span data-stu-id="fe60c-146">Request Body for Basic Authentication</span></span>
<span data-ttu-id="fe60c-147">När du lägger till autentisering med hjälp av hello `Basic` modellen, ange följande ytterligare element i begärandetexten för hello hello.</span><span class="sxs-lookup"><span data-stu-id="fe60c-147">When adding authentication using hello `Basic` model, specify hello following additional elements in hello request body.</span></span>

| <span data-ttu-id="fe60c-148">Element</span><span class="sxs-lookup"><span data-stu-id="fe60c-148">Element</span></span> | <span data-ttu-id="fe60c-149">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fe60c-149">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="fe60c-150">*autentisering (överordnat element)*</span><span class="sxs-lookup"><span data-stu-id="fe60c-150">*authentication (parent element)*</span></span> |<span data-ttu-id="fe60c-151">Autentiseringsobjekt för att använda grundläggande autentisering.</span><span class="sxs-lookup"><span data-stu-id="fe60c-151">Authentication object for using Basic authentication.</span></span> |
| <span data-ttu-id="fe60c-152">*typ*</span><span class="sxs-lookup"><span data-stu-id="fe60c-152">*type*</span></span> |<span data-ttu-id="fe60c-153">Krävs.</span><span class="sxs-lookup"><span data-stu-id="fe60c-153">Required.</span></span> <span data-ttu-id="fe60c-154">Typ av autentisering.</span><span class="sxs-lookup"><span data-stu-id="fe60c-154">Type of authentication.</span></span> <span data-ttu-id="fe60c-155">För grundläggande autentisering hello-värdet måste vara `Basic`.</span><span class="sxs-lookup"><span data-stu-id="fe60c-155">For Basic authentication, hello value must be `Basic`.</span></span> |
| <span data-ttu-id="fe60c-156">*användarnamn*</span><span class="sxs-lookup"><span data-stu-id="fe60c-156">*username*</span></span> |<span data-ttu-id="fe60c-157">Krävs.</span><span class="sxs-lookup"><span data-stu-id="fe60c-157">Required.</span></span> <span data-ttu-id="fe60c-158">Användarnamnet tooauthenticate.</span><span class="sxs-lookup"><span data-stu-id="fe60c-158">Username tooauthenticate.</span></span> |
| <span data-ttu-id="fe60c-159">*lösenord*</span><span class="sxs-lookup"><span data-stu-id="fe60c-159">*password*</span></span> |<span data-ttu-id="fe60c-160">Krävs.</span><span class="sxs-lookup"><span data-stu-id="fe60c-160">Required.</span></span> <span data-ttu-id="fe60c-161">Lösenordet tooauthenticate.</span><span class="sxs-lookup"><span data-stu-id="fe60c-161">Password tooauthenticate.</span></span> |

## <a name="response-body-for-basic-authentication"></a><span data-ttu-id="fe60c-162">Svarstexten för grundläggande autentisering</span><span class="sxs-lookup"><span data-stu-id="fe60c-162">Response Body for Basic Authentication</span></span>
<span data-ttu-id="fe60c-163">När en begäran skickas med autentisering information innehåller hello svaret hello efter autentisering-relaterade element.</span><span class="sxs-lookup"><span data-stu-id="fe60c-163">When a request is sent with authentication info, hello response contains hello following authentication-related elements.</span></span>

| <span data-ttu-id="fe60c-164">Element</span><span class="sxs-lookup"><span data-stu-id="fe60c-164">Element</span></span> | <span data-ttu-id="fe60c-165">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fe60c-165">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="fe60c-166">*autentisering (överordnat element)*</span><span class="sxs-lookup"><span data-stu-id="fe60c-166">*authentication (parent element)*</span></span> |<span data-ttu-id="fe60c-167">Autentiseringsobjekt för att använda grundläggande autentisering.</span><span class="sxs-lookup"><span data-stu-id="fe60c-167">Authentication object for using Basic authentication.</span></span> |
| <span data-ttu-id="fe60c-168">*typ*</span><span class="sxs-lookup"><span data-stu-id="fe60c-168">*type*</span></span> |<span data-ttu-id="fe60c-169">Typ av autentisering.</span><span class="sxs-lookup"><span data-stu-id="fe60c-169">Type of authentication.</span></span> <span data-ttu-id="fe60c-170">Hello-värdet är för grundläggande autentisering, `Basic`.</span><span class="sxs-lookup"><span data-stu-id="fe60c-170">For Basic authentication, hello value is `Basic`.</span></span> |
| <span data-ttu-id="fe60c-171">*användarnamn*</span><span class="sxs-lookup"><span data-stu-id="fe60c-171">*username*</span></span> |<span data-ttu-id="fe60c-172">hello autentisera användarnamnet.</span><span class="sxs-lookup"><span data-stu-id="fe60c-172">hello authenticated username.</span></span> |

## <a name="sample-rest-request-for-basic-authentication"></a><span data-ttu-id="fe60c-173">Exempel på REST-begäran för grundläggande autentisering</span><span class="sxs-lookup"><span data-stu-id="fe60c-173">Sample REST Request for Basic Authentication</span></span>
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

## <a name="sample-rest-response-for-basic-authentication"></a><span data-ttu-id="fe60c-174">RESTEN av Exempelsvar för grundläggande autentisering</span><span class="sxs-lookup"><span data-stu-id="fe60c-174">Sample REST Response for Basic Authentication</span></span>
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

## <a name="request-body-for-activedirectoryoauth-authentication"></a><span data-ttu-id="fe60c-175">Begäran för ActiveDirectoryOAuth autentisering</span><span class="sxs-lookup"><span data-stu-id="fe60c-175">Request Body for ActiveDirectoryOAuth Authentication</span></span>
<span data-ttu-id="fe60c-176">När du lägger till autentisering med hjälp av hello `ActiveDirectoryOAuth` modellen, ange följande ytterligare element i begärandetexten för hello hello.</span><span class="sxs-lookup"><span data-stu-id="fe60c-176">When adding authentication using hello `ActiveDirectoryOAuth` model, specify hello following additional elements in hello request body.</span></span>

| <span data-ttu-id="fe60c-177">Element</span><span class="sxs-lookup"><span data-stu-id="fe60c-177">Element</span></span> | <span data-ttu-id="fe60c-178">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fe60c-178">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="fe60c-179">*autentisering (överordnat element)*</span><span class="sxs-lookup"><span data-stu-id="fe60c-179">*authentication (parent element)*</span></span> |<span data-ttu-id="fe60c-180">Autentiseringsobjekt för att använda ActiveDirectoryOAuth autentisering.</span><span class="sxs-lookup"><span data-stu-id="fe60c-180">Authentication object for using ActiveDirectoryOAuth authentication.</span></span> |
| <span data-ttu-id="fe60c-181">*typ*</span><span class="sxs-lookup"><span data-stu-id="fe60c-181">*type*</span></span> |<span data-ttu-id="fe60c-182">Krävs.</span><span class="sxs-lookup"><span data-stu-id="fe60c-182">Required.</span></span> <span data-ttu-id="fe60c-183">Typ av autentisering.</span><span class="sxs-lookup"><span data-stu-id="fe60c-183">Type of authentication.</span></span> <span data-ttu-id="fe60c-184">För ActiveDirectoryOAuth autentisering hello-värdet måste vara `ActiveDirectoryOAuth`.</span><span class="sxs-lookup"><span data-stu-id="fe60c-184">For ActiveDirectoryOAuth authentication, hello value must be `ActiveDirectoryOAuth`.</span></span> |
| <span data-ttu-id="fe60c-185">*klient*</span><span class="sxs-lookup"><span data-stu-id="fe60c-185">*tenant*</span></span> |<span data-ttu-id="fe60c-186">Krävs.</span><span class="sxs-lookup"><span data-stu-id="fe60c-186">Required.</span></span> <span data-ttu-id="fe60c-187">hello klient-ID för hello Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="fe60c-187">hello tenant identifier for hello Azure AD tenant.</span></span> |
| <span data-ttu-id="fe60c-188">*målgrupp*</span><span class="sxs-lookup"><span data-stu-id="fe60c-188">*audience*</span></span> |<span data-ttu-id="fe60c-189">Krävs.</span><span class="sxs-lookup"><span data-stu-id="fe60c-189">Required.</span></span> <span data-ttu-id="fe60c-190">Detta är inställt toohttps://management.core.windows.net/.</span><span class="sxs-lookup"><span data-stu-id="fe60c-190">This is set toohttps://management.core.windows.net/.</span></span> |
| <span data-ttu-id="fe60c-191">*clientId*</span><span class="sxs-lookup"><span data-stu-id="fe60c-191">*clientId*</span></span> |<span data-ttu-id="fe60c-192">Krävs.</span><span class="sxs-lookup"><span data-stu-id="fe60c-192">Required.</span></span> <span data-ttu-id="fe60c-193">Ange hello klient-ID för hello Azure AD-program.</span><span class="sxs-lookup"><span data-stu-id="fe60c-193">Provide hello client identifier for hello Azure AD application.</span></span> |
| <span data-ttu-id="fe60c-194">*hemligt*</span><span class="sxs-lookup"><span data-stu-id="fe60c-194">*secret*</span></span> |<span data-ttu-id="fe60c-195">Krävs.</span><span class="sxs-lookup"><span data-stu-id="fe60c-195">Required.</span></span> <span data-ttu-id="fe60c-196">Hemligheten för hello-klient som begär hello-token.</span><span class="sxs-lookup"><span data-stu-id="fe60c-196">Secret of hello client that is requesting hello token.</span></span> |

### <a name="determining-your-tenant-identifier"></a><span data-ttu-id="fe60c-197">Fastställa din klient-ID.</span><span class="sxs-lookup"><span data-stu-id="fe60c-197">Determining your Tenant Identifier</span></span>
<span data-ttu-id="fe60c-198">Du kan hitta hello klient-ID för hello Azure AD-klienten genom att köra `Get-AzureAccount` i Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fe60c-198">You can find hello tenant identifier for hello Azure AD tenant by running `Get-AzureAccount` in Azure PowerShell.</span></span>

## <a name="response-body-for-activedirectoryoauth-authentication"></a><span data-ttu-id="fe60c-199">Svarstexten för ActiveDirectoryOAuth autentisering</span><span class="sxs-lookup"><span data-stu-id="fe60c-199">Response Body for ActiveDirectoryOAuth Authentication</span></span>
<span data-ttu-id="fe60c-200">När en begäran skickas med autentisering information innehåller hello svaret hello efter autentisering-relaterade element.</span><span class="sxs-lookup"><span data-stu-id="fe60c-200">When a request is sent with authentication info, hello response contains hello following authentication-related elements.</span></span>

| <span data-ttu-id="fe60c-201">Element</span><span class="sxs-lookup"><span data-stu-id="fe60c-201">Element</span></span> | <span data-ttu-id="fe60c-202">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fe60c-202">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="fe60c-203">*autentisering (överordnat element)*</span><span class="sxs-lookup"><span data-stu-id="fe60c-203">*authentication (parent element)*</span></span> |<span data-ttu-id="fe60c-204">Autentiseringsobjekt för att använda ActiveDirectoryOAuth autentisering.</span><span class="sxs-lookup"><span data-stu-id="fe60c-204">Authentication object for using ActiveDirectoryOAuth authentication.</span></span> |
| <span data-ttu-id="fe60c-205">*typ*</span><span class="sxs-lookup"><span data-stu-id="fe60c-205">*type*</span></span> |<span data-ttu-id="fe60c-206">Typ av autentisering.</span><span class="sxs-lookup"><span data-stu-id="fe60c-206">Type of authentication.</span></span> <span data-ttu-id="fe60c-207">Hello-värdet är för ActiveDirectoryOAuth autentisering `ActiveDirectoryOAuth`.</span><span class="sxs-lookup"><span data-stu-id="fe60c-207">For ActiveDirectoryOAuth authentication, hello value is `ActiveDirectoryOAuth`.</span></span> |
| <span data-ttu-id="fe60c-208">*klient*</span><span class="sxs-lookup"><span data-stu-id="fe60c-208">*tenant*</span></span> |<span data-ttu-id="fe60c-209">hello klient-ID för hello Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="fe60c-209">hello tenant identifier for hello Azure AD tenant.</span></span> |
| <span data-ttu-id="fe60c-210">*målgrupp*</span><span class="sxs-lookup"><span data-stu-id="fe60c-210">*audience*</span></span> |<span data-ttu-id="fe60c-211">Detta är inställt toohttps://management.core.windows.net/.</span><span class="sxs-lookup"><span data-stu-id="fe60c-211">This is set toohttps://management.core.windows.net/.</span></span> |
| <span data-ttu-id="fe60c-212">*clientId*</span><span class="sxs-lookup"><span data-stu-id="fe60c-212">*clientId*</span></span> |<span data-ttu-id="fe60c-213">Hej klient-ID för hello Azure AD-program.</span><span class="sxs-lookup"><span data-stu-id="fe60c-213">hello client identifier for hello Azure AD application.</span></span> |

## <a name="sample-rest-request-for-activedirectoryoauth-authentication"></a><span data-ttu-id="fe60c-214">Exempel på REST-begäran för ActiveDirectoryOAuth autentisering</span><span class="sxs-lookup"><span data-stu-id="fe60c-214">Sample REST Request for ActiveDirectoryOAuth Authentication</span></span>
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

## <a name="sample-rest-response-for-activedirectoryoauth-authentication"></a><span data-ttu-id="fe60c-215">RESTEN av Exempelsvar för ActiveDirectoryOAuth autentisering</span><span class="sxs-lookup"><span data-stu-id="fe60c-215">Sample REST Response for ActiveDirectoryOAuth Authentication</span></span>
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

## <a name="see-also"></a><span data-ttu-id="fe60c-216">Se även</span><span class="sxs-lookup"><span data-stu-id="fe60c-216">See Also</span></span>
 [<span data-ttu-id="fe60c-217">Vad är Scheduler?</span><span class="sxs-lookup"><span data-stu-id="fe60c-217">What is Scheduler?</span></span>](scheduler-intro.md)

 [<span data-ttu-id="fe60c-218">Begrepp, terminologi och entitetshierarki relaterade till Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="fe60c-218">Azure Scheduler concepts, terminology, and entity hierarchy</span></span>](scheduler-concepts-terms.md)

 [<span data-ttu-id="fe60c-219">Komma igång med Schemaläggaren i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="fe60c-219">Get started using Scheduler in hello Azure portal</span></span>](scheduler-get-started-portal.md)

 [<span data-ttu-id="fe60c-220">Prenumerationer och fakturering i Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="fe60c-220">Plans and billing in Azure Scheduler</span></span>](scheduler-plans-billing.md)

 [<span data-ttu-id="fe60c-221">Referens för REST-API:et för Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="fe60c-221">Azure Scheduler REST API reference</span></span>](https://msdn.microsoft.com/library/mt629143)

 [<span data-ttu-id="fe60c-222">Referens för PowerShell-cmdlets för Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="fe60c-222">Azure Scheduler PowerShell cmdlets reference</span></span>](scheduler-powershell-reference.md)

 [<span data-ttu-id="fe60c-223">Hög tillgänglighet och tillförlitlighet i Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="fe60c-223">Azure Scheduler high-availability and reliability</span></span>](scheduler-high-availability-reliability.md)

 [<span data-ttu-id="fe60c-224">Gränser, standardinställningar och felkoder i Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="fe60c-224">Azure Scheduler limits, defaults, and error codes</span></span>](scheduler-limits-defaults-errors.md)

