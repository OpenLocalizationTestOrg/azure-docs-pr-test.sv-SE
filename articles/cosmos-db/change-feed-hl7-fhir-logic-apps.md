---
title: "Ändra feed för HL7 FHIR resurser - Azure Cosmos DB | Microsoft Docs"
description: "Lär dig hur du ställer in ändringsmeddelanden för HL7 FHIR hälsovård patientjournaler med Azure Logikappar, Azure Cosmos DB och Service Bus."
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
ms.openlocfilehash: d2b50c0b6864af41fb9cfa051721c432772b228d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="notifying-patients-of-hl7-fhir-health-care-record-changes-using-logic-apps-and-azure-cosmos-db"></a><span data-ttu-id="fd6d1-104">Meddela patienter HL7 FHIR hälsovård post ändringar med hjälp av Logic Apps och Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="fd6d1-104">Notifying patients of HL7 FHIR health care record changes using Logic Apps and Azure Cosmos DB</span></span>

<span data-ttu-id="fd6d1-105">Azure MVP Howard Edidin kontaktades nyligen av en sjukvårdsorganisation som du vill lägga till nya funktioner till deras patient portal.</span><span class="sxs-lookup"><span data-stu-id="fd6d1-105">Azure MVP Howard Edidin was recently contacted by a healthcare organization that wanted to add new functionality to their patient portal.</span></span> <span data-ttu-id="fd6d1-106">De behövs för att skicka meddelanden till patienter när deras hälsa posten har uppdaterats och de behövs patienter för att kunna prenumerera på dessa uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="fd6d1-106">They needed to send notifications to patients when their health record was updated, and they needed patients to be able to subscribe to these updates.</span></span> 

<span data-ttu-id="fd6d1-107">Den här artikeln beskriver hur ändringen meddelande lösning som har skapats för den här sjukvårdsorganisation med Azure Cosmos DB, Logic Apps och Service Bus-feed.</span><span class="sxs-lookup"><span data-stu-id="fd6d1-107">This article walks through the change feed notification solution created for this healthcare organization using Azure Cosmos DB, Logic Apps, and Service Bus.</span></span> 

