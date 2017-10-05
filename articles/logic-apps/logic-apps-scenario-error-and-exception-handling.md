---
title: "Undantagshantering & fel loggning scenario – Azure Logic Apps | Microsoft Docs"
description: "Beskriver en verklig användningsfall om avancerad undantagshantering och felloggning för Logikappar i Azure"
keywords: 
services: logic-apps
author: hedidin
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 63b0b843-f6b0-4d9a-98d0-17500be17385
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 07/29/2016
ms.author: LADocs; b-hoedid
ms.openlocfilehash: 044de27c75da93c95609110d2b73336c42f746fe
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="scenario-exception-handling-and-error-logging-for-logic-apps"></a><span data-ttu-id="db985-103">Scenario: Undantagshantering och felloggning för logic apps</span><span class="sxs-lookup"><span data-stu-id="db985-103">Scenario: Exception handling and error logging for logic apps</span></span>

<span data-ttu-id="db985-104">Det här scenariot beskriver hur du kan utöka en logikapp för att bättre kunna stödja undantagshantering.</span><span class="sxs-lookup"><span data-stu-id="db985-104">This scenario describes how you can extend a logic app to better support exception handling.</span></span> <span data-ttu-id="db985-105">Vi har använt en verklig användningsfall för att besvara frågan: ”Azure Logikappar stöder undantag och felhantering”?</span><span class="sxs-lookup"><span data-stu-id="db985-105">We've used a real-life use case to answer the question: "Does Azure Logic Apps support exception and error handling?"</span></span>

> [!NOTE]
> <span data-ttu-id="db985-106">Det aktuella schemat för Logikappar i Azure tillhandahåller en standardmall för åtgärden svar.</span><span class="sxs-lookup"><span data-stu-id="db985-106">The current Azure Logic Apps schema provides a standard template for action responses.</span></span> <span data-ttu-id="db985-107">Den här mallen innehåller både interna validering och felsvar som returneras från en API-app.</span><span class="sxs-lookup"><span data-stu-id="db985-107">This template includes both internal validation and error responses returned from an API app.</span></span>

## <a name="scenario-and-use-case-overview"></a><span data-ttu-id="db985-108">Scenariot och Använd case-översikt</span><span class="sxs-lookup"><span data-stu-id="db985-108">Scenario and use case overview</span></span>

<span data-ttu-id="db985-109">Här är artikeln användningsfall för det här scenariot:</span><span class="sxs-lookup"><span data-stu-id="db985-109">Here's the story as the use case for this scenario:</span></span> 

