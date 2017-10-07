---
title: "aaaException hantering & fel loggning scenario – Azure Logic Apps | Microsoft Docs"
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
ms.openlocfilehash: e893a7b652254dca7b8a82398e8afd571f6ccd25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scenario-exception-handling-and-error-logging-for-logic-apps"></a><span data-ttu-id="0f5fd-103">Scenario: Undantagshantering och felloggning för logic apps</span><span class="sxs-lookup"><span data-stu-id="0f5fd-103">Scenario: Exception handling and error logging for logic apps</span></span>

<span data-ttu-id="0f5fd-104">Det här scenariot beskriver hur du kan utöka en logik app toobetter stöd undantagshantering.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-104">This scenario describes how you can extend a logic app toobetter support exception handling.</span></span> <span data-ttu-id="0f5fd-105">Vi har använt en verklig användning case tooanswer hello-fråga: ”Azure Logikappar stöder undantag och felhantering”?</span><span class="sxs-lookup"><span data-stu-id="0f5fd-105">We've used a real-life use case tooanswer hello question: "Does Azure Logic Apps support exception and error handling?"</span></span>

> [!NOTE]
> <span data-ttu-id="0f5fd-106">hello aktuella Logikappar i Azure-schemat innehåller en standardmall för åtgärden svar.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-106">hello current Azure Logic Apps schema provides a standard template for action responses.</span></span> <span data-ttu-id="0f5fd-107">Den här mallen innehåller både interna validering och felsvar som returneras från en API-app.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-107">This template includes both internal validation and error responses returned from an API app.</span></span>

## <a name="scenario-and-use-case-overview"></a><span data-ttu-id="0f5fd-108">Scenariot och Använd case-översikt</span><span class="sxs-lookup"><span data-stu-id="0f5fd-108">Scenario and use case overview</span></span>

<span data-ttu-id="0f5fd-109">Här är hello artikeln hello användningsfall för det här scenariot:</span><span class="sxs-lookup"><span data-stu-id="0f5fd-109">Here's hello story as hello use case for this scenario:</span></span> 

