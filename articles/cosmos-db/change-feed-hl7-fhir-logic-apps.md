---
title: "aaaChange feed för HL7 FHIR resurser - Azure Cosmos DB | Microsoft Docs"
description: "Lär dig hur tooset in meddelanden om filändringar för HL7 FHIR hälsovård patientjournaler med Azure Logikappar, Azure Cosmos DB och Service Bus."
keywords: HL7 fhir
services: cosmos-db
author: hedidin
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 0d25c11f-9197-419a-aa19-4614c6ab2d06
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: b-hoedid
ms.openlocfilehash: d2809bf5c6d8c193c49438d20684c56caea646bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="notifying-patients-of-hl7-fhir-health-care-record-changes-using-logic-apps-and-azure-cosmos-db"></a><span data-ttu-id="8ffae-104">Meddela patienter HL7 FHIR hälsovård post ändringar med hjälp av Logic Apps och Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="8ffae-104">Notifying patients of HL7 FHIR health care record changes using Logic Apps and Azure Cosmos DB</span></span>

<span data-ttu-id="8ffae-105">Azure MVP Howard Edidin kontaktades nyligen av en sjukvårdsorganisation som ville ha tooadd nya funktioner tootheir patient portalen.</span><span class="sxs-lookup"><span data-stu-id="8ffae-105">Azure MVP Howard Edidin was recently contacted by a healthcare organization that wanted tooadd new functionality tootheir patient portal.</span></span> <span data-ttu-id="8ffae-106">De behövs toosend meddelanden toopatients när deras hälsa posten har uppdaterats och de behövs patienter toobe kan toosubscribe toothese uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="8ffae-106">They needed toosend notifications toopatients when their health record was updated, and they needed patients toobe able toosubscribe toothese updates.</span></span> 

<span data-ttu-id="8ffae-107">Den här artikeln beskriver hur hello ändra feed meddelande lösning som har skapats för den här sjukvårdsorganisation med Azure Cosmos DB, Logic Apps och Service Bus.</span><span class="sxs-lookup"><span data-stu-id="8ffae-107">This article walks through hello change feed notification solution created for this healthcare organization using Azure Cosmos DB, Logic Apps, and Service Bus.</span></span> 