## <a name="project-requirements"></a><span data-ttu-id="fd6d1-108">Projektkrav för</span><span class="sxs-lookup"><span data-stu-id="fd6d1-108">Project requirements</span></span>
- <span data-ttu-id="fd6d1-109">Providers skicka HL7 konsoliderade kliniska dokumentet arkitektur (C-CDA) dokument i XML-format.</span><span class="sxs-lookup"><span data-stu-id="fd6d1-109">Providers send HL7 Consolidated-Clinical Document Architecture (C-CDA) documents in XML format.</span></span> <span data-ttu-id="fd6d1-110">C-CDA dokument omfattar nästan alla typer av kliniska dokument, inklusive kliniska dokument, till exempel family historik och vaccination poster, samt som administrativa, arbetsflöden och ekonomiska dokument.</span><span class="sxs-lookup"><span data-stu-id="fd6d1-110">C-CDA documents encompass just about every type of clinical document, including clinical documents such as family histories and immunization records, as well as administrative, workflow, and financial documents.</span></span> 
- <span data-ttu-id="fd6d1-111">C-CDA dokument konverteras till [HL7 FHIR resurser](http://hl7.org/fhir/2017Jan/resourcelist.html) i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="fd6d1-111">C-CDA documents are converted to [HL7 FHIR Resources](http://hl7.org/fhir/2017Jan/resourcelist.html) in JSON format.</span></span>
- <span data-ttu-id="fd6d1-112">Ändrade FHIR resurs dokument skickas via e-post i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="fd6d1-112">Modified FHIR resource documents are sent by email in JSON format.</span></span>

## <a name="solution-workflow"></a><span data-ttu-id="fd6d1-113">Lösning för arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="fd6d1-113">Solution workflow</span></span> 

<span data-ttu-id="fd6d1-114">På en hög nivå krävs projektet följande arbetsflödessteg:</span><span class="sxs-lookup"><span data-stu-id="fd6d1-114">At a high level, the project required the following workflow steps:</span></span> 
1. <span data-ttu-id="fd6d1-115">Konvertera C CDA dokument till FHIR resurser.</span><span class="sxs-lookup"><span data-stu-id="fd6d1-115">Convert C-CDA documents to FHIR resources.</span></span>
2. <span data-ttu-id="fd6d1-116">Utför återkommande utlösare avsöker för ändrade FHIR resurser.</span><span class="sxs-lookup"><span data-stu-id="fd6d1-116">Perform recurring trigger polling for modified FHIR resources.</span></span> 
2. <span data-ttu-id="fd6d1-117">Anropa en anpassad app FhirNotificationApi att ansluta till Azure Cosmos DB och fråga efter nya eller ändrade dokument.</span><span class="sxs-lookup"><span data-stu-id="fd6d1-117">Call a custom app, FhirNotificationApi, to connect to Azure Cosmos DB and query for new or modified documents.</span></span>
3. <span data-ttu-id="fd6d1-118">Spara svar om du vill att Service Bus-kö.</span><span class="sxs-lookup"><span data-stu-id="fd6d1-118">Save the response to to the Service Bus queue.</span></span>
4. <span data-ttu-id="fd6d1-119">Avsökning för nya meddelanden i Service Bus-kö.</span><span class="sxs-lookup"><span data-stu-id="fd6d1-119">Poll for new messages in the Service Bus queue.</span></span>
5. <span data-ttu-id="fd6d1-120">Skicka e-postmeddelanden till patienter.</span><span class="sxs-lookup"><span data-stu-id="fd6d1-120">Send email notifications to patients.</span></span>

## <a name="solution-architecture"></a><span data-ttu-id="fd6d1-121">Lösningsarkitektur</span><span class="sxs-lookup"><span data-stu-id="fd6d1-121">Solution architecture</span></span>
<span data-ttu-id="fd6d1-122">Denna lösning kräver tre Logic Apps att uppfylla kraven ovan och slutföra arbetsflödet lösning.</span><span class="sxs-lookup"><span data-stu-id="fd6d1-122">This solution requires three Logic Apps to meet the above requirements and complete the solution workflow.</span></span> <span data-ttu-id="fd6d1-123">Tre logikappar är:</span><span class="sxs-lookup"><span data-stu-id="fd6d1-123">The three logic apps are:</span></span>
1. <span data-ttu-id="fd6d1-124">**HL7-FHIR-mappning app**: tar emot HL7 C-CDA dokumentet, omvandlar till FHIR resursen och sedan sparar det på Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="fd6d1-124">**HL7-FHIR-Mapping app**: Receives the HL7 C-CDA document, transforms it to the FHIR Resource, then saves it to Azure Cosmos DB.</span></span>
2. <span data-ttu-id="fd6d1-125">**EHR app**: frågar Azure Cosmos DB FHIR databasen och sparar svar till en Service Bus-kö.</span><span class="sxs-lookup"><span data-stu-id="fd6d1-125">**EHR app**: Queries the Azure Cosmos DB FHIR repository and saves the response to a Service Bus queue.</span></span> <span data-ttu-id="fd6d1-126">Den här logikapp använder en [API-app](#api-app) att hämta nya och ändrade dokument.</span><span class="sxs-lookup"><span data-stu-id="fd6d1-126">This logic app uses an [API app](#api-app) to retrieve new and changed documents.</span></span>
3. <span data-ttu-id="fd6d1-127">**Processen meddelandeprogram**: skickar ett e-postmeddelande med FHIR resurs dokument i brödtexten.</span><span class="sxs-lookup"><span data-stu-id="fd6d1-127">**Process notification app**: Sends an email notification with the FHIR resource documents in the body.</span></span>

![Tre Logic Apps som används i den här HL7 FHIR sjukvården lösningen](./media/change-feed-hl7-fhir-logic-apps/health-care-solution-hl7-fhir.png)



### <a name="azure-services-used-in-the-solution"></a><span data-ttu-id="fd6d1-129">Azure-tjänster används i lösningen</span><span class="sxs-lookup"><span data-stu-id="fd6d1-129">Azure services used in the solution</span></span>

#### <a name="azure-cosmos-db-documentdb-api"></a><span data-ttu-id="fd6d1-130">Azure Cosmos DB DocumentDB API</span><span class="sxs-lookup"><span data-stu-id="fd6d1-130">Azure Cosmos DB DocumentDB API</span></span>
<span data-ttu-id="fd6d1-131">Azure Cosmos-DB är lagringsplatsen för FHIR resurser som visas i följande bild.</span><span class="sxs-lookup"><span data-stu-id="fd6d1-131">Azure Cosmos DB is the repository for the FHIR resources as shown in the following figure.</span></span>

![Azure DB som Cosmos-kontot som används i självstudierna HL7 FHIR sjukvården](./media/change-feed-hl7-fhir-logic-apps/account.png)

#### <a name="logic-apps"></a><span data-ttu-id="fd6d1-133">Logic Apps</span><span class="sxs-lookup"><span data-stu-id="fd6d1-133">Logic Apps</span></span>
<span data-ttu-id="fd6d1-134">Logic Apps hantera arbetsflödesprocessen.</span><span class="sxs-lookup"><span data-stu-id="fd6d1-134">Logic Apps handle the workflow process.</span></span> <span data-ttu-id="fd6d1-135">Följande skärmdumpar visar logikappar som skapats för den här lösningen.</span><span class="sxs-lookup"><span data-stu-id="fd6d1-135">The following screenshots show the Logic apps created for this solution.</span></span> 


1. <span data-ttu-id="fd6d1-136">**HL7-FHIR-mappning app**: ta emot HL7 C-CDA dokumentet och omvandla dem till en resurs för FHIR använder Enterprise-Integrationspaket för Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="fd6d1-136">**HL7-FHIR-Mapping app**: Receive the HL7 C-CDA document and transform it to an FHIR resource using the Enterprise Integration Pack for Logic Apps.</span></span> <span data-ttu-id="fd6d1-137">Enterprise-Integrationspaket hanterar mappning från C-CDA FHIR resurser.</span><span class="sxs-lookup"><span data-stu-id="fd6d1-137">The Enterprise Integration Pack handles the mapping from the C-CDA to FHIR resources.</span></span>

    ![Logikappen som används för att ta emot HL7 FHIR sjukvården poster](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-json-transform.png)


2. <span data-ttu-id="fd6d1-139">**EHR app**: fråga Azure Cosmos DB FHIR databasen och spara svar till en Service Bus-kö.</span><span class="sxs-lookup"><span data-stu-id="fd6d1-139">**EHR app**: Query the Azure Cosmos DB FHIR repository and save the response to a Service Bus queue.</span></span> <span data-ttu-id="fd6d1-140">Koden för GetNewOrModifiedFHIRDocuments appen är nedan.</span><span class="sxs-lookup"><span data-stu-id="fd6d1-140">The code for the GetNewOrModifiedFHIRDocuments app is below.</span></span>

    ![Logikappen som används för att fråga Azure Cosmos DB](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-api-app.png)

3. <span data-ttu-id="fd6d1-142">**Processen meddelandeprogram**: skicka ett e-postmeddelande med FHIR resurs dokument i.</span><span class="sxs-lookup"><span data-stu-id="fd6d1-142">**Process notification app**: Send an email notification with the FHIR resource documents in the body.</span></span>

    ![Logikappen som skickar patient e-post med HL7 FHIR resursen i brödtexten](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-send-email.png)

#### <a name="service-bus"></a><span data-ttu-id="fd6d1-144">Service Bus</span><span class="sxs-lookup"><span data-stu-id="fd6d1-144">Service Bus</span></span>
<span data-ttu-id="fd6d1-145">Följande bild visar patienterna kön.</span><span class="sxs-lookup"><span data-stu-id="fd6d1-145">The following figure shows the patients queue.</span></span> <span data-ttu-id="fd6d1-146">Taggen egenskapens värde används för ämnet för e-post.</span><span class="sxs-lookup"><span data-stu-id="fd6d1-146">The Tag property value is used for the email subject.</span></span>

![Service Bus-kö som används i den här självstudiekursen HL7 FHIR](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-service-bus-queue.png)

<a id="api-app"></a>

#### <a name="api-app"></a><span data-ttu-id="fd6d1-148">API-app</span><span class="sxs-lookup"><span data-stu-id="fd6d1-148">API app</span></span>
<span data-ttu-id="fd6d1-149">En API-app ansluter till Azure Cosmos DB och frågor om nya eller ändrade FHIR dokument uppdelat efter resurstyp.</span><span class="sxs-lookup"><span data-stu-id="fd6d1-149">An API app connects to Azure Cosmos DB and queries for new or modified FHIR documents By resource type.</span></span> <span data-ttu-id="fd6d1-150">Det här programmet har en domänkontrollant **FhirNotificationApi** med en åtgärd **GetNewOrModifiedFhirDocuments**, se [källa för API-app](#api-app-source).</span><span class="sxs-lookup"><span data-stu-id="fd6d1-150">This app has one controller, **FhirNotificationApi** with a one operation **GetNewOrModifiedFhirDocuments**, see [source for API app](#api-app-source).</span></span>

<span data-ttu-id="fd6d1-151">Vi använder den [ `CreateDocumentChangeFeedQuery` ](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery.aspx) klass från Azure Cosmos DB DocumentDB .NET API.</span><span class="sxs-lookup"><span data-stu-id="fd6d1-151">We are using the [`CreateDocumentChangeFeedQuery`](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery.aspx) class from the Azure Cosmos DB DocumentDB .NET API.</span></span> <span data-ttu-id="fd6d1-152">Mer information finns i [ändra feed artikel](change-feed.md).</span><span class="sxs-lookup"><span data-stu-id="fd6d1-152">For more information, see the [change feed article](change-feed.md).</span></span> 

##### <a name="getnewormodifiedfhirdocuments-operation"></a><span data-ttu-id="fd6d1-153">GetNewOrModifiedFhirDocuments åtgärden</span><span class="sxs-lookup"><span data-stu-id="fd6d1-153">GetNewOrModifiedFhirDocuments operation</span></span>

<span data-ttu-id="fd6d1-154">**Indata**</span><span class="sxs-lookup"><span data-stu-id="fd6d1-154">**Inputs**</span></span>
- <span data-ttu-id="fd6d1-155">DatabaseId</span><span class="sxs-lookup"><span data-stu-id="fd6d1-155">DatabaseId</span></span>
- <span data-ttu-id="fd6d1-156">CollectionId</span><span class="sxs-lookup"><span data-stu-id="fd6d1-156">CollectionId</span></span>
- <span data-ttu-id="fd6d1-157">Resurstyp för HL7 FHIR namn</span><span class="sxs-lookup"><span data-stu-id="fd6d1-157">HL7 FHIR Resource Type name</span></span>
- <span data-ttu-id="fd6d1-158">Boolean: Starta från början</span><span class="sxs-lookup"><span data-stu-id="fd6d1-158">Boolean: Start from Beginning</span></span>
- <span data-ttu-id="fd6d1-159">Int: Antal dokument som returneras</span><span class="sxs-lookup"><span data-stu-id="fd6d1-159">Int: Number of documents returned</span></span>

<span data-ttu-id="fd6d1-160">**Utdata**</span><span class="sxs-lookup"><span data-stu-id="fd6d1-160">**Outputs**</span></span>
- <span data-ttu-id="fd6d1-161">Lyckades: Statuskod: 200, svar: listan över dokument (JSON-matris)</span><span class="sxs-lookup"><span data-stu-id="fd6d1-161">Success: Status Code: 200, Response: List of Documents (JSON Array)</span></span>
- <span data-ttu-id="fd6d1-162">Fel: Statuskod: 404, svar ”: inga dokument hittades för”*resursnamnet '* resurstypen ”</span><span class="sxs-lookup"><span data-stu-id="fd6d1-162">Failure: Status Code: 404, Response: "No Documents found for '*resource name'* Resource Type"</span></span>

<a id="api-app-source"></a>

<span data-ttu-id="fd6d1-163">**Källa för API-appen**</span><span class="sxs-lookup"><span data-stu-id="fd6d1-163">**Source for the API app**</span></span>

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
            ///     Gets the new or modified FHIR documents from Last Run Date 
            ///     or create date of the collection
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

### <a name="testing-the-fhirnotificationapi"></a><span data-ttu-id="fd6d1-164">Testa FhirNotificationApi</span><span class="sxs-lookup"><span data-stu-id="fd6d1-164">Testing the FhirNotificationApi</span></span> 

<span data-ttu-id="fd6d1-165">Följande bild visar hur swagger användes till att testa den [FhirNotificationApi](#api-app-source).</span><span class="sxs-lookup"><span data-stu-id="fd6d1-165">The following image demonstrates how swagger was used to to test the [FhirNotificationApi](#api-app-source).</span></span>

![Swagger-filen som används för att testa API-appen](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-testing-app.png)


### <a name="azure-portal-dashboard"></a><span data-ttu-id="fd6d1-167">Azure portalens instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="fd6d1-167">Azure portal dashboard</span></span>

<span data-ttu-id="fd6d1-168">Följande bild visar alla Azure-tjänster för den här lösningen som körs i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fd6d1-168">The following image shows all of the Azure services for this solution running in the Azure portal.</span></span>

![Azure portal som visar alla tjänster som används i den här självstudiekursen HL7 FHIR](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-portal.png)


## <a name="summary"></a><span data-ttu-id="fd6d1-170">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="fd6d1-170">Summary</span></span>

- <span data-ttu-id="fd6d1-171">Du har lärt dig att Azure Cosmos DB har inbyggda relationstypen för meddelanden om nya eller ändras dokument och hur lätt det är att använda.</span><span class="sxs-lookup"><span data-stu-id="fd6d1-171">You have learned that Azure Cosmos DB has native suppport for notifications for new or modifed documents and how easy it is to use.</span></span> 
- <span data-ttu-id="fd6d1-172">Du kan skapa arbetsflöden utan att skriva någon kod genom att utnyttja Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="fd6d1-172">By leveraging Logic Apps, you can create workflows without writing any code.</span></span>
- <span data-ttu-id="fd6d1-173">Med Azure Service Bus-köer för att hantera distribution för HL7 FHIR-dokument.</span><span class="sxs-lookup"><span data-stu-id="fd6d1-173">Using Azure Service Bus Queues to handle the distribution for the HL7 FHIR documents.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fd6d1-174">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fd6d1-174">Next steps</span></span>
<span data-ttu-id="fd6d1-175">Mer information om Azure Cosmos DB finns på [Azure Cosmos DB startsidan](https://azure.microsoft.com/services/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="fd6d1-175">For more information about Azure Cosmos DB, see the [Azure Cosmos DB home page](https://azure.microsoft.com/services/cosmos-db/).</span></span> <span data-ttu-id="fd6d1-176">Mer information om Logic Apps finns i [Logikappar](https://azure.microsoft.com/services/logic-apps/).</span><span class="sxs-lookup"><span data-stu-id="fd6d1-176">For more informaiton about Logic Apps, see [Logic Apps](https://azure.microsoft.com/services/logic-apps/).</span></span>