<span data-ttu-id="0f5fd-110">Välkända sjukvårdsorganisation arbetar oss toodevelop en Azure-lösning som skapar en patient portal genom att använda Microsoft Dynamics CRM Online.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-110">A well-known healthcare organization engaged us toodevelop an Azure solution that would create a patient portal by using Microsoft Dynamics CRM Online.</span></span> <span data-ttu-id="0f5fd-111">De behövs toosend möte poster mellan hello Dynamics CRM Online patient portal och Salesforce.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-111">They needed toosend appointment records between hello Dynamics CRM Online patient portal and Salesforce.</span></span> <span data-ttu-id="0f5fd-112">Har vi frågade toouse hello [HL7 FHIR](http://www.hl7.org/implement/standards/fhir/) standard för alla patientjournaler.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-112">We were asked toouse hello [HL7 FHIR](http://www.hl7.org/implement/standards/fhir/) standard for all patient records.</span></span>

<span data-ttu-id="0f5fd-113">hello projektet hade två viktiga krav:</span><span class="sxs-lookup"><span data-stu-id="0f5fd-113">hello project had two major requirements:</span></span>  

* <span data-ttu-id="0f5fd-114">En metod toolog poster som skickas från hello Dynamics CRM Online-portalen</span><span class="sxs-lookup"><span data-stu-id="0f5fd-114">A method toolog records sent from hello Dynamics CRM Online portal</span></span>
* <span data-ttu-id="0f5fd-115">Ett sätt tooview eventuella fel som uppstått i hello-arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="0f5fd-115">A way tooview any errors that occurred within hello workflow</span></span>

> [!TIP]
> <span data-ttu-id="0f5fd-116">Se en övergripande video om det här projektet [integrering användargrupp](http://www.integrationusergroup.com/logic-apps-support-error-handling/ "integrering användargrupp").</span><span class="sxs-lookup"><span data-stu-id="0f5fd-116">For a high-level video about this project, see [Integration User Group](http://www.integrationusergroup.com/logic-apps-support-error-handling/ "Integration User Group").</span></span>

## <a name="how-we-solved-hello-problem"></a><span data-ttu-id="0f5fd-117">Hur vi har löst problemet hello</span><span class="sxs-lookup"><span data-stu-id="0f5fd-117">How we solved hello problem</span></span>

<span data-ttu-id="0f5fd-118">Vi valde [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/ "Azure Cosmos DB") som en lagringsplats för hello loggen och fel poster (Cosmos DB refererar toorecords som dokument).</span><span class="sxs-lookup"><span data-stu-id="0f5fd-118">We chose [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/ "Azure Cosmos DB") As a repository for hello log and error records (Cosmos DB refers toorecords as documents).</span></span> <span data-ttu-id="0f5fd-119">Eftersom Azure Logikappar har en standardmall för alla svar kan vi inte toocreate ett anpassat schema.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-119">Because Azure Logic Apps has a standard template for all responses, we would not have toocreate a custom schema.</span></span> <span data-ttu-id="0f5fd-120">Kan vi skapa en API-app för**infoga** och **frågan** för både fel och loggfiler poster.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-120">We could create an API app too**Insert** and **Query** for both error and log records.</span></span> <span data-ttu-id="0f5fd-121">Vi kan också definiera ett schema för varje inom hello API-app.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-121">We could also define a schema for each within hello API app.</span></span>  

<span data-ttu-id="0f5fd-122">Ett annat krav var toopurge poster efter ett visst datum.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-122">Another requirement was toopurge records after a certain date.</span></span> <span data-ttu-id="0f5fd-123">Cosmos DB har en egenskap som kallas [tid tooLive](https://azure.microsoft.com/blog/documentdb-now-supports-time-to-live-ttl/ "tid tooLive") (TTL) som tillåts oss tooset en **tid tooLive** värde för varje post eller en samling.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-123">Cosmos DB has a property called [Time tooLive](https://azure.microsoft.com/blog/documentdb-now-supports-time-to-live-ttl/ "Time tooLive") (TTL), which allowed us tooset a **Time tooLive** value for each record or collection.</span></span> <span data-ttu-id="0f5fd-124">Den här funktionen elimineras hello måste toomanually ta bort poster i Cosmos-databasen.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-124">This capability eliminated hello need toomanually delete records in Cosmos DB.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0f5fd-125">toocomplete den här kursen behöver du toocreate en Cosmos-DB-databas och två samlingar (loggning och -fel).</span><span class="sxs-lookup"><span data-stu-id="0f5fd-125">toocomplete this tutorial, you need toocreate a Cosmos DB database and two collections (Logging and Errors).</span></span>

## <a name="create-hello-logic-app"></a><span data-ttu-id="0f5fd-126">Skapa hello logikapp</span><span class="sxs-lookup"><span data-stu-id="0f5fd-126">Create hello logic app</span></span>

<span data-ttu-id="0f5fd-127">hello första steget är toocreate hello logikapp och öppna hello app i logik App Designer.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-127">hello first step is toocreate hello logic app and open hello app in Logic App Designer.</span></span> <span data-ttu-id="0f5fd-128">I det här exemplet använder vi överordnad-underordnad logikappar.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-128">In this example, we are using parent-child logic apps.</span></span> <span data-ttu-id="0f5fd-129">Vi förutsätter att vi har redan skapat hello överordnade och ska toocreate en underordnad logikapp.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-129">Let's assume that we have already created hello parent and are going toocreate one child logic app.</span></span>

<span data-ttu-id="0f5fd-130">Eftersom vi toolog hello post från Dynamics CRM Online, låt oss börja hello överst.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-130">Because we are going toolog hello record coming out of Dynamics CRM Online, let's start at hello top.</span></span> <span data-ttu-id="0f5fd-131">Vi måste använda en **begära** utlösare, eftersom hello överordnade logikapp utlöser underordnad.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-131">We must use a **Request** trigger because hello parent logic app triggers this child.</span></span>

### <a name="logic-app-trigger"></a><span data-ttu-id="0f5fd-132">Logik apputlösare</span><span class="sxs-lookup"><span data-stu-id="0f5fd-132">Logic app trigger</span></span>

<span data-ttu-id="0f5fd-133">Vi använder en **begära** utlösa som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="0f5fd-133">We are using a **Request** trigger as shown in hello following example:</span></span>

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


## <a name="steps"></a><span data-ttu-id="0f5fd-134">Steg</span><span class="sxs-lookup"><span data-stu-id="0f5fd-134">Steps</span></span>

<span data-ttu-id="0f5fd-135">Vi måste logga hello källan (request) för hello patient post från hello Dynamics CRM Online-portalen.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-135">We must log hello source (request) of hello patient record from hello Dynamics CRM Online portal.</span></span>

1. <span data-ttu-id="0f5fd-136">Vi måste få en ny post för möte från Dynamics CRM Online.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-136">We must get a new appointment record from Dynamics CRM Online.</span></span>

   <span data-ttu-id="0f5fd-137">hello-utlösare som kommer från CRM ger oss hello **CRM PatentId**, **posttyp**, **ny eller uppdaterad post** (ny eller uppdatera booleskt värde), och  **SalesforceId**.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-137">hello trigger coming from CRM provides us with hello **CRM PatentId**, **record type**, **New or Updated Record** (new or update Boolean value), and **SalesforceId**.</span></span> <span data-ttu-id="0f5fd-138">Hej **SalesforceId** kan vara null eftersom den används endast för en uppdatering.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-138">hello **SalesforceId** can be null because it's only used for an update.</span></span>
   <span data-ttu-id="0f5fd-139">Vi få hello CRM-post med hjälp av hello CRM **PatientID** och hello **posttyp**.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-139">We get hello CRM record by using hello CRM **PatientID** and hello **Record Type**.</span></span>

2. <span data-ttu-id="0f5fd-140">Nu ska vi behöver tooadd vår DocumentDB API-app **InsertLogEntry** åtgärden som visas här i logik App Designer.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-140">Next, we need tooadd our DocumentDB API app **InsertLogEntry** operation as shown here in Logic App Designer.</span></span>

   <span data-ttu-id="0f5fd-141">**Infoga loggpost**</span><span class="sxs-lookup"><span data-stu-id="0f5fd-141">**Insert log entry**</span></span>

   ![Infoga loggpost](media/logic-apps-scenario-error-and-exception-handling/lognewpatient.png)

   <span data-ttu-id="0f5fd-143">**Infoga fel post**</span><span class="sxs-lookup"><span data-stu-id="0f5fd-143">**Insert error entry**</span></span>

   ![Infoga loggpost](media/logic-apps-scenario-error-and-exception-handling/insertlogentry.png)

   <span data-ttu-id="0f5fd-145">**Kontrollera att skapa poster fel**</span><span class="sxs-lookup"><span data-stu-id="0f5fd-145">**Check for create record failure**</span></span>

   ![Villkor](media/logic-apps-scenario-error-and-exception-handling/condition.png)

## <a name="logic-app-source-code"></a><span data-ttu-id="0f5fd-147">Källkoden för logik app</span><span class="sxs-lookup"><span data-stu-id="0f5fd-147">Logic app source code</span></span>

> [!NOTE]
> <span data-ttu-id="0f5fd-148">hello följande exempel är bara exempel.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-148">hello following examples are samples only.</span></span> <span data-ttu-id="0f5fd-149">Eftersom den här kursen är baserad på en implementering nu i produktion, hello värdet för en **Källnoden** kanske inte visar egenskaper som är relaterade tooscheduling ett möte. ></span><span class="sxs-lookup"><span data-stu-id="0f5fd-149">Because this tutorial is based on an implementation now in production, hello value of a **Source Node** might not display properties that are related tooscheduling an appointment.></span></span> 

### <a name="logging"></a><span data-ttu-id="0f5fd-150">Loggning</span><span class="sxs-lookup"><span data-stu-id="0f5fd-150">Logging</span></span>

<span data-ttu-id="0f5fd-151">hello följande logik Appkod exempel visar hur toohandle loggning.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-151">hello following logic app code sample shows how toohandle logging.</span></span>

#### <a name="log-entry"></a><span data-ttu-id="0f5fd-152">Loggpost</span><span class="sxs-lookup"><span data-stu-id="0f5fd-152">Log entry</span></span>

<span data-ttu-id="0f5fd-153">Här är hello logik app källkod för att infoga en loggpost.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-153">Here is hello logic app source code for inserting a log entry.</span></span>

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

#### <a name="log-request"></a><span data-ttu-id="0f5fd-154">Log-begäran</span><span class="sxs-lookup"><span data-stu-id="0f5fd-154">Log request</span></span>

<span data-ttu-id="0f5fd-155">Här är hello begäran loggmeddelande bokförd toohello API-app.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-155">Here is hello log request message posted toohello API app.</span></span>

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


#### <a name="log-response"></a><span data-ttu-id="0f5fd-156">Loggsvar</span><span class="sxs-lookup"><span data-stu-id="0f5fd-156">Log response</span></span>

<span data-ttu-id="0f5fd-157">Här är hello loggen svar från hello API-app.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-157">Here is hello log response message from hello API app.</span></span>

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

<span data-ttu-id="0f5fd-158">Nu ska vi titta på hello felhantering steg.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-158">Now let's look at hello error handling steps.</span></span>

### <a name="error-handling"></a><span data-ttu-id="0f5fd-159">Felhantering</span><span class="sxs-lookup"><span data-stu-id="0f5fd-159">Error handling</span></span>

<span data-ttu-id="0f5fd-160">hello visar följande logik app-kodexempel hur du kan implementera felhantering.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-160">hello following logic app code sample shows how you can implement error handling.</span></span>

#### <a name="create-error-record"></a><span data-ttu-id="0f5fd-161">Skapa Felpost</span><span class="sxs-lookup"><span data-stu-id="0f5fd-161">Create error record</span></span>

<span data-ttu-id="0f5fd-162">Här är hello logik app källkod för att skapa en Felpost.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-162">Here is hello logic app source code for creating an error record.</span></span>

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

#### <a name="insert-error-into-cosmos-db--request"></a><span data-ttu-id="0f5fd-163">Infoga fel i Cosmos-DB - begäran</span><span class="sxs-lookup"><span data-stu-id="0f5fd-163">Insert error into Cosmos DB--request</span></span>

``` json

{
    "uri": "https://.../api/CrMtoSfError",
    "method": "post",
    "body": {
        "action": "New_Patient",
        "isError": true,
        "crmId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "message": "Salesforce failed toocomplete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "providerId": "",
        "severity": 4,
        "salesforceId": "",
        "update": false,
        "source": "{/"Account_Class_vod__c/":/"PRAC/",/"Account_Status_MED__c/":/"I/",/"CRM_HUB_ID__c/":/"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0/",/"Credentials_vod__c/",/"DTC_ID_MED__c/":/"/",/"Fax/":/"/",/"FirstName/":/"A/",/"Gender_vod__c/":/"/",/"IMS_ID__c/":/"/",/"LastName/":/"BAILEY/",/"MasterID_mp__c/":/"/",/"C_ID_MED__c/":/"851588/",/"Middle_vod__c/":/"/",/"NPI_vod__c/":/"/",/"PDRP_MED__c/":false,/"PersonDoNotCall/":false,/"PersonEmail/":/"/",/"PersonHasOptedOutOfEmail/":false,/"PersonHasOptedOutOfFax/":false,/"PersonMobilePhone/":/"/",/"Phone/":/"/",/"Practicing_Specialty__c/":/"FM - FAMILY MEDICINE/",/"Primary_City__c/":/"/",/"Primary_State__c/":/"/",/"Primary_Street_Line2__c/":/"/",/"Primary_Street__c/":/"/",/"Primary_Zip__c/":/"/",/"RecordTypeId/":/"012U0000000JaPWIA0/",/"Request_Date__c/":/"2016-06-10T22:31:55.9647467Z/",/"ONY_ID__c/":/"/",/"Specialty_1_vod__c/":/"/",/"Suffix_vod__c/":/"/",/"Website/":/"/"}",
        "statusCode": "400"
    }
}
```

#### <a name="insert-error-into-cosmos-db--response"></a><span data-ttu-id="0f5fd-164">Infoga fel i Cosmos-DB - svar</span><span class="sxs-lookup"><span data-stu-id="0f5fd-164">Insert error into Cosmos DB--response</span></span>

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
        "body": "CRM failed toocomplete task: Message: duplicate value found: CRM_HUB_ID__c duplicates value on record with id: 001U000001c83gK",
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

#### <a name="salesforce-error-response"></a><span data-ttu-id="0f5fd-165">Salesforce felsvaret</span><span class="sxs-lookup"><span data-stu-id="0f5fd-165">Salesforce error response</span></span>

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
        "message": "Salesforce failed toocomplete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "source": "Salesforce.Common",
        "errors": []
    }
}

```

### <a name="return-hello-response-back-tooparent-logic-app"></a><span data-ttu-id="0f5fd-166">Returnera hello svar tillbaka tooparent logikapp</span><span class="sxs-lookup"><span data-stu-id="0f5fd-166">Return hello response back tooparent logic app</span></span>

<span data-ttu-id="0f5fd-167">När du får svar hello kan du överföra hello svar tillbaka toohello överordnade logikapp.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-167">After you get hello response, you can pass hello response back toohello parent logic app.</span></span>

#### <a name="return-success-response-tooparent-logic-app"></a><span data-ttu-id="0f5fd-168">Returnera lyckade svar tooparent logikapp</span><span class="sxs-lookup"><span data-stu-id="0f5fd-168">Return success response tooparent logic app</span></span>

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

#### <a name="return-error-response-tooparent-logic-app"></a><span data-ttu-id="0f5fd-169">Returnera fel svar tooparent logikapp</span><span class="sxs-lookup"><span data-stu-id="0f5fd-169">Return error response tooparent logic app</span></span>

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


## <a name="cosmos-db-repository-and-portal"></a><span data-ttu-id="0f5fd-170">Cosmos DB-databasen och -portalen</span><span class="sxs-lookup"><span data-stu-id="0f5fd-170">Cosmos DB repository and portal</span></span>

<span data-ttu-id="0f5fd-171">Vår lösning tillagda funktioner med [Cosmos DB](https://azure.microsoft.com/services/documentdb).</span><span class="sxs-lookup"><span data-stu-id="0f5fd-171">Our solution added capabilities with [Cosmos DB](https://azure.microsoft.com/services/documentdb).</span></span>

### <a name="error-management-portal"></a><span data-ttu-id="0f5fd-172">Fel-hanteringsportalen</span><span class="sxs-lookup"><span data-stu-id="0f5fd-172">Error management portal</span></span>

<span data-ttu-id="0f5fd-173">tooview hello fel, du kan skapa en MVC web app toodisplay hello felposter från Cosmos-databasen.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-173">tooview hello errors, you can create an MVC web app toodisplay hello error records from Cosmos DB.</span></span> <span data-ttu-id="0f5fd-174">Hej **lista**, **information**, **redigera**, och **ta bort** operations ingår i hello aktuell version.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-174">hello **List**, **Details**, **Edit**, and **Delete** operations are included in hello current version.</span></span>

> [!NOTE]
> <span data-ttu-id="0f5fd-175">Redigera åtgärd: Cosmos DB ersätter hello hela dokumentet.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-175">Edit operation: Cosmos DB replaces hello entire document.</span></span> <span data-ttu-id="0f5fd-176">Hej poster visas i hello **lista** och **detaljer** vyer är bara exempel.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-176">hello records shown in hello **List** and **Detail** views are samples only.</span></span> <span data-ttu-id="0f5fd-177">De är inte faktiska patient möte poster.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-177">They are not actual patient appointment records.</span></span>

<span data-ttu-id="0f5fd-178">Här följer exempel på vår MVC-app information som skapats tidigare med hello beskrivs metod.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-178">Here are examples of our MVC app details created with hello previously described approach.</span></span>

#### <a name="error-management-list"></a><span data-ttu-id="0f5fd-179">Fel vid hantering av lista</span><span class="sxs-lookup"><span data-stu-id="0f5fd-179">Error management list</span></span>
![Fel vid lista](media/logic-apps-scenario-error-and-exception-handling/errorlist.png)

#### <a name="error-management-detail-view"></a><span data-ttu-id="0f5fd-181">Fel vid hantering av detaljerad vy</span><span class="sxs-lookup"><span data-stu-id="0f5fd-181">Error management detail view</span></span>
![Information om fel](media/logic-apps-scenario-error-and-exception-handling/errordetails.png)

### <a name="log-management-portal"></a><span data-ttu-id="0f5fd-183">Log-hanteringsportalen</span><span class="sxs-lookup"><span data-stu-id="0f5fd-183">Log management portal</span></span>

<span data-ttu-id="0f5fd-184">tooview hello loggar vi också skapat en MVC-webbapp.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-184">tooview hello logs, we also created an MVC web app.</span></span> <span data-ttu-id="0f5fd-185">Här följer exempel på vår MVC-app information som skapats tidigare med hello beskrivs metod.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-185">Here are examples of our MVC app details created with hello previously described approach.</span></span>

#### <a name="sample-log-detail-view"></a><span data-ttu-id="0f5fd-186">Exempel loggen detaljvy</span><span class="sxs-lookup"><span data-stu-id="0f5fd-186">Sample log detail view</span></span>
![Loggen detaljvy](media/logic-apps-scenario-error-and-exception-handling/samplelogdetail.png)

### <a name="api-app-details"></a><span data-ttu-id="0f5fd-188">Information om API-app</span><span class="sxs-lookup"><span data-stu-id="0f5fd-188">API app details</span></span>

#### <a name="logic-apps-exception-management-api"></a><span data-ttu-id="0f5fd-189">Logic Apps undantag hanterings-API</span><span class="sxs-lookup"><span data-stu-id="0f5fd-189">Logic Apps exception management API</span></span>

<span data-ttu-id="0f5fd-190">Vår öppen källkod Azure Logikappar undantag hanterings-API-app innehåller funktioner som beskrivs här – det finns två domänkontrollanter:</span><span class="sxs-lookup"><span data-stu-id="0f5fd-190">Our open-source Azure Logic Apps exception management API app provides functionality as described here - there are two controllers:</span></span>

* <span data-ttu-id="0f5fd-191">**ErrorController** infogar en Felpost (dokument) i en DocumentDB-samling.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-191">**ErrorController** inserts an error record (document) in a DocumentDB collection.</span></span>
* <span data-ttu-id="0f5fd-192">**LogController** infogar en loggpost (dokument) i en DocumentDB-samling.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-192">**LogController** Inserts a log record (document) in a DocumentDB collection.</span></span>

> [!TIP]
> <span data-ttu-id="0f5fd-193">Både domänkontrollanter använder `async Task<dynamic>` åtgärder, så att operations tooresolve vid körning, så att vi kan skapa hello DocumentDB schemat i hello brödtext hello igen.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-193">Both controllers use `async Task<dynamic>` operations, allowing operations tooresolve at runtime, so we can create hello DocumentDB schema in hello body of hello operation.</span></span> 
> 

<span data-ttu-id="0f5fd-194">Alla dokument i DocumentDB måste ha ett unikt ID.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-194">Every document in DocumentDB must have a unique ID.</span></span> <span data-ttu-id="0f5fd-195">Vi använder `PatientId` och lägga till en tidsstämpel som är konverterade tooa Unix tidsstämpelvärde (double).</span><span class="sxs-lookup"><span data-stu-id="0f5fd-195">We are using `PatientId` and adding a timestamp that is converted tooa Unix timestamp value (double).</span></span> <span data-ttu-id="0f5fd-196">Vi trunkera hello värde tooremove hello bråkdelar värde.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-196">We truncate hello value tooremove hello fractional value.</span></span>

<span data-ttu-id="0f5fd-197">Du kan visa hello källkod i fel styrningen API [från GitHub](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi/blob/master/Logic App Exception Management API/Controllers/ErrorController.cs).</span><span class="sxs-lookup"><span data-stu-id="0f5fd-197">You can view hello source code of our error controller API [from GitHub](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi/blob/master/Logic App Exception Management API/Controllers/ErrorController.cs).</span></span>

<span data-ttu-id="0f5fd-198">Vi kallar hello API från en logikapp genom att använda hello följande syntax:</span><span class="sxs-lookup"><span data-stu-id="0f5fd-198">We call hello API from a logic app by using hello following syntax:</span></span>

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

<span data-ttu-id="0f5fd-199">Hej uttryck i hello föregående kod exempel söker efter hello *Create_NewPatientRecord* status för **misslyckades**.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-199">hello expression in hello preceding code sample checks for hello *Create_NewPatientRecord* status of **Failed**.</span></span>

## <a name="summary"></a><span data-ttu-id="0f5fd-200">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="0f5fd-200">Summary</span></span>

* <span data-ttu-id="0f5fd-201">Du kan enkelt implementera loggning och hantera fel i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-201">You can easily implement logging and error handling in a logic app.</span></span>
* <span data-ttu-id="0f5fd-202">Du kan använda DocumentDB som hello lagringsplats för logg- och fel-poster (dokument).</span><span class="sxs-lookup"><span data-stu-id="0f5fd-202">You can use DocumentDB as hello repository for log and error records (documents).</span></span>
* <span data-ttu-id="0f5fd-203">Du kan använda MVC toocreate en portal toodisplay logg och felposter.</span><span class="sxs-lookup"><span data-stu-id="0f5fd-203">You can use MVC toocreate a portal toodisplay log and error records.</span></span>

### <a name="source-code"></a><span data-ttu-id="0f5fd-204">Källkod</span><span class="sxs-lookup"><span data-stu-id="0f5fd-204">Source code</span></span>

<span data-ttu-id="0f5fd-205">hello källkoden för hello Logic Apps undantag management API-program finns i det här [GitHub-lagringsplatsen](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi "logik App undantag Management API").</span><span class="sxs-lookup"><span data-stu-id="0f5fd-205">hello source code for hello Logic Apps exception management API application is available in this [GitHub repository](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi "Logic App Exception Management API").</span></span>

## <a name="next-steps"></a><span data-ttu-id="0f5fd-206">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0f5fd-206">Next steps</span></span>

* [<span data-ttu-id="0f5fd-207">Visa mer logik app exempel och scenarier</span><span class="sxs-lookup"><span data-stu-id="0f5fd-207">View more logic app examples and scenarios</span></span>](../logic-apps/logic-apps-examples-and-scenarios.md)
* [<span data-ttu-id="0f5fd-208">Lär dig mer om hur du övervakar logikappar</span><span class="sxs-lookup"><span data-stu-id="0f5fd-208">Learn about monitoring logic apps</span></span>](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [<span data-ttu-id="0f5fd-209">Skapa mallar för automatisk distribution för logic apps</span><span class="sxs-lookup"><span data-stu-id="0f5fd-209">Create automated deployment templates for logic apps</span></span>](../logic-apps/logic-apps-create-deploy-template.md)