## <a name="project-requirements"></a><span data-ttu-id="8ffae-108">Projektkrav för</span><span class="sxs-lookup"><span data-stu-id="8ffae-108">Project requirements</span></span>
- <span data-ttu-id="8ffae-109">Providers skicka HL7 konsoliderade kliniska dokumentet arkitektur (C-CDA) dokument i XML-format.</span><span class="sxs-lookup"><span data-stu-id="8ffae-109">Providers send HL7 Consolidated-Clinical Document Architecture (C-CDA) documents in XML format.</span></span> <span data-ttu-id="8ffae-110">C-CDA dokument omfattar nästan alla typer av kliniska dokument, inklusive kliniska dokument, till exempel family historik och vaccination poster, samt som administrativa, arbetsflöden och ekonomiska dokument.</span><span class="sxs-lookup"><span data-stu-id="8ffae-110">C-CDA documents encompass just about every type of clinical document, including clinical documents such as family histories and immunization records, as well as administrative, workflow, and financial documents.</span></span> 
- <span data-ttu-id="8ffae-111">C-CDA dokument konverteras för[HL7 FHIR resurser](http://hl7.org/fhir/2017Jan/resourcelist.html) i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="8ffae-111">C-CDA documents are converted too[HL7 FHIR Resources](http://hl7.org/fhir/2017Jan/resourcelist.html) in JSON format.</span></span>
- <span data-ttu-id="8ffae-112">Ändrade FHIR resurs dokument skickas via e-post i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="8ffae-112">Modified FHIR resource documents are sent by email in JSON format.</span></span>

## <a name="solution-workflow"></a><span data-ttu-id="8ffae-113">Lösning för arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="8ffae-113">Solution workflow</span></span> 

<span data-ttu-id="8ffae-114">På en hög nivå hello som krävs för hello följande arbetsflöde:</span><span class="sxs-lookup"><span data-stu-id="8ffae-114">At a high level, hello project required hello following workflow steps:</span></span> 
1. <span data-ttu-id="8ffae-115">Konvertera C CDA dokument tooFHIR resurser.</span><span class="sxs-lookup"><span data-stu-id="8ffae-115">Convert C-CDA documents tooFHIR resources.</span></span>
2. <span data-ttu-id="8ffae-116">Utför återkommande utlösare avsöker för ändrade FHIR resurser.</span><span class="sxs-lookup"><span data-stu-id="8ffae-116">Perform recurring trigger polling for modified FHIR resources.</span></span> 
2. <span data-ttu-id="8ffae-117">Anropa en anpassad app, FhirNotificationApi, tooconnect tooAzure Cosmos DB och fråga efter nya eller ändrade dokument.</span><span class="sxs-lookup"><span data-stu-id="8ffae-117">Call a custom app, FhirNotificationApi, tooconnect tooAzure Cosmos DB and query for new or modified documents.</span></span>
3. <span data-ttu-id="8ffae-118">Spara hello svar tootoohello Service Bus-kö.</span><span class="sxs-lookup"><span data-stu-id="8ffae-118">Save hello response tootoohello Service Bus queue.</span></span>
4. <span data-ttu-id="8ffae-119">Avsökning för nya meddelanden i hello Service Bus-kö.</span><span class="sxs-lookup"><span data-stu-id="8ffae-119">Poll for new messages in hello Service Bus queue.</span></span>
5. <span data-ttu-id="8ffae-120">Skicka e-postaviseringar toopatients.</span><span class="sxs-lookup"><span data-stu-id="8ffae-120">Send email notifications toopatients.</span></span>

## <a name="solution-architecture"></a><span data-ttu-id="8ffae-121">Lösningsarkitektur</span><span class="sxs-lookup"><span data-stu-id="8ffae-121">Solution architecture</span></span>
<span data-ttu-id="8ffae-122">Denna lösning kräver tre Logic Apps toomeet hello ovan krav och fullständig hello lösning arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="8ffae-122">This solution requires three Logic Apps toomeet hello above requirements and complete hello solution workflow.</span></span> <span data-ttu-id="8ffae-123">hello tre logic apps är:</span><span class="sxs-lookup"><span data-stu-id="8ffae-123">hello three logic apps are:</span></span>
1. <span data-ttu-id="8ffae-124">**HL7-FHIR-mappning app**: tar emot hello HL7 C-CDA dokument, omvandlar den toohello FHIR resurs och sparar den tooAzure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="8ffae-124">**HL7-FHIR-Mapping app**: Receives hello HL7 C-CDA document, transforms it toohello FHIR Resource, then saves it tooAzure Cosmos DB.</span></span>
2. <span data-ttu-id="8ffae-125">**EHR app**: frågar hello Azure Cosmos DB FHIR databasen och sparar hello svar tooa Service Bus-kö.</span><span class="sxs-lookup"><span data-stu-id="8ffae-125">**EHR app**: Queries hello Azure Cosmos DB FHIR repository and saves hello response tooa Service Bus queue.</span></span> <span data-ttu-id="8ffae-126">Den här logikapp använder en [API-app](#api-app) tooretrieve nya och ändrade dokument.</span><span class="sxs-lookup"><span data-stu-id="8ffae-126">This logic app uses an [API app](#api-app) tooretrieve new and changed documents.</span></span>
3. <span data-ttu-id="8ffae-127">**Processen meddelandeprogram**: skickar ett e-postmeddelande med hello FHIR resurs dokument i hello brödtext.</span><span class="sxs-lookup"><span data-stu-id="8ffae-127">**Process notification app**: Sends an email notification with hello FHIR resource documents in hello body.</span></span>

![hello tre Logic Apps används i den här HL7 FHIR sjukvården lösningen](./media/change-feed-hl7-fhir-logic-apps/health-care-solution-hl7-fhir.png)



### <a name="azure-services-used-in-hello-solution"></a><span data-ttu-id="8ffae-129">Azure-tjänster används i hello lösningar</span><span class="sxs-lookup"><span data-stu-id="8ffae-129">Azure services used in hello solution</span></span>

#### <a name="azure-cosmos-db-documentdb-api"></a><span data-ttu-id="8ffae-130">Azure Cosmos DB DocumentDB API</span><span class="sxs-lookup"><span data-stu-id="8ffae-130">Azure Cosmos DB DocumentDB API</span></span>
<span data-ttu-id="8ffae-131">Azure Cosmos-DB är hello lagringsplats för hello FHIR resurser som visas i följande bild hello.</span><span class="sxs-lookup"><span data-stu-id="8ffae-131">Azure Cosmos DB is hello repository for hello FHIR resources as shown in hello following figure.</span></span>

![hello Azure Cosmos DB konto som används i självstudierna HL7 FHIR sjukvården](./media/change-feed-hl7-fhir-logic-apps/account.png)

#### <a name="logic-apps"></a><span data-ttu-id="8ffae-133">Logic Apps</span><span class="sxs-lookup"><span data-stu-id="8ffae-133">Logic Apps</span></span>
<span data-ttu-id="8ffae-134">Logic Apps hantera hello arbetsflödesprocessen.</span><span class="sxs-lookup"><span data-stu-id="8ffae-134">Logic Apps handle hello workflow process.</span></span> <span data-ttu-id="8ffae-135">hello visar följande skärmdumpar hello Logic apps som skapats för den här lösningen.</span><span class="sxs-lookup"><span data-stu-id="8ffae-135">hello following screenshots show hello Logic apps created for this solution.</span></span> 


1. <span data-ttu-id="8ffae-136">**HL7-FHIR-mappning app**: får hello HL7 C-CDA dokument och transformera dem tooan FHIR resursen med hjälp av hello Enterprise-Integrationspaket för Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="8ffae-136">**HL7-FHIR-Mapping app**: Receive hello HL7 C-CDA document and transform it tooan FHIR resource using hello Enterprise Integration Pack for Logic Apps.</span></span> <span data-ttu-id="8ffae-137">hello Enterprise-Integrationspaket hanterar hello mappning från hello C CDA tooFHIR resurser.</span><span class="sxs-lookup"><span data-stu-id="8ffae-137">hello Enterprise Integration Pack handles hello mapping from hello C-CDA tooFHIR resources.</span></span>

    ![Hej Logikapp används tooreceive HL7 FHIR sjukvården poster](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-json-transform.png)


2. <span data-ttu-id="8ffae-139">**EHR app**: fråga hello Azure Cosmos DB FHIR databasen och spara hello svar tooa Service Bus-kö.</span><span class="sxs-lookup"><span data-stu-id="8ffae-139">**EHR app**: Query hello Azure Cosmos DB FHIR repository and save hello response tooa Service Bus queue.</span></span> <span data-ttu-id="8ffae-140">hello-koden för hello GetNewOrModifiedFHIRDocuments app är nedan.</span><span class="sxs-lookup"><span data-stu-id="8ffae-140">hello code for hello GetNewOrModifiedFHIRDocuments app is below.</span></span>

    ![Hej Logikapp används tooquery Azure Cosmos DB](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-api-app.png)

3. <span data-ttu-id="8ffae-142">**Processen meddelandeprogram**: skicka ett e-postmeddelande med hello FHIR resurs dokument i hello brödtext.</span><span class="sxs-lookup"><span data-stu-id="8ffae-142">**Process notification app**: Send an email notification with hello FHIR resource documents in hello body.</span></span>

    ![Hej Logikappen som skickar patient e-postmeddelande med hello HL7 FHIR resurs i hello brödtext](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-send-email.png)

#### <a name="service-bus"></a><span data-ttu-id="8ffae-144">Service Bus</span><span class="sxs-lookup"><span data-stu-id="8ffae-144">Service Bus</span></span>
<span data-ttu-id="8ffae-145">Följande bild visar hello patienter kön hello.</span><span class="sxs-lookup"><span data-stu-id="8ffae-145">hello following figure shows hello patients queue.</span></span> <span data-ttu-id="8ffae-146">hello taggen egenskapens värde används för hello e-postmeddelandets ämne.</span><span class="sxs-lookup"><span data-stu-id="8ffae-146">hello Tag property value is used for hello email subject.</span></span>

![hello används i den här självstudiekursen HL7 FHIR Service Bus-kö](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-service-bus-queue.png)

<a id="api-app"></a>

#### <a name="api-app"></a><span data-ttu-id="8ffae-148">API-app</span><span class="sxs-lookup"><span data-stu-id="8ffae-148">API app</span></span>
<span data-ttu-id="8ffae-149">En API-app ansluter tooAzure Cosmos DB och frågor om nya eller ändrade FHIR dokument uppdelat efter resurstyp.</span><span class="sxs-lookup"><span data-stu-id="8ffae-149">An API app connects tooAzure Cosmos DB and queries for new or modified FHIR documents By resource type.</span></span> <span data-ttu-id="8ffae-150">Det här programmet har en domänkontrollant **FhirNotificationApi** med en åtgärd **GetNewOrModifiedFhirDocuments**, se [källa för API-app](#api-app-source).</span><span class="sxs-lookup"><span data-stu-id="8ffae-150">This app has one controller, **FhirNotificationApi** with a one operation **GetNewOrModifiedFhirDocuments**, see [source for API app](#api-app-source).</span></span>

<span data-ttu-id="8ffae-151">Vi använder hello [ `CreateDocumentChangeFeedQuery` ](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery.aspx) klass från hello Cosmos-DocumentDB på Azure DB .NET API: et.</span><span class="sxs-lookup"><span data-stu-id="8ffae-151">We are using hello [`CreateDocumentChangeFeedQuery`](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery.aspx) class from hello Azure Cosmos DB DocumentDB .NET API.</span></span> <span data-ttu-id="8ffae-152">Mer information finns i hello [ändra feed artikel](change-feed.md).</span><span class="sxs-lookup"><span data-stu-id="8ffae-152">For more information, see hello [change feed article](change-feed.md).</span></span> 

##### <a name="getnewormodifiedfhirdocuments-operation"></a><span data-ttu-id="8ffae-153">GetNewOrModifiedFhirDocuments åtgärden</span><span class="sxs-lookup"><span data-stu-id="8ffae-153">GetNewOrModifiedFhirDocuments operation</span></span>

<span data-ttu-id="8ffae-154">**Indata**</span><span class="sxs-lookup"><span data-stu-id="8ffae-154">**Inputs**</span></span>
- <span data-ttu-id="8ffae-155">DatabaseId</span><span class="sxs-lookup"><span data-stu-id="8ffae-155">DatabaseId</span></span>
- <span data-ttu-id="8ffae-156">CollectionId</span><span class="sxs-lookup"><span data-stu-id="8ffae-156">CollectionId</span></span>
- <span data-ttu-id="8ffae-157">Resurstyp för HL7 FHIR namn</span><span class="sxs-lookup"><span data-stu-id="8ffae-157">HL7 FHIR Resource Type name</span></span>
- <span data-ttu-id="8ffae-158">Boolean: Starta från början</span><span class="sxs-lookup"><span data-stu-id="8ffae-158">Boolean: Start from Beginning</span></span>
- <span data-ttu-id="8ffae-159">Int: Antal dokument som returneras</span><span class="sxs-lookup"><span data-stu-id="8ffae-159">Int: Number of documents returned</span></span>

<span data-ttu-id="8ffae-160">**Utdata**</span><span class="sxs-lookup"><span data-stu-id="8ffae-160">**Outputs**</span></span>
- <span data-ttu-id="8ffae-161">Lyckades: Statuskod: 200, svar: listan över dokument (JSON-matris)</span><span class="sxs-lookup"><span data-stu-id="8ffae-161">Success: Status Code: 200, Response: List of Documents (JSON Array)</span></span>
- <span data-ttu-id="8ffae-162">Fel: Statuskod: 404, svar ”: inga dokument hittades för”*resursnamnet '* resurstypen ”</span><span class="sxs-lookup"><span data-stu-id="8ffae-162">Failure: Status Code: 404, Response: "No Documents found for '*resource name'* Resource Type"</span></span>

<a id="api-app-source"></a>

<span data-ttu-id="8ffae-163">**Källa för hello API-app**</span><span class="sxs-lookup"><span data-stu-id="8ffae-163">**Source for hello API app**</span></span>

```C#

    using System.Collections.Generic;
    using System.Linq;
    using System.Net;
    using System.Net.Http;
    using System.Threading.Tasks;
    using System.Web.Http;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Swashbuckle.Swagger.Annotations;
    using TRex.Metadata;
    
    namespace FhirNotificationApi.Controllers
    {
        /// <summary>
        ///     FHIR Resource Type Controller
        /// </summary>
        /// <seealso cref="System.Web.Http.ApiController" />
        public class FhirResourceTypeController : ApiController
        {
            /// <summary>
            ///     Gets hello new or modified FHIR documents from Last Run Date 
            ///     or create date of hello collection
            /// </summary>
            /// <param name="databaseId"></param>
            /// <param name="collectionId"></param>
            /// <param name="resourceType"></param>
            /// <param name="startfromBeginning"></param>
            /// <param name="maximumItemCount">-1 returns all (default)</param>
            /// <returns></returns>
            [Metadata("Get New or Modified FHIR Documents",
                "Query for new or modifed FHIR Documents By Resource Type " +
                "from Last Run Date or Begining of Collection creation"
            )]
            [SwaggerResponse(HttpStatusCode.OK, type: typeof(Task<dynamic>))]
            [SwaggerResponse(HttpStatusCode.NotFound, "No New or Modifed Documents found")]
            [SwaggerOperation("GetNewOrModifiedFHIRDocuments")]
            public async Task<dynamic> GetNewOrModifiedFhirDocuments(
                [Metadata("Database Id", "Database Id")] string databaseId,
                [Metadata("Collection Id", "Collection Id")] string collectionId,
                [Metadata("Resource Type", "FHIR resource type name")] string resourceType,
                [Metadata("Start from Beginning ", "Change Feed Option")] bool startfromBeginning,
                [Metadata("Maximum Item Count", "Number of documents returned. '-1 returns all' (default)")] int maximumItemCount = -1
            )
            {
                var collectionLink = UriFactory.CreateDocumentCollectionUri(databaseId, collectionId);
    
                var context = new DocumentDbContext();  
    
                var docs = new List<dynamic>();
    
                var partitionKeyRanges = new List<PartitionKeyRange>();
                FeedResponse<PartitionKeyRange> pkRangesResponse;
    
                do
                {
                    pkRangesResponse = await context.Client.ReadPartitionKeyRangeFeedAsync(collectionLink);
                    partitionKeyRanges.AddRange(pkRangesResponse);
                } while (pkRangesResponse.ResponseContinuation != null);
    
                foreach (var pkRange in partitionKeyRanges)
                {
                    var changeFeedOptions = new ChangeFeedOptions
                    {
                        StartFromBeginning = startfromBeginning,
                        RequestContinuation = null,
                        MaxItemCount = maximumItemCount,
                        PartitionKeyRangeId = pkRange.Id
                    };
    
                    using (var query = context.Client.CreateDocumentChangeFeedQuery(collectionLink, changeFeedOptions))
                    {
                        do
                        {
                            if (query != null)
                            {
                                var results = await query.ExecuteNextAsync<dynamic>().ConfigureAwait(false);
                                if (results.Count > 0)
                                    docs.AddRange(results.Where(doc => doc.resourceType == resourceType));
                            }
                            else
                            {
                                throw new HttpResponseException(new HttpResponseMessage(HttpStatusCode.NotFound));
                            }
                        } while (query.HasMoreResults);
                    }
                }
                if (docs.Count > 0)
                    return docs;
                var msg = new StringContent("No documents found for " + resourceType + " Resource");
                var response = new HttpResponseMessage
                {
                    StatusCode = HttpStatusCode.NotFound,
                    Content = msg
                };
                return response;
            }
        }
    }
    
```

### <a name="testing-hello-fhirnotificationapi"></a><span data-ttu-id="8ffae-164">Testa hello FhirNotificationApi</span><span class="sxs-lookup"><span data-stu-id="8ffae-164">Testing hello FhirNotificationApi</span></span> 

<span data-ttu-id="8ffae-165">hello följande bild visar hur swagger har använt tootootest hello [FhirNotificationApi](#api-app-source).</span><span class="sxs-lookup"><span data-stu-id="8ffae-165">hello following image demonstrates how swagger was used tootootest hello [FhirNotificationApi](#api-app-source).</span></span>

![Hej Swagger-filen används tootest hello API-app](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-testing-app.png)


### <a name="azure-portal-dashboard"></a><span data-ttu-id="8ffae-167">Azure portalens instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="8ffae-167">Azure portal dashboard</span></span>

<span data-ttu-id="8ffae-168">hello följande bild visar alla hello Azure-tjänster för den här lösningen som körs i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8ffae-168">hello following image shows all of hello Azure services for this solution running in hello Azure portal.</span></span>

![hello Azure portal som visar alla hello-tjänster som används i den här självstudiekursen HL7 FHIR](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-portal.png)


## <a name="summary"></a><span data-ttu-id="8ffae-170">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="8ffae-170">Summary</span></span>

- <span data-ttu-id="8ffae-171">Du har lärt dig att Azure Cosmos DB har inbyggda relationstypen för meddelanden om nya eller ändras dokument och hur lätt det är toouse.</span><span class="sxs-lookup"><span data-stu-id="8ffae-171">You have learned that Azure Cosmos DB has native suppport for notifications for new or modifed documents and how easy it is toouse.</span></span> 
- <span data-ttu-id="8ffae-172">Du kan skapa arbetsflöden utan att skriva någon kod genom att utnyttja Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="8ffae-172">By leveraging Logic Apps, you can create workflows without writing any code.</span></span>
- <span data-ttu-id="8ffae-173">Med hjälp av Azure Service Bus-köer toohandle hello distribution för hello HL7 FHIR dokument.</span><span class="sxs-lookup"><span data-stu-id="8ffae-173">Using Azure Service Bus Queues toohandle hello distribution for hello HL7 FHIR documents.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8ffae-174">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8ffae-174">Next steps</span></span>
<span data-ttu-id="8ffae-175">Mer information om Azure Cosmos DB finns hello [Azure Cosmos DB startsidan](https://azure.microsoft.com/services/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="8ffae-175">For more information about Azure Cosmos DB, see hello [Azure Cosmos DB home page](https://azure.microsoft.com/services/cosmos-db/).</span></span> <span data-ttu-id="8ffae-176">Mer information om Logic Apps finns i [Logikappar](https://azure.microsoft.com/services/logic-apps/).</span><span class="sxs-lookup"><span data-stu-id="8ffae-176">For more informaiton about Logic Apps, see [Logic Apps](https://azure.microsoft.com/services/logic-apps/).</span></span>