<span data-ttu-id="db985-110">Välkända sjukvårdsorganisation arbetar oss att utveckla en lösning för Azure som skulle skapa en patient portal genom att använda Microsoft Dynamics CRM Online.</span><span class="sxs-lookup"><span data-stu-id="db985-110">A well-known healthcare organization engaged us to develop an Azure solution that would create a patient portal by using Microsoft Dynamics CRM Online.</span></span> <span data-ttu-id="db985-111">De behövs för att skicka möte poster mellan patient Dynamics CRM Online-portalen och Salesforce.</span><span class="sxs-lookup"><span data-stu-id="db985-111">They needed to send appointment records between the Dynamics CRM Online patient portal and Salesforce.</span></span> <span data-ttu-id="db985-112">Vi har ombedd att använda den [HL7 FHIR](http://www.hl7.org/implement/standards/fhir/) standard för alla patientjournaler.</span><span class="sxs-lookup"><span data-stu-id="db985-112">We were asked to use the [HL7 FHIR](http://www.hl7.org/implement/standards/fhir/) standard for all patient records.</span></span>

<span data-ttu-id="db985-113">Projektet har två viktiga krav:</span><span class="sxs-lookup"><span data-stu-id="db985-113">The project had two major requirements:</span></span>  

* <span data-ttu-id="db985-114">En metod för att logga poster som skickas från Dynamics CRM Online-portalen</span><span class="sxs-lookup"><span data-stu-id="db985-114">A method to log records sent from the Dynamics CRM Online portal</span></span>
* <span data-ttu-id="db985-115">Ett sätt att visa alla fel som uppstått i arbetsflödet</span><span class="sxs-lookup"><span data-stu-id="db985-115">A way to view any errors that occurred within the workflow</span></span>

> [!TIP]
> <span data-ttu-id="db985-116">Se en övergripande video om det här projektet [integrering användargrupp](http://www.integrationusergroup.com/logic-apps-support-error-handling/ "Integration User Group").</span><span class="sxs-lookup"><span data-stu-id="db985-116">For a high-level video about this project, see [Integration User Group](http://www.integrationusergroup.com/logic-apps-support-error-handling/ "Integration User Group").</span></span>

## <a name="how-we-solved-the-problem"></a><span data-ttu-id="db985-117">Hur vi löste problemet</span><span class="sxs-lookup"><span data-stu-id="db985-117">How we solved the problem</span></span>

<span data-ttu-id="db985-118">Vi valde [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/ "Azure Cosmos DB") som databas för logg- och fel-poster (Cosmos DB refererar till poster som dokument).</span><span class="sxs-lookup"><span data-stu-id="db985-118">We chose [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/ "Azure Cosmos DB") As a repository for the log and error records (Cosmos DB refers to records as documents).</span></span> <span data-ttu-id="db985-119">Eftersom Azure Logikappar har en standardmall för alla svar kan vi inte att skapa ett anpassat schema.</span><span class="sxs-lookup"><span data-stu-id="db985-119">Because Azure Logic Apps has a standard template for all responses, we would not have to create a custom schema.</span></span> <span data-ttu-id="db985-120">Kan vi skapa en API-app på **infoga** och **frågan** för både fel och loggfiler poster.</span><span class="sxs-lookup"><span data-stu-id="db985-120">We could create an API app to **Insert** and **Query** for both error and log records.</span></span> <span data-ttu-id="db985-121">Vi kan också definiera ett schema för varje API-App.</span><span class="sxs-lookup"><span data-stu-id="db985-121">We could also define a schema for each within the API app.</span></span>  

<span data-ttu-id="db985-122">Ett annat krav är att rensa poster efter ett visst datum.</span><span class="sxs-lookup"><span data-stu-id="db985-122">Another requirement was to purge records after a certain date.</span></span> <span data-ttu-id="db985-123">Cosmos DB har en egenskap som kallas [Time to Live](https://azure.microsoft.com/blog/documentdb-now-supports-time-to-live-ttl/ "Time to Live") (TTL) som tillåts att vi kan ange en **Time to Live** värde för varje post eller en samling.</span><span class="sxs-lookup"><span data-stu-id="db985-123">Cosmos DB has a property called [Time to Live](https://azure.microsoft.com/blog/documentdb-now-supports-time-to-live-ttl/ "Time to Live") (TTL), which allowed us to set a **Time to Live** value for each record or collection.</span></span> <span data-ttu-id="db985-124">Den här funktionen elimineras behovet av att manuellt ta bort poster i Cosmos-databasen.</span><span class="sxs-lookup"><span data-stu-id="db985-124">This capability eliminated the need to manually delete records in Cosmos DB.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="db985-125">Den här kursen måste du skapa en Cosmos-DB-databas och två samlingar (loggning och -fel).</span><span class="sxs-lookup"><span data-stu-id="db985-125">To complete this tutorial, you need to create a Cosmos DB database and two collections (Logging and Errors).</span></span>

## <a name="create-the-logic-app"></a><span data-ttu-id="db985-126">Skapa logikappen</span><span class="sxs-lookup"><span data-stu-id="db985-126">Create the logic app</span></span>

<span data-ttu-id="db985-127">Det första steget är att skapa logikappen och öppna appen i logik App Designer.</span><span class="sxs-lookup"><span data-stu-id="db985-127">The first step is to create the logic app and open the app in Logic App Designer.</span></span> <span data-ttu-id="db985-128">I det här exemplet använder vi överordnad-underordnad logikappar.</span><span class="sxs-lookup"><span data-stu-id="db985-128">In this example, we are using parent-child logic apps.</span></span> <span data-ttu-id="db985-129">Vi förutsätter att det redan har skapat överordnat och kommer att skapa en logikapp för underordnade.</span><span class="sxs-lookup"><span data-stu-id="db985-129">Let's assume that we have already created the parent and are going to create one child logic app.</span></span>

<span data-ttu-id="db985-130">Eftersom vi ska logga posten från Dynamics CRM Online, låt oss börja längst upp.</span><span class="sxs-lookup"><span data-stu-id="db985-130">Because we are going to log the record coming out of Dynamics CRM Online, let's start at the top.</span></span> <span data-ttu-id="db985-131">Vi måste använda en **begära** utlösare, eftersom logikappen överordnade utlöser underordnad.</span><span class="sxs-lookup"><span data-stu-id="db985-131">We must use a **Request** trigger because the parent logic app triggers this child.</span></span>

### <a name="logic-app-trigger"></a><span data-ttu-id="db985-132">Logik apputlösare</span><span class="sxs-lookup"><span data-stu-id="db985-132">Logic app trigger</span></span>

<span data-ttu-id="db985-133">Vi använder en **begära** utlösa som visas i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="db985-133">We are using a **Request** trigger as shown in the following example:</span></span>

```` json
"triggers": {
        "request": {
          "type": "request",
          "kind": "http",
          "inputs": {
            "schema": {
              "properties": {
                "CRMid": {
                  "type": "string"
                },
                "recordType": {
                  "type": "string"
                },
                "salesforceID": {
                  "type": "string"
                },
                "update": {
                  "type": "boolean"
                }
              },
              "required": [
                "CRMid",
                "recordType",
                "salesforceID",
                "update"
              ],
              "type": "object"
            }
          }
        }
      },

````


## <a name="steps"></a><span data-ttu-id="db985-134">Steg</span><span class="sxs-lookup"><span data-stu-id="db985-134">Steps</span></span>

<span data-ttu-id="db985-135">Vi måste logga källan (request) för patient posten från Dynamics CRM Online-portalen.</span><span class="sxs-lookup"><span data-stu-id="db985-135">We must log the source (request) of the patient record from the Dynamics CRM Online portal.</span></span>

1. <span data-ttu-id="db985-136">Vi måste få en ny post för möte från Dynamics CRM Online.</span><span class="sxs-lookup"><span data-stu-id="db985-136">We must get a new appointment record from Dynamics CRM Online.</span></span>

   <span data-ttu-id="db985-137">Utlösaren kommer från CRM ger oss med den **CRM PatentId**, **posttyp**, **ny eller uppdaterad post** (ny eller uppdatera booleskt värde), och  **SalesforceId**.</span><span class="sxs-lookup"><span data-stu-id="db985-137">The trigger coming from CRM provides us with the **CRM PatentId**, **record type**, **New or Updated Record** (new or update Boolean value), and **SalesforceId**.</span></span> <span data-ttu-id="db985-138">Den **SalesforceId** kan vara null eftersom den används endast för en uppdatering.</span><span class="sxs-lookup"><span data-stu-id="db985-138">The **SalesforceId** can be null because it's only used for an update.</span></span>
   <span data-ttu-id="db985-139">Vi få CRM-post med hjälp av CRM **PatientID** och **posttyp**.</span><span class="sxs-lookup"><span data-stu-id="db985-139">We get the CRM record by using the CRM **PatientID** and the **Record Type**.</span></span>

2. <span data-ttu-id="db985-140">Nu ska vi måste du lägga till våra DocumentDB API-app **InsertLogEntry** åtgärden som visas här i logik App Designer.</span><span class="sxs-lookup"><span data-stu-id="db985-140">Next, we need to add our DocumentDB API app **InsertLogEntry** operation as shown here in Logic App Designer.</span></span>

   <span data-ttu-id="db985-141">**Infoga loggpost**</span><span class="sxs-lookup"><span data-stu-id="db985-141">**Insert log entry**</span></span>

   ![Infoga loggpost](media/logic-apps-scenario-error-and-exception-handling/lognewpatient.png)

   <span data-ttu-id="db985-143">**Infoga fel post**</span><span class="sxs-lookup"><span data-stu-id="db985-143">**Insert error entry**</span></span>

   ![Infoga loggpost](media/logic-apps-scenario-error-and-exception-handling/insertlogentry.png)

   <span data-ttu-id="db985-145">**Kontrollera att skapa poster fel**</span><span class="sxs-lookup"><span data-stu-id="db985-145">**Check for create record failure**</span></span>

   ![Villkor](media/logic-apps-scenario-error-and-exception-handling/condition.png)

## <a name="logic-app-source-code"></a><span data-ttu-id="db985-147">Källkoden för logik app</span><span class="sxs-lookup"><span data-stu-id="db985-147">Logic app source code</span></span>

> [!NOTE]
> <span data-ttu-id="db985-148">Följande exempel är bara exempel.</span><span class="sxs-lookup"><span data-stu-id="db985-148">The following examples are samples only.</span></span> <span data-ttu-id="db985-149">Eftersom den här kursen är baserad på en implementering nu i produktion, värdet för en **Källnoden** kan inte visa egenskaper som är relaterade till att schemalägga ett möte. ></span><span class="sxs-lookup"><span data-stu-id="db985-149">Because this tutorial is based on an implementation now in production, the value of a **Source Node** might not display properties that are related to scheduling an appointment.></span></span> 

### <a name="logging"></a><span data-ttu-id="db985-150">Loggning</span><span class="sxs-lookup"><span data-stu-id="db985-150">Logging</span></span>

<span data-ttu-id="db985-151">Följande logik app kodexempel visar hur du hanterar loggning.</span><span class="sxs-lookup"><span data-stu-id="db985-151">The following logic app code sample shows how to handle logging.</span></span>

#### <a name="log-entry"></a><span data-ttu-id="db985-152">Loggpost</span><span class="sxs-lookup"><span data-stu-id="db985-152">Log entry</span></span>

<span data-ttu-id="db985-153">Här är källkoden logik app för att lägga till en loggpost.</span><span class="sxs-lookup"><span data-stu-id="db985-153">Here is the logic app source code for inserting a log entry.</span></span>

``` json
"InsertLogEntry": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "date": "@{outputs('Gets_NewPatientRecord')['headers']['Date']}",
            "operation": "New Patient",
            "patientId": "@{triggerBody()['CRMid']}",
            "providerId": "@{triggerBody()['providerID']}",
            "source": "@{outputs('Gets_NewPatientRecord')['headers']}"
        },
        "method": "post",
        "uri": "https://.../api/Log"
        },
        "runAfter":    {
            "Gets_NewPatientecord": ["Succeeded"]
        }
}
```

#### <a name="log-request"></a><span data-ttu-id="db985-154">Log-begäran</span><span class="sxs-lookup"><span data-stu-id="db985-154">Log request</span></span>

<span data-ttu-id="db985-155">Här är begäran loggmeddelande anslås API-app.</span><span class="sxs-lookup"><span data-stu-id="db985-155">Here is the log request message posted to the API app.</span></span>

``` json
    {
    "uri": "https://.../api/Log",
    "method": "post",
    "body": {
        "date": "Fri, 10 Jun 2016 22:31:56 GMT",
        "operation": "New Patient",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "providerId": "",
        "source": "{/"Pragma/":/"no-cache/",/"x-ms-request-id/":/"e750c9a9-bd48-44c4-bbba-1688b6f8a132/",/"OData-Version/":/"4.0/",/"Cache-Control/":/"no-cache/",/"Date/":/"Fri, 10 Jun 2016 22:31:56 GMT/",/"Set-Cookie/":/"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1/",/"Server/":/"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0/",/"X-AspNet-Version/":/"4.0.30319/",/"X-Powered-By/":/"ASP.NET/",/"Content-Length/":/"1935/",/"Content-Type/":/"application/json; odata.metadata=minimal; odata.streaming=true/",/"Expires/":/"-1/"}"
        }
    }

```


#### <a name="log-response"></a><span data-ttu-id="db985-156">Loggsvar</span><span class="sxs-lookup"><span data-stu-id="db985-156">Log response</span></span>

<span data-ttu-id="db985-157">Här är svar loggmeddelande från API-app.</span><span class="sxs-lookup"><span data-stu-id="db985-157">Here is the log response message from the API app.</span></span>

``` json
{
    "statusCode": 200,
    "headers": {
        "Pragma": "no-cache",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:32:17 GMT",
        "Server": "Microsoft-IIS/8.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "964",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "ttl": 2592000,
        "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0_1465597937",
        "_rid": "XngRAOT6IQEHAAAAAAAAAA==",
        "_self": "dbs/XngRAA==/colls/XngRAOT6IQE=/docs/XngRAOT6IQEHAAAAAAAAAA==/",
        "_ts": 1465597936,
        "_etag": "/"0400fc2f-0000-0000-0000-575b3ff00000/"",
        "patientID": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "timestamp": "2016-06-10T22:31:56Z",
        "source": "{/"Pragma/":/"no-cache/",/"x-ms-request-id/":/"e750c9a9-bd48-44c4-bbba-1688b6f8a132/",/"OData-Version/":/"4.0/",/"Cache-Control/":/"no-cache/",/"Date/":/"Fri, 10 Jun 2016 22:31:56 GMT/",/"Set-Cookie/":/"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1/",/"Server/":/"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0/",/"X-AspNet-Version/":/"4.0.30319/",/"X-Powered-By/":/"ASP.NET/",/"Content-Length/":/"1935/",/"Content-Type/":/"application/json; odata.metadata=minimal; odata.streaming=true/",/"Expires/":/"-1/"}",
        "operation": "New Patient",
        "salesforceId": "",
        "expired": false
    }
}

```

<span data-ttu-id="db985-158">Nu ska vi titta på felhantering steg.</span><span class="sxs-lookup"><span data-stu-id="db985-158">Now let's look at the error handling steps.</span></span>

### <a name="error-handling"></a><span data-ttu-id="db985-159">Felhantering</span><span class="sxs-lookup"><span data-stu-id="db985-159">Error handling</span></span>

<span data-ttu-id="db985-160">Följande logik app kodexempel visar hur du kan implementera felhantering.</span><span class="sxs-lookup"><span data-stu-id="db985-160">The following logic app code sample shows how you can implement error handling.</span></span>

#### <a name="create-error-record"></a><span data-ttu-id="db985-161">Skapa Felpost</span><span class="sxs-lookup"><span data-stu-id="db985-161">Create error record</span></span>

<span data-ttu-id="db985-162">Här är källkoden för att skapa en Felpost logik app.</span><span class="sxs-lookup"><span data-stu-id="db985-162">Here is the logic app source code for creating an error record.</span></span>

``` json
"actions": {
    "CreateErrorRecord": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "action": "New_Patient",
            "isError": true,
            "crmId": "@{triggerBody()['CRMid']}",
            "patientID": "@{triggerBody()['CRMid']}",
            "message": "@{body('Create_NewPatientRecord')['message']}",
            "providerId": "@{triggerBody()['providerId']}",
            "severity": 4,
            "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
            "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
            "salesforceId": "",
            "update": false
        },
        "method": "post",
        "uri": "https://.../api/CrMtoSfError"
        },
        "runAfter":
        {
            "Create_NewPatientRecord": ["Failed" ]
        }
    }
}             
```

#### <a name="insert-error-into-cosmos-db--request"></a><span data-ttu-id="db985-163">Infoga fel i Cosmos-DB - begäran</span><span class="sxs-lookup"><span data-stu-id="db985-163">Insert error into Cosmos DB--request</span></span>

``` json

{
    "uri": "https://.../api/CrMtoSfError",
    "method": "post",
    "body": {
        "action": "New_Patient",
        "isError": true,
        "crmId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "providerId": "",
        "severity": 4,
        "salesforceId": "",
        "update": false,
        "source": "{/"Account_Class_vod__c/":/"PRAC/",/"Account_Status_MED__c/":/"I/",/"CRM_HUB_ID__c/":/"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0/",/"Credentials_vod__c/",/"DTC_ID_MED__c/":/"/",/"Fax/":/"/",/"FirstName/":/"A/",/"Gender_vod__c/":/"/",/"IMS_ID__c/":/"/",/"LastName/":/"BAILEY/",/"MasterID_mp__c/":/"/",/"C_ID_MED__c/":/"851588/",/"Middle_vod__c/":/"/",/"NPI_vod__c/":/"/",/"PDRP_MED__c/":false,/"PersonDoNotCall/":false,/"PersonEmail/":/"/",/"PersonHasOptedOutOfEmail/":false,/"PersonHasOptedOutOfFax/":false,/"PersonMobilePhone/":/"/",/"Phone/":/"/",/"Practicing_Specialty__c/":/"FM - FAMILY MEDICINE/",/"Primary_City__c/":/"/",/"Primary_State__c/":/"/",/"Primary_Street_Line2__c/":/"/",/"Primary_Street__c/":/"/",/"Primary_Zip__c/":/"/",/"RecordTypeId/":/"012U0000000JaPWIA0/",/"Request_Date__c/":/"2016-06-10T22:31:55.9647467Z/",/"ONY_ID__c/":/"/",/"Specialty_1_vod__c/":/"/",/"Suffix_vod__c/":/"/",/"Website/":/"/"}",
        "statusCode": "400"
    }
}
```

#### <a name="insert-error-into-cosmos-db--response"></a><span data-ttu-id="db985-164">Infoga fel i Cosmos-DB - svar</span><span class="sxs-lookup"><span data-stu-id="db985-164">Insert error into Cosmos DB--response</span></span>

``` json
{
    "statusCode": 200,
    "headers": {
        "Pragma": "no-cache",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:57 GMT",
        "Server": "Microsoft-IIS/8.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "1561",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0-1465597917",
        "_rid": "sQx2APhVzAA8AAAAAAAAAA==",
        "_self": "dbs/sQx2AA==/colls/sQx2APhVzAA=/docs/sQx2APhVzAA8AAAAAAAAAA==/",
        "_ts": 1465597912,
        "_etag": "/"0c00eaac-0000-0000-0000-575b3fdc0000/"",
        "prescriberId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "timestamp": "2016-06-10T22:31:57.3651027Z",
        "action": "New_Patient",
        "salesforceId": "",
        "update": false,
        "body": "CRM failed to complete task: Message: duplicate value found: CRM_HUB_ID__c duplicates value on record with id: 001U000001c83gK",
        "source": "{/"Account_Class_vod__c/":/"PRAC/",/"Account_Status_MED__c/":/"I/",/"CRM_HUB_ID__c/":/"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0/",/"Credentials_vod__c/":/"DO - Degree level is DO/",/"DTC_ID_MED__c/":/"/",/"Fax/":/"/",/"FirstName/":/"A/",/"Gender_vod__c/":/"/",/"IMS_ID__c/":/"/",/"LastName/":/"BAILEY/",/"MterID_mp__c/":/"/",/"Medicis_ID_MED__c/":/"851588/",/"Middle_vod__c/":/"/",/"NPI_vod__c/":/"/",/"PDRP_MED__c/":false,/"PersonDoNotCall/":false,/"PersonEmail/":/"/",/"PersonHasOptedOutOfEmail/":false,/"PersonHasOptedOutOfFax/":false,/"PersonMobilePhone/":/"/",/"Phone/":/"/",/"Practicing_Specialty__c/":/"FM - FAMILY MEDICINE/",/"Primary_City__c/":/"/",/"Primary_State__c/":/"/",/"Primary_Street_Line2__c/":/"/",/"Primary_Street__c/":/"/",/"Primary_Zip__c/":/"/",/"RecordTypeId/":/"012U0000000JaPWIA0/",/"Request_Date__c/":/"2016-06-10T22:31:55.9647467Z/",/"XXXXXXX/":/"/",/"Specialty_1_vod__c/":/"/",/"Suffix_vod__c/":/"/",/"Website/":/"/"}",
        "code": 400,
        "errors": null,
        "isError": true,
        "severity": 4,
        "notes": null,
        "resolved": 0
        }
}
```

#### <a name="salesforce-error-response"></a><span data-ttu-id="db985-165">Salesforce felsvaret</span><span class="sxs-lookup"><span data-stu-id="db985-165">Salesforce error response</span></span>

``` json
{
    "statusCode": 400,
    "headers": {
        "Pragma": "no-cache",
        "x-ms-request-id": "3e8e4884-288e-4633-972c-8271b2cc912c",
        "X-Content-Type-Options": "nosniff",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:56 GMT",
        "Set-Cookie": "ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1",
        "Server": "Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "205",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "status": 400,
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "source": "Salesforce.Common",
        "errors": []
    }
}

```

### <a name="return-the-response-back-to-parent-logic-app"></a><span data-ttu-id="db985-166">Returnera svar tillbaka till överordnad logikapp</span><span class="sxs-lookup"><span data-stu-id="db985-166">Return the response back to parent logic app</span></span>

<span data-ttu-id="db985-167">När du får svar kan överföra du svaret tillbaka till överordnad logikappen.</span><span class="sxs-lookup"><span data-stu-id="db985-167">After you get the response, you can pass the response back to the parent logic app.</span></span>

#### <a name="return-success-response-to-parent-logic-app"></a><span data-ttu-id="db985-168">Returnera lyckat svar till överordnade logikapp</span><span class="sxs-lookup"><span data-stu-id="db985-168">Return success response to parent logic app</span></span>

``` json
"SuccessResponse": {
    "runAfter":
        {
            "UpdateNew_CRMPatientResponse": ["Succeeded"]
        },
    "inputs": {
        "body": {
            "status": "Success"
    },
    "headers": {
    "    Content-type": "application/json",
        "x-ms-date": "@utcnow()"
    },
    "statusCode": 200
    },
    "type": "Response"
}
```

#### <a name="return-error-response-to-parent-logic-app"></a><span data-ttu-id="db985-169">Returnera felsvar på överordnade logikapp</span><span class="sxs-lookup"><span data-stu-id="db985-169">Return error response to parent logic app</span></span>

``` json
"ErrorResponse": {
    "runAfter":
        {
            "Create_NewPatientRecord": ["Failed"]
        },
    "inputs": {
        "body": {
            "status": "BadRequest"
        },
        "headers": {
            "Content-type": "application/json",
            "x-ms-date": "@utcnow()"
        },
        "statusCode": 400
    },
    "type": "Response"
}

```


## <a name="cosmos-db-repository-and-portal"></a><span data-ttu-id="db985-170">Cosmos DB-databasen och -portalen</span><span class="sxs-lookup"><span data-stu-id="db985-170">Cosmos DB repository and portal</span></span>

<span data-ttu-id="db985-171">Vår lösning tillagda funktioner med [Cosmos DB](https://azure.microsoft.com/services/documentdb).</span><span class="sxs-lookup"><span data-stu-id="db985-171">Our solution added capabilities with [Cosmos DB](https://azure.microsoft.com/services/documentdb).</span></span>

### <a name="error-management-portal"></a><span data-ttu-id="db985-172">Fel-hanteringsportalen</span><span class="sxs-lookup"><span data-stu-id="db985-172">Error management portal</span></span>

<span data-ttu-id="db985-173">Du kan skapa en MVC-webbapp för att visa felposter från Cosmos DB om du vill visa felen.</span><span class="sxs-lookup"><span data-stu-id="db985-173">To view the errors, you can create an MVC web app to display the error records from Cosmos DB.</span></span> <span data-ttu-id="db985-174">Den **lista**, **information**, **redigera**, och **ta bort** operations ingår i den aktuella versionen.</span><span class="sxs-lookup"><span data-stu-id="db985-174">The **List**, **Details**, **Edit**, and **Delete** operations are included in the current version.</span></span>

> [!NOTE]
> <span data-ttu-id="db985-175">Redigera åtgärd: Cosmos DB ersätter hela dokumentet.</span><span class="sxs-lookup"><span data-stu-id="db985-175">Edit operation: Cosmos DB replaces the entire document.</span></span> <span data-ttu-id="db985-176">Poster som visas i den **lista** och **detaljer** vyer är bara exempel.</span><span class="sxs-lookup"><span data-stu-id="db985-176">The records shown in the **List** and **Detail** views are samples only.</span></span> <span data-ttu-id="db985-177">De är inte faktiska patient möte poster.</span><span class="sxs-lookup"><span data-stu-id="db985-177">They are not actual patient appointment records.</span></span>

<span data-ttu-id="db985-178">Här följer exempel på vår MVC-appinformation som skapats med metoden som beskrivits tidigare.</span><span class="sxs-lookup"><span data-stu-id="db985-178">Here are examples of our MVC app details created with the previously described approach.</span></span>

#### <a name="error-management-list"></a><span data-ttu-id="db985-179">Fel vid hantering av lista</span><span class="sxs-lookup"><span data-stu-id="db985-179">Error management list</span></span>
![Fel vid lista](media/logic-apps-scenario-error-and-exception-handling/errorlist.png)

#### <a name="error-management-detail-view"></a><span data-ttu-id="db985-181">Fel vid hantering av detaljerad vy</span><span class="sxs-lookup"><span data-stu-id="db985-181">Error management detail view</span></span>
![Information om fel](media/logic-apps-scenario-error-and-exception-handling/errordetails.png)

### <a name="log-management-portal"></a><span data-ttu-id="db985-183">Log-hanteringsportalen</span><span class="sxs-lookup"><span data-stu-id="db985-183">Log management portal</span></span>

<span data-ttu-id="db985-184">Om du vill visa loggfilerna kan skapat vi också en MVC-webbapp.</span><span class="sxs-lookup"><span data-stu-id="db985-184">To view the logs, we also created an MVC web app.</span></span> <span data-ttu-id="db985-185">Här följer exempel på vår MVC-appinformation som skapats med metoden som beskrivits tidigare.</span><span class="sxs-lookup"><span data-stu-id="db985-185">Here are examples of our MVC app details created with the previously described approach.</span></span>

#### <a name="sample-log-detail-view"></a><span data-ttu-id="db985-186">Exempel loggen detaljvy</span><span class="sxs-lookup"><span data-stu-id="db985-186">Sample log detail view</span></span>
![Loggen detaljvy](media/logic-apps-scenario-error-and-exception-handling/samplelogdetail.png)

### <a name="api-app-details"></a><span data-ttu-id="db985-188">Information om API-app</span><span class="sxs-lookup"><span data-stu-id="db985-188">API app details</span></span>

#### <a name="logic-apps-exception-management-api"></a><span data-ttu-id="db985-189">Logic Apps undantag hanterings-API</span><span class="sxs-lookup"><span data-stu-id="db985-189">Logic Apps exception management API</span></span>

<span data-ttu-id="db985-190">Vår öppen källkod Azure Logikappar undantag hanterings-API-app innehåller funktioner som beskrivs här – det finns två domänkontrollanter:</span><span class="sxs-lookup"><span data-stu-id="db985-190">Our open-source Azure Logic Apps exception management API app provides functionality as described here - there are two controllers:</span></span>

* <span data-ttu-id="db985-191">**ErrorController** infogar en Felpost (dokument) i en DocumentDB-samling.</span><span class="sxs-lookup"><span data-stu-id="db985-191">**ErrorController** inserts an error record (document) in a DocumentDB collection.</span></span>
* <span data-ttu-id="db985-192">**LogController** infogar en loggpost (dokument) i en DocumentDB-samling.</span><span class="sxs-lookup"><span data-stu-id="db985-192">**LogController** Inserts a log record (document) in a DocumentDB collection.</span></span>

> [!TIP]
> <span data-ttu-id="db985-193">Både domänkontrollanter använder `async Task<dynamic>` åtgärder, så att åtgärder att lösa vid körning, så att vi kan skapa DocumentDB-schemat i brödtexten för åtgärden.</span><span class="sxs-lookup"><span data-stu-id="db985-193">Both controllers use `async Task<dynamic>` operations, allowing operations to resolve at runtime, so we can create the DocumentDB schema in the body of the operation.</span></span> 
> 

<span data-ttu-id="db985-194">Alla dokument i DocumentDB måste ha ett unikt ID.</span><span class="sxs-lookup"><span data-stu-id="db985-194">Every document in DocumentDB must have a unique ID.</span></span> <span data-ttu-id="db985-195">Vi använder `PatientId` och lägga till en tidsstämpel som konverteras till ett tidsstämpelvärde Unix (double).</span><span class="sxs-lookup"><span data-stu-id="db985-195">We are using `PatientId` and adding a timestamp that is converted to a Unix timestamp value (double).</span></span> <span data-ttu-id="db985-196">Vi trunkera värdet för att ta bort värdet bråkdelar.</span><span class="sxs-lookup"><span data-stu-id="db985-196">We truncate the value to remove the fractional value.</span></span>

<span data-ttu-id="db985-197">Du kan visa källkoden för fel styrningen API [från GitHub](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi/blob/master/Logic App Exception Management API/Controllers/ErrorController.cs).</span><span class="sxs-lookup"><span data-stu-id="db985-197">You can view the source code of our error controller API [from GitHub](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi/blob/master/Logic App Exception Management API/Controllers/ErrorController.cs).</span></span>

<span data-ttu-id="db985-198">Vi kallar API: et från en logikapp genom att använda följande syntax:</span><span class="sxs-lookup"><span data-stu-id="db985-198">We call the API from a logic app by using the following syntax:</span></span>

``` json
 "actions": {
        "CreateErrorRecord": {
          "metadata": {
            "apiDefinitionUrl": "https://.../swagger/docs/v1",
            "swaggerSource": "website"
          },
          "type": "Http",
          "inputs": {
            "body": {
              "action": "New_Patient",
              "isError": true,
              "crmId": "@{triggerBody()['CRMid']}",
              "prescriberId": "@{triggerBody()['CRMid']}",
              "message": "@{body('Create_NewPatientRecord')['message']}",
              "salesforceId": "@{triggerBody()['salesforceID']}",
              "severity": 4,
              "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
              "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
              "update": false
            },
            "method": "post",
            "uri": "https://.../api/CrMtoSfError"
          },
          "runAfter": {
              "Create_NewPatientRecord": ["Failed"]
            }
        }
 }
```

<span data-ttu-id="db985-199">Uttrycket i föregående kodexemplet söker efter den *Create_NewPatientRecord* status för **misslyckades**.</span><span class="sxs-lookup"><span data-stu-id="db985-199">The expression in the preceding code sample checks for the *Create_NewPatientRecord* status of **Failed**.</span></span>

## <a name="summary"></a><span data-ttu-id="db985-200">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="db985-200">Summary</span></span>

* <span data-ttu-id="db985-201">Du kan enkelt implementera loggning och hantera fel i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="db985-201">You can easily implement logging and error handling in a logic app.</span></span>
* <span data-ttu-id="db985-202">Du kan använda DocumentDB som en lagringsplats för logg- och fel-poster (dokument).</span><span class="sxs-lookup"><span data-stu-id="db985-202">You can use DocumentDB as the repository for log and error records (documents).</span></span>
* <span data-ttu-id="db985-203">Du kan använda MVC för att skapa en portal för att visa loggen och fel poster.</span><span class="sxs-lookup"><span data-stu-id="db985-203">You can use MVC to create a portal to display log and error records.</span></span>

### <a name="source-code"></a><span data-ttu-id="db985-204">Källkod</span><span class="sxs-lookup"><span data-stu-id="db985-204">Source code</span></span>

<span data-ttu-id="db985-205">Källkoden för Logic Apps undantag management API-program finns i det här [GitHub-lagringsplatsen](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi "logik App undantag Management API").</span><span class="sxs-lookup"><span data-stu-id="db985-205">The source code for the Logic Apps exception management API application is available in this [GitHub repository](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi "Logic App Exception Management API").</span></span>

## <a name="next-steps"></a><span data-ttu-id="db985-206">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="db985-206">Next steps</span></span>

* [<span data-ttu-id="db985-207">Visa mer logik app exempel och scenarier</span><span class="sxs-lookup"><span data-stu-id="db985-207">View more logic app examples and scenarios</span></span>](../logic-apps/logic-apps-examples-and-scenarios.md)
* [<span data-ttu-id="db985-208">Lär dig mer om hur du övervakar logikappar</span><span class="sxs-lookup"><span data-stu-id="db985-208">Learn about monitoring logic apps</span></span>](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [<span data-ttu-id="db985-209">Skapa mallar för automatisk distribution för logic apps</span><span class="sxs-lookup"><span data-stu-id="db985-209">Create automated deployment templates for logic apps</span></span>](../logic-apps/logic-apps-create-deploy-template.md)
