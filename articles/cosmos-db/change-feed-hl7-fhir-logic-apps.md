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
# <a name="notifying-patients-of-hl7-fhir-health-care-record-changes-using-logic-apps-and-azure-cosmos-db"></a>Meddela patienter HL7 FHIR hälsovård post ändringar med hjälp av Logic Apps och Azure Cosmos DB

Azure MVP Howard Edidin kontaktades nyligen av en sjukvårdsorganisation som ville ha tooadd nya funktioner tootheir patient portalen. De behövs toosend meddelanden toopatients när deras hälsa posten har uppdaterats och de behövs patienter toobe kan toosubscribe toothese uppdateringar. 

Den här artikeln beskriver hur hello ändra feed meddelande lösning som har skapats för den här sjukvårdsorganisation med Azure Cosmos DB, Logic Apps och Service Bus. 

## <a name="project-requirements"></a>Projektkrav för
- Providers skicka HL7 konsoliderade kliniska dokumentet arkitektur (C-CDA) dokument i XML-format. C-CDA dokument omfattar nästan alla typer av kliniska dokument, inklusive kliniska dokument, till exempel family historik och vaccination poster, samt som administrativa, arbetsflöden och ekonomiska dokument. 
- C-CDA dokument konverteras för[HL7 FHIR resurser](http://hl7.org/fhir/2017Jan/resourcelist.html) i JSON-format.
- Ändrade FHIR resurs dokument skickas via e-post i JSON-format.

## <a name="solution-workflow"></a>Lösning för arbetsflöde 

På en hög nivå hello som krävs för hello följande arbetsflöde: 
1. Konvertera C CDA dokument tooFHIR resurser.
2. Utför återkommande utlösare avsöker för ändrade FHIR resurser. 
2. Anropa en anpassad app, FhirNotificationApi, tooconnect tooAzure Cosmos DB och fråga efter nya eller ändrade dokument.
3. Spara hello svar tootoohello Service Bus-kö.
4. Avsökning för nya meddelanden i hello Service Bus-kö.
5. Skicka e-postaviseringar toopatients.

## <a name="solution-architecture"></a>Lösningsarkitektur
Denna lösning kräver tre Logic Apps toomeet hello ovan krav och fullständig hello lösning arbetsflöde. hello tre logic apps är:
1. **HL7-FHIR-mappning app**: tar emot hello HL7 C-CDA dokument, omvandlar den toohello FHIR resurs och sparar den tooAzure Cosmos DB.
2. **EHR app**: frågar hello Azure Cosmos DB FHIR databasen och sparar hello svar tooa Service Bus-kö. Den här logikapp använder en [API-app](#api-app) tooretrieve nya och ändrade dokument.
3. **Processen meddelandeprogram**: skickar ett e-postmeddelande med hello FHIR resurs dokument i hello brödtext.

![hello tre Logic Apps används i den här HL7 FHIR sjukvården lösningen](./media/change-feed-hl7-fhir-logic-apps/health-care-solution-hl7-fhir.png)



### <a name="azure-services-used-in-hello-solution"></a>Azure-tjänster används i hello lösningar

#### <a name="azure-cosmos-db-documentdb-api"></a>Azure Cosmos DB DocumentDB API
Azure Cosmos-DB är hello lagringsplats för hello FHIR resurser som visas i följande bild hello.

![hello Azure Cosmos DB konto som används i självstudierna HL7 FHIR sjukvården](./media/change-feed-hl7-fhir-logic-apps/account.png)

#### <a name="logic-apps"></a>Logic Apps
Logic Apps hantera hello arbetsflödesprocessen. hello visar följande skärmdumpar hello Logic apps som skapats för den här lösningen. 


1. **HL7-FHIR-mappning app**: får hello HL7 C-CDA dokument och transformera dem tooan FHIR resursen med hjälp av hello Enterprise-Integrationspaket för Logic Apps. hello Enterprise-Integrationspaket hanterar hello mappning från hello C CDA tooFHIR resurser.

    ![Hej Logikapp används tooreceive HL7 FHIR sjukvården poster](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-json-transform.png)


2. **EHR app**: fråga hello Azure Cosmos DB FHIR databasen och spara hello svar tooa Service Bus-kö. hello-koden för hello GetNewOrModifiedFHIRDocuments app är nedan.

    ![Hej Logikapp används tooquery Azure Cosmos DB](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-api-app.png)

3. **Processen meddelandeprogram**: skicka ett e-postmeddelande med hello FHIR resurs dokument i hello brödtext.

    ![Hej Logikappen som skickar patient e-postmeddelande med hello HL7 FHIR resurs i hello brödtext](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-send-email.png)

#### <a name="service-bus"></a>Service Bus
Följande bild visar hello patienter kön hello. hello taggen egenskapens värde används för hello e-postmeddelandets ämne.

![hello används i den här självstudiekursen HL7 FHIR Service Bus-kö](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-service-bus-queue.png)

<a id="api-app"></a>

#### <a name="api-app"></a>API-app
En API-app ansluter tooAzure Cosmos DB och frågor om nya eller ändrade FHIR dokument uppdelat efter resurstyp. Det här programmet har en domänkontrollant **FhirNotificationApi** med en åtgärd **GetNewOrModifiedFhirDocuments**, se [källa för API-app](#api-app-source).

Vi använder hello [ `CreateDocumentChangeFeedQuery` ](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery.aspx) klass från hello Cosmos-DocumentDB på Azure DB .NET API: et. Mer information finns i hello [ändra feed artikel](change-feed.md). 

##### <a name="getnewormodifiedfhirdocuments-operation"></a>GetNewOrModifiedFhirDocuments åtgärden

**Indata**
- DatabaseId
- CollectionId
- Resurstyp för HL7 FHIR namn
- Boolean: Starta från början
- Int: Antal dokument som returneras

**Utdata**
- Lyckades: Statuskod: 200, svar: listan över dokument (JSON-matris)
- Fel: Statuskod: 404, svar ”: inga dokument hittades för”*resursnamnet '* resurstypen ”

<a id="api-app-source"></a>

**Källa för hello API-app**

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

### <a name="testing-hello-fhirnotificationapi"></a>Testa hello FhirNotificationApi 

hello följande bild visar hur swagger har använt tootootest hello [FhirNotificationApi](#api-app-source).

![Hej Swagger-filen används tootest hello API-app](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-testing-app.png)


### <a name="azure-portal-dashboard"></a>Azure portalens instrumentpanel

hello följande bild visar alla hello Azure-tjänster för den här lösningen som körs i hello Azure-portalen.

![hello Azure portal som visar alla hello-tjänster som används i den här självstudiekursen HL7 FHIR](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-portal.png)


## <a name="summary"></a>Sammanfattning

- Du har lärt dig att Azure Cosmos DB har inbyggda relationstypen för meddelanden om nya eller ändras dokument och hur lätt det är toouse. 
- Du kan skapa arbetsflöden utan att skriva någon kod genom att utnyttja Logic Apps.
- Med hjälp av Azure Service Bus-köer toohandle hello distribution för hello HL7 FHIR dokument.

## <a name="next-steps"></a>Nästa steg
Mer information om Azure Cosmos DB finns hello [Azure Cosmos DB startsidan](https://azure.microsoft.com/services/cosmos-db/). Mer information om Logic Apps finns i [Logikappar](https://azure.microsoft.com/services/logic-apps/).


