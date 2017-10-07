---
title: aaaLog Analytics HTTP Data Collector API | Microsoft Docs
description: "Du kan använda hello Log Analytics HTTP Data Collector API tooadd POST JSON toohello logganalys lagringsplats för data från alla klienter som kan anropa hello REST API. Den här artikeln beskriver hur toouse hello API och exempel på hur har toopublish data med hjälp av olika programmeringsspråk."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: a831fd90-3f55-423b-8b20-ccbaaac2ca75
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: bwren
ms.openlocfilehash: c2921082831c49da764d946ac9c4fab975a38185
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="send-data-toolog-analytics-with-hello-http-data-collector-api"></a><span data-ttu-id="d03d7-104">Skicka data tooLog Analytics med hello HTTP Data Collector API: et</span><span class="sxs-lookup"><span data-stu-id="d03d7-104">Send data tooLog Analytics with hello HTTP Data Collector API</span></span>
<span data-ttu-id="d03d7-105">Den här artikeln visar hur toouse hello HTTP Data Collector API: et toosend data tooLog Analytics från en REST API-klient.</span><span class="sxs-lookup"><span data-stu-id="d03d7-105">This article shows you how toouse hello HTTP Data Collector API toosend data tooLog Analytics from a REST API client.</span></span>  <span data-ttu-id="d03d7-106">Det beskrivs hur tooformat data som samlas in av skript eller program, inkludera den i en begäran och har den begäran som auktoriserad genom logganalys.</span><span class="sxs-lookup"><span data-stu-id="d03d7-106">It describes how tooformat data collected by your script or application, include it in a request, and have that request authorized by Log Analytics.</span></span>  <span data-ttu-id="d03d7-107">Exempel för för PowerShell, C# och Python.</span><span class="sxs-lookup"><span data-stu-id="d03d7-107">Examples are provided for PowerShell, C#, and Python.</span></span>

## <a name="concepts"></a><span data-ttu-id="d03d7-108">Koncept</span><span class="sxs-lookup"><span data-stu-id="d03d7-108">Concepts</span></span>
<span data-ttu-id="d03d7-109">Du kan använda hello HTTP Data Collector API: et toosend data tooLog Analytics från klienter som kan anropa REST-API.</span><span class="sxs-lookup"><span data-stu-id="d03d7-109">You can use hello HTTP Data Collector API toosend data tooLog Analytics from any client that can call a REST API.</span></span>  <span data-ttu-id="d03d7-110">Detta kan bero på en runbook i Azure Automation som samlar in hantering av data från Azure eller ett annat moln, eller så kan vara ett alternativ-system som använder logganalys tooconsolidate och analysera data.</span><span class="sxs-lookup"><span data-stu-id="d03d7-110">This might be a runbook in Azure Automation that collects management data from Azure or another cloud, or it might be an alternate management system that uses Log Analytics tooconsolidate and analyze data.</span></span>

<span data-ttu-id="d03d7-111">Alla data i hello logganalys databasen lagras som en post med en viss posttyp.</span><span class="sxs-lookup"><span data-stu-id="d03d7-111">All data in hello Log Analytics repository is stored as a record with a particular record type.</span></span>  <span data-ttu-id="d03d7-112">Du kan formatera dina data toosend toohello HTTP Data Collector API som flera poster i JSON.</span><span class="sxs-lookup"><span data-stu-id="d03d7-112">You format your data toosend toohello HTTP Data Collector API as multiple records in JSON.</span></span>  <span data-ttu-id="d03d7-113">När du skickar hello data, skapas en enskild post i hello databas för varje post i hello nyttolasten i begäran.</span><span class="sxs-lookup"><span data-stu-id="d03d7-113">When you submit hello data, an individual record is created in hello repository for each record in hello request payload.</span></span>


![Översikt över HTTP-datainsamlare](media/log-analytics-data-collector-api/overview.png)



## <a name="create-a-request"></a><span data-ttu-id="d03d7-115">Skapa en förfrågan</span><span class="sxs-lookup"><span data-stu-id="d03d7-115">Create a request</span></span>
<span data-ttu-id="d03d7-116">toouse hello HTTP datainsamlaren API, skapa en POST-begäran som innehåller hello data toosend i JavaScript Object Notation (JSON).</span><span class="sxs-lookup"><span data-stu-id="d03d7-116">toouse hello HTTP Data Collector API, you create a POST request that includes hello data toosend in JavaScript Object Notation (JSON).</span></span>  <span data-ttu-id="d03d7-117">Hej följande tre tabeller listan hello attribut som krävs för varje begäran.</span><span class="sxs-lookup"><span data-stu-id="d03d7-117">hello next three tables list hello attributes that are required for each request.</span></span> <span data-ttu-id="d03d7-118">Vi beskriver varje attribut i detalj senare i hello artikeln.</span><span class="sxs-lookup"><span data-stu-id="d03d7-118">We describe each attribute in more detail later in hello article.</span></span>

### <a name="request-uri"></a><span data-ttu-id="d03d7-119">URI-begäran</span><span class="sxs-lookup"><span data-stu-id="d03d7-119">Request URI</span></span>
| <span data-ttu-id="d03d7-120">Attribut</span><span class="sxs-lookup"><span data-stu-id="d03d7-120">Attribute</span></span> | <span data-ttu-id="d03d7-121">Egenskap</span><span class="sxs-lookup"><span data-stu-id="d03d7-121">Property</span></span> |
|:--- |:--- |
| <span data-ttu-id="d03d7-122">Metod</span><span class="sxs-lookup"><span data-stu-id="d03d7-122">Method</span></span> |<span data-ttu-id="d03d7-123">POST</span><span class="sxs-lookup"><span data-stu-id="d03d7-123">POST</span></span> |
| <span data-ttu-id="d03d7-124">URI: N</span><span class="sxs-lookup"><span data-stu-id="d03d7-124">URI</span></span> |<span data-ttu-id="d03d7-125">https://\<CustomerId\>.ods.opinsights.azure.com/api/logs?api-version=2016-04-01</span><span class="sxs-lookup"><span data-stu-id="d03d7-125">https://\<CustomerId\>.ods.opinsights.azure.com/api/logs?api-version=2016-04-01</span></span> |
| <span data-ttu-id="d03d7-126">Innehållstyp</span><span class="sxs-lookup"><span data-stu-id="d03d7-126">Content type</span></span> |<span data-ttu-id="d03d7-127">application/json</span><span class="sxs-lookup"><span data-stu-id="d03d7-127">application/json</span></span> |

### <a name="request-uri-parameters"></a><span data-ttu-id="d03d7-128">URI-parametrar för begäran</span><span class="sxs-lookup"><span data-stu-id="d03d7-128">Request URI parameters</span></span>
| <span data-ttu-id="d03d7-129">Parameter</span><span class="sxs-lookup"><span data-stu-id="d03d7-129">Parameter</span></span> | <span data-ttu-id="d03d7-130">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="d03d7-130">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="d03d7-131">CustomerID</span><span class="sxs-lookup"><span data-stu-id="d03d7-131">CustomerID</span></span> |<span data-ttu-id="d03d7-132">hello Unik identifierare för hello Microsoft Operations Management Suite-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="d03d7-132">hello unique identifier for hello Microsoft Operations Management Suite workspace.</span></span> |
| <span data-ttu-id="d03d7-133">Resurs</span><span class="sxs-lookup"><span data-stu-id="d03d7-133">Resource</span></span> |<span data-ttu-id="d03d7-134">hello API resursnamn: / api/logs.</span><span class="sxs-lookup"><span data-stu-id="d03d7-134">hello API resource name: /api/logs.</span></span> |
| <span data-ttu-id="d03d7-135">API-Version</span><span class="sxs-lookup"><span data-stu-id="d03d7-135">API Version</span></span> |<span data-ttu-id="d03d7-136">hello version av hello API toouse med den här begäran.</span><span class="sxs-lookup"><span data-stu-id="d03d7-136">hello version of hello API toouse with this request.</span></span> <span data-ttu-id="d03d7-137">Det är för närvarande 2016-04-01.</span><span class="sxs-lookup"><span data-stu-id="d03d7-137">Currently, it's 2016-04-01.</span></span> |

### <a name="request-headers"></a><span data-ttu-id="d03d7-138">Huvuden för begäran</span><span class="sxs-lookup"><span data-stu-id="d03d7-138">Request headers</span></span>
| <span data-ttu-id="d03d7-139">Huvudet</span><span class="sxs-lookup"><span data-stu-id="d03d7-139">Header</span></span> | <span data-ttu-id="d03d7-140">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="d03d7-140">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="d03d7-141">Auktorisering</span><span class="sxs-lookup"><span data-stu-id="d03d7-141">Authorization</span></span> |<span data-ttu-id="d03d7-142">hello auktorisering signatur.</span><span class="sxs-lookup"><span data-stu-id="d03d7-142">hello authorization signature.</span></span> <span data-ttu-id="d03d7-143">Senare i hello artikeln kan du läsa om hur toocreate en HMAC SHA256-huvud.</span><span class="sxs-lookup"><span data-stu-id="d03d7-143">Later in hello article, you can read about how toocreate an HMAC-SHA256 header.</span></span> |
| <span data-ttu-id="d03d7-144">Typ</span><span class="sxs-lookup"><span data-stu-id="d03d7-144">Log-Type</span></span> |<span data-ttu-id="d03d7-145">Ange hello posttyp hello data som skickas.</span><span class="sxs-lookup"><span data-stu-id="d03d7-145">Specify hello record type of hello data that is being submitted.</span></span> <span data-ttu-id="d03d7-146">Typ av hello stöder för närvarande endast alfanumeriska tecken.</span><span class="sxs-lookup"><span data-stu-id="d03d7-146">Currently, hello log type supports only alpha characters.</span></span> <span data-ttu-id="d03d7-147">Stöder inte siffror eller specialtecken.</span><span class="sxs-lookup"><span data-stu-id="d03d7-147">It does not support numerics or special characters.</span></span> |
| <span data-ttu-id="d03d7-148">x-ms-date</span><span class="sxs-lookup"><span data-stu-id="d03d7-148">x-ms-date</span></span> |<span data-ttu-id="d03d7-149">hello datum hello begäran bearbetades i RFC 1123 format.</span><span class="sxs-lookup"><span data-stu-id="d03d7-149">hello date that hello request was processed, in RFC 1123 format.</span></span> |
| <span data-ttu-id="d03d7-150">tid genererade fält</span><span class="sxs-lookup"><span data-stu-id="d03d7-150">time-generated-field</span></span> |<span data-ttu-id="d03d7-151">hello namnet på ett fält i hello data som innehåller hello tidsstämpel för hello dataobjektet.</span><span class="sxs-lookup"><span data-stu-id="d03d7-151">hello name of a field in hello data that contains hello timestamp of hello data item.</span></span> <span data-ttu-id="d03d7-152">Om du anger ett fält och dess innehåll används för **TimeGenerated**.</span><span class="sxs-lookup"><span data-stu-id="d03d7-152">If you specify a field then its contents are used for **TimeGenerated**.</span></span> <span data-ttu-id="d03d7-153">Om det här fältet har inte angetts, hello standard för **TimeGenerated** är hello tid att hello meddelandet inhämtas.</span><span class="sxs-lookup"><span data-stu-id="d03d7-153">If this field isn’t specified, hello default for **TimeGenerated** is hello time that hello message is ingested.</span></span> <span data-ttu-id="d03d7-154">hello innehållet i fältet för hello-meddelande bör följa hello ISO 8601-formatet ÅÅÅÅ-MM-ddTHH.</span><span class="sxs-lookup"><span data-stu-id="d03d7-154">hello contents of hello message field should follow hello ISO 8601 format YYYY-MM-DDThh:mm:ssZ.</span></span> |

## <a name="authorization"></a><span data-ttu-id="d03d7-155">Auktorisering</span><span class="sxs-lookup"><span data-stu-id="d03d7-155">Authorization</span></span>
<span data-ttu-id="d03d7-156">Alla begäran toohello Log Analytics HTTP Data Collector API måste innehålla ett authorization-huvud.</span><span class="sxs-lookup"><span data-stu-id="d03d7-156">Any request toohello Log Analytics HTTP Data Collector API must include an authorization header.</span></span> <span data-ttu-id="d03d7-157">tooauthenticate en begäran, måste du registrera hello begäran med hello primära eller hello sekundärnyckeln för hello-arbetsyta som gör hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="d03d7-157">tooauthenticate a request, you must sign hello request with either hello primary or hello secondary key for hello workspace that is making hello request.</span></span> <span data-ttu-id="d03d7-158">Sen skicka signaturen i hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="d03d7-158">Then, pass that signature as part of hello request.</span></span>   

<span data-ttu-id="d03d7-159">Här är hello format för hello authorization-huvud:</span><span class="sxs-lookup"><span data-stu-id="d03d7-159">Here's hello format for hello authorization header:</span></span>

```
Authorization: SharedKey <WorkspaceID>:<Signature>
```

<span data-ttu-id="d03d7-160">*WorkspaceID* är hello Unik identifierare för hello Operations Management Suite-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="d03d7-160">*WorkspaceID* is hello unique identifier for hello Operations Management Suite workspace.</span></span> <span data-ttu-id="d03d7-161">*Signaturen* är en [hashbaserad meddelandeautentiseringskod (HMAC)](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) som skapas från hello begäran och sedan beräknas med hjälp av hello [SHA256-algoritmen](https://msdn.microsoft.com/library/system.security.cryptography.sha256.aspx).</span><span class="sxs-lookup"><span data-stu-id="d03d7-161">*Signature* is a [Hash-based Message Authentication Code (HMAC)](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) that is constructed from hello request and then computed by using hello [SHA256 algorithm](https://msdn.microsoft.com/library/system.security.cryptography.sha256.aspx).</span></span> <span data-ttu-id="d03d7-162">Sedan, koda den med hjälp av Base64-kodning.</span><span class="sxs-lookup"><span data-stu-id="d03d7-162">Then, you encode it by using Base64 encoding.</span></span>

<span data-ttu-id="d03d7-163">Använd det här formatet tooencode hello **SharedKey** signatur sträng:</span><span class="sxs-lookup"><span data-stu-id="d03d7-163">Use this format tooencode hello **SharedKey** signature string:</span></span>

```
StringToSign = VERB + "\n" +
                  Content-Length + "\n" +
               Content-Type + "\n" +
                  x-ms-date + "\n" +
                  "/api/logs";
```

<span data-ttu-id="d03d7-164">Här är ett exempel på en signatur-sträng:</span><span class="sxs-lookup"><span data-stu-id="d03d7-164">Here's an example of a signature string:</span></span>

```
POST\n1024\napplication/json\nx-ms-date:Mon, 04 Apr 2016 08:00:00 GMT\n/api/logs
```

<span data-ttu-id="d03d7-165">När du har hello signatur sträng, koda med hjälp av hello HMAC SHA256-algoritmen på hello UTF-8-kodad sträng och koda hello resultat som Base64.</span><span class="sxs-lookup"><span data-stu-id="d03d7-165">When you have hello signature string, encode it by using hello HMAC-SHA256 algorithm on hello UTF-8-encoded string, and then encode hello result as Base64.</span></span> <span data-ttu-id="d03d7-166">Använd följande format:</span><span class="sxs-lookup"><span data-stu-id="d03d7-166">Use this format:</span></span>

```
Signature=Base64(HMAC-SHA256(UTF8(StringToSign)))
```

<span data-ttu-id="d03d7-167">hello prover i nästa avsnitt av hello ha exempel kod toohelp du skapar ett authorization-huvud.</span><span class="sxs-lookup"><span data-stu-id="d03d7-167">hello samples in hello next sections have sample code toohelp you create an authorization header.</span></span>

## <a name="request-body"></a><span data-ttu-id="d03d7-168">Begärandetexten</span><span class="sxs-lookup"><span data-stu-id="d03d7-168">Request body</span></span>
<span data-ttu-id="d03d7-169">hello innehållet i hello-meddelande måste vara i JSON.</span><span class="sxs-lookup"><span data-stu-id="d03d7-169">hello body of hello message must be in JSON.</span></span> <span data-ttu-id="d03d7-170">Det måste innehålla en eller flera poster med hello egenskapen namn och värdepar i det här formatet:</span><span class="sxs-lookup"><span data-stu-id="d03d7-170">It must include one or more records with hello property name and value pairs in this format:</span></span>

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

<span data-ttu-id="d03d7-171">Du kan batch flera poster tillsammans i en enskild begäran med hjälp av hello följande format.</span><span class="sxs-lookup"><span data-stu-id="d03d7-171">You can batch multiple records together in a single request by using hello following format.</span></span> <span data-ttu-id="d03d7-172">Alla hello-poster måste vara hello samma posttyp.</span><span class="sxs-lookup"><span data-stu-id="d03d7-172">All hello records must be hello same record type.</span></span>

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
},
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

## <a name="record-type-and-properties"></a><span data-ttu-id="d03d7-173">Posttyp och egenskaper</span><span class="sxs-lookup"><span data-stu-id="d03d7-173">Record type and properties</span></span>
<span data-ttu-id="d03d7-174">Du kan definiera en anpassad posttyp när du skickar data via hello Log Analytics HTTP Data Collector API.</span><span class="sxs-lookup"><span data-stu-id="d03d7-174">You define a custom record type when you submit data through hello Log Analytics HTTP Data Collector API.</span></span> <span data-ttu-id="d03d7-175">Du kan inte för närvarande kan skriva data tooexisting posttyper som har skapats av andra typer av data och lösningar.</span><span class="sxs-lookup"><span data-stu-id="d03d7-175">Currently, you can't write data tooexisting record types that were created by other data types and solutions.</span></span> <span data-ttu-id="d03d7-176">Logganalys läser hello inkommande data och skapar sedan egenskaper som matchar hello datatyperna hello-värden som du anger.</span><span class="sxs-lookup"><span data-stu-id="d03d7-176">Log Analytics reads hello incoming data and then creates properties that match hello data types of hello values that you enter.</span></span>

<span data-ttu-id="d03d7-177">Varje begäran toohello Log Analytics API måste innehålla en **loggtyp** huvud med hello namn för hello posttyp.</span><span class="sxs-lookup"><span data-stu-id="d03d7-177">Each request toohello Log Analytics API must include a **Log-Type** header with hello name for hello record type.</span></span> <span data-ttu-id="d03d7-178">hello suffix **_CL** är automatiskt tillagda toohello namn som du anger toodistinguish den från andra logga svarstyperna som en anpassad logg.</span><span class="sxs-lookup"><span data-stu-id="d03d7-178">hello suffix **_CL** is automatically appended toohello name you enter toodistinguish it from other log types as a custom log.</span></span> <span data-ttu-id="d03d7-179">Om du anger hello namn exempelvis **MyNewRecordType**, logganalys skapas en post med hello typen **MyNewRecordType_CL**.</span><span class="sxs-lookup"><span data-stu-id="d03d7-179">For example, if you enter hello name **MyNewRecordType**, Log Analytics creates a record with hello type **MyNewRecordType_CL**.</span></span> <span data-ttu-id="d03d7-180">Detta säkerställer att det inte finns några konflikter mellan användarskapade typnamn och de levereras i aktuellt eller framtida Microsoft solutions.</span><span class="sxs-lookup"><span data-stu-id="d03d7-180">This helps ensure that there are no conflicts between user-created type names and those shipped in current or future Microsoft solutions.</span></span>

<span data-ttu-id="d03d7-181">datatypen för tooidentify en egenskap, logganalys lägger till ett suffix toohello egenskapsnamn.</span><span class="sxs-lookup"><span data-stu-id="d03d7-181">tooidentify a property's data type, Log Analytics adds a suffix toohello property name.</span></span> <span data-ttu-id="d03d7-182">Om en egenskap innehåller ett null-värde, ingår inte hello-egenskapen i posten.</span><span class="sxs-lookup"><span data-stu-id="d03d7-182">If a property contains a null value, hello property is not included in that record.</span></span> <span data-ttu-id="d03d7-183">Den här tabellen innehåller hello egenskapsdatatyp och motsvarande suffix:</span><span class="sxs-lookup"><span data-stu-id="d03d7-183">This table lists hello property data type and corresponding suffix:</span></span>

| <span data-ttu-id="d03d7-184">Datatypen för egenskapen</span><span class="sxs-lookup"><span data-stu-id="d03d7-184">Property data type</span></span> | <span data-ttu-id="d03d7-185">Suffix</span><span class="sxs-lookup"><span data-stu-id="d03d7-185">Suffix</span></span> |
|:--- |:--- |
| <span data-ttu-id="d03d7-186">Sträng</span><span class="sxs-lookup"><span data-stu-id="d03d7-186">String</span></span> |<span data-ttu-id="d03d7-187">_S</span><span class="sxs-lookup"><span data-stu-id="d03d7-187">_s</span></span> |
| <span data-ttu-id="d03d7-188">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="d03d7-188">Boolean</span></span> |<span data-ttu-id="d03d7-189">_b</span><span class="sxs-lookup"><span data-stu-id="d03d7-189">_b</span></span> |
| <span data-ttu-id="d03d7-190">dubbla</span><span class="sxs-lookup"><span data-stu-id="d03d7-190">Double</span></span> |<span data-ttu-id="d03d7-191">_D</span><span class="sxs-lookup"><span data-stu-id="d03d7-191">_d</span></span> |
| <span data-ttu-id="d03d7-192">Datum/tid</span><span class="sxs-lookup"><span data-stu-id="d03d7-192">Date/time</span></span> |<span data-ttu-id="d03d7-193">_Tätt</span><span class="sxs-lookup"><span data-stu-id="d03d7-193">_t</span></span> |
| <span data-ttu-id="d03d7-194">GUID</span><span class="sxs-lookup"><span data-stu-id="d03d7-194">GUID</span></span> |<span data-ttu-id="d03d7-195">_g</span><span class="sxs-lookup"><span data-stu-id="d03d7-195">_g</span></span> |

<span data-ttu-id="d03d7-196">hello-datatyp som Log Analytics använder för varje egenskap beror på om hello posttypen för hello nya posten redan finns.</span><span class="sxs-lookup"><span data-stu-id="d03d7-196">hello data type that Log Analytics uses for each property depends on whether hello record type for hello new record already exists.</span></span>

* <span data-ttu-id="d03d7-197">Om hello posttypen inte finns, skapar en ny logganalys.</span><span class="sxs-lookup"><span data-stu-id="d03d7-197">If hello record type does not exist, Log Analytics creates a new one.</span></span> <span data-ttu-id="d03d7-198">Log Analytics använder hello JSON typen härledning toodetermine hello datatyp för varje egenskap för hello nya posten.</span><span class="sxs-lookup"><span data-stu-id="d03d7-198">Log Analytics uses hello JSON type inference toodetermine hello data type for each property for hello new record.</span></span>
* <span data-ttu-id="d03d7-199">Om hello posttyp finns försöker logganalys toocreate en ny post baserat på befintliga egenskaper.</span><span class="sxs-lookup"><span data-stu-id="d03d7-199">If hello record type does exist, Log Analytics attempts toocreate a new record based on existing properties.</span></span> <span data-ttu-id="d03d7-200">Om hello datatypen för en egenskap i hello nya posten inte matchar och inte går att konvertera toohello befintlig typ eller om hello post innehåller en egenskap som inte finns, Log Analytics skapar en ny egenskap har som relevanta hello-suffix.</span><span class="sxs-lookup"><span data-stu-id="d03d7-200">If hello data type for a property in hello new record doesn’t match and can’t be converted toohello existing type, or if hello record includes a property that doesn’t exist, Log Analytics creates a new property that has hello relevant suffix.</span></span>

<span data-ttu-id="d03d7-201">Skicka posten skulle till exempel skapa en post med tre egenskaper **number_d**, **boolean_b**, och **string_s**:</span><span class="sxs-lookup"><span data-stu-id="d03d7-201">For example, this submission entry would create a record with three properties, **number_d**, **boolean_b**, and **string_s**:</span></span>

![Exempelpost 1](media/log-analytics-data-collector-api/record-01.png)

<span data-ttu-id="d03d7-203">Om du sedan skickat den här nästa post med alla värden som är formaterade som strängar, ändrar inte hello egenskaper.</span><span class="sxs-lookup"><span data-stu-id="d03d7-203">If you then submitted this next entry, with all values formatted as strings, hello properties would not change.</span></span> <span data-ttu-id="d03d7-204">Dessa värden kan vara konverterade tooexisting datatyper:</span><span class="sxs-lookup"><span data-stu-id="d03d7-204">These values can be converted tooexisting data types:</span></span>

![Exempelpost 2](media/log-analytics-data-collector-api/record-02.png)

<span data-ttu-id="d03d7-206">Men om du har gjort den här nästa skickas sedan Log Analytics skapar hello egenskaper för ny **boolean_d** och **string_d**.</span><span class="sxs-lookup"><span data-stu-id="d03d7-206">But, if you then made this next submission, Log Analytics would create hello new properties **boolean_d** and **string_d**.</span></span> <span data-ttu-id="d03d7-207">Dessa värden kan inte konverteras:</span><span class="sxs-lookup"><span data-stu-id="d03d7-207">These values can't be converted:</span></span>

![Exempelpost 3](media/log-analytics-data-collector-api/record-03.png)

<span data-ttu-id="d03d7-209">Om du sedan skickat hello följande post, innan hello posttyp skapades logganalys skulle skapa en post med tre egenskaper **antal_l**, **boolean_s**, och **string_s**.</span><span class="sxs-lookup"><span data-stu-id="d03d7-209">If you then submitted hello following entry, before hello record type was created, Log Analytics would create a record with three properties, **number_s**, **boolean_s**, and **string_s**.</span></span> <span data-ttu-id="d03d7-210">I den här posten formateras hello inledande värden som en sträng:</span><span class="sxs-lookup"><span data-stu-id="d03d7-210">In this entry, each of hello initial values is formatted as a string:</span></span>

![Exempelpost 4](media/log-analytics-data-collector-api/record-04.png)

## <a name="data-limits"></a><span data-ttu-id="d03d7-212">Databegränsningar</span><span class="sxs-lookup"><span data-stu-id="d03d7-212">Data limits</span></span>
<span data-ttu-id="d03d7-213">Det finns vissa begränsningar runt hello-data som skickats toohello API för Log Analytics datainsamling.</span><span class="sxs-lookup"><span data-stu-id="d03d7-213">There are some constraints around hello data posted toohello Log Analytics Data collection API.</span></span>

* <span data-ttu-id="d03d7-214">Högst 30 MB per post tooLog Analytics Data Collector API: et.</span><span class="sxs-lookup"><span data-stu-id="d03d7-214">Maximum of 30 MB per post tooLog Analytics Data Collector API.</span></span> <span data-ttu-id="d03d7-215">Det här är en storleksgräns för en enskild post.</span><span class="sxs-lookup"><span data-stu-id="d03d7-215">This is a size limit for a single post.</span></span> <span data-ttu-id="d03d7-216">Om hello data från en enda post som är större än 30 MB, du ska dela hello data in toosmaller storlek blocken och skicka dem samtidigt.</span><span class="sxs-lookup"><span data-stu-id="d03d7-216">If hello data from a single post that exceeds 30 MB, you should split hello data up toosmaller sized chunks and send them concurrently.</span></span>
* <span data-ttu-id="d03d7-217">Högst 32 KB-gränsen för fältvärden.</span><span class="sxs-lookup"><span data-stu-id="d03d7-217">Maximum of 32 KB limit for field values.</span></span> <span data-ttu-id="d03d7-218">Om hello fältvärdet är större än 32 KB trunkeras hello data.</span><span class="sxs-lookup"><span data-stu-id="d03d7-218">If hello field value is greater than 32 KB, hello data will be truncated.</span></span>
* <span data-ttu-id="d03d7-219">Rekommenderade maximala antalet fält för en viss typ är 50.</span><span class="sxs-lookup"><span data-stu-id="d03d7-219">Recommended maximum number of fields for a given type is 50.</span></span> <span data-ttu-id="d03d7-220">Det här är en praktisk gräns från en användbarhet och Sök upplevelse perspektiv.</span><span class="sxs-lookup"><span data-stu-id="d03d7-220">This is a practical limit from a usability and search experience perspective.</span></span>  

## <a name="return-codes"></a><span data-ttu-id="d03d7-221">Returkoder</span><span class="sxs-lookup"><span data-stu-id="d03d7-221">Return codes</span></span>
<span data-ttu-id="d03d7-222">hello HTTP-statuskod 200 innebär att hello-begäran har tagits emot för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="d03d7-222">hello HTTP status code 200 means that hello request has been received for processing.</span></span> <span data-ttu-id="d03d7-223">Detta anger att hello-åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="d03d7-223">This indicates that hello operation completed successfully.</span></span>

<span data-ttu-id="d03d7-224">Den här tabellen innehåller hello fullständig uppsättning statuskoder som kan returnera hello-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="d03d7-224">This table lists hello complete set of status codes that hello service might return:</span></span>

| <span data-ttu-id="d03d7-225">Kod</span><span class="sxs-lookup"><span data-stu-id="d03d7-225">Code</span></span> | <span data-ttu-id="d03d7-226">Status</span><span class="sxs-lookup"><span data-stu-id="d03d7-226">Status</span></span> | <span data-ttu-id="d03d7-227">Felkod</span><span class="sxs-lookup"><span data-stu-id="d03d7-227">Error code</span></span> | <span data-ttu-id="d03d7-228">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="d03d7-228">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="d03d7-229">200</span><span class="sxs-lookup"><span data-stu-id="d03d7-229">200</span></span> |<span data-ttu-id="d03d7-230">OKEJ</span><span class="sxs-lookup"><span data-stu-id="d03d7-230">OK</span></span> | |<span data-ttu-id="d03d7-231">hello-begäran har accepterats.</span><span class="sxs-lookup"><span data-stu-id="d03d7-231">hello request was successfully accepted.</span></span> |
| <span data-ttu-id="d03d7-232">400</span><span class="sxs-lookup"><span data-stu-id="d03d7-232">400</span></span> |<span data-ttu-id="d03d7-233">Felaktig begäran</span><span class="sxs-lookup"><span data-stu-id="d03d7-233">Bad request</span></span> |<span data-ttu-id="d03d7-234">InactiveCustomer</span><span class="sxs-lookup"><span data-stu-id="d03d7-234">InactiveCustomer</span></span> |<span data-ttu-id="d03d7-235">hello-arbetsytan har stängts.</span><span class="sxs-lookup"><span data-stu-id="d03d7-235">hello workspace has been closed.</span></span> |
| <span data-ttu-id="d03d7-236">400</span><span class="sxs-lookup"><span data-stu-id="d03d7-236">400</span></span> |<span data-ttu-id="d03d7-237">Felaktig begäran</span><span class="sxs-lookup"><span data-stu-id="d03d7-237">Bad request</span></span> |<span data-ttu-id="d03d7-238">InvalidApiVersion</span><span class="sxs-lookup"><span data-stu-id="d03d7-238">InvalidApiVersion</span></span> |<span data-ttu-id="d03d7-239">hello API-version som du angett kunde inte identifieras av hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d03d7-239">hello API version that you specified was not recognized by hello service.</span></span> |
| <span data-ttu-id="d03d7-240">400</span><span class="sxs-lookup"><span data-stu-id="d03d7-240">400</span></span> |<span data-ttu-id="d03d7-241">Felaktig begäran</span><span class="sxs-lookup"><span data-stu-id="d03d7-241">Bad request</span></span> |<span data-ttu-id="d03d7-242">InvalidCustomerId</span><span class="sxs-lookup"><span data-stu-id="d03d7-242">InvalidCustomerId</span></span> |<span data-ttu-id="d03d7-243">hello arbetsyte-ID som angetts är ogiltigt.</span><span class="sxs-lookup"><span data-stu-id="d03d7-243">hello workspace ID specified is invalid.</span></span> |
| <span data-ttu-id="d03d7-244">400</span><span class="sxs-lookup"><span data-stu-id="d03d7-244">400</span></span> |<span data-ttu-id="d03d7-245">Felaktig begäran</span><span class="sxs-lookup"><span data-stu-id="d03d7-245">Bad request</span></span> |<span data-ttu-id="d03d7-246">InvalidDataFormat</span><span class="sxs-lookup"><span data-stu-id="d03d7-246">InvalidDataFormat</span></span> |<span data-ttu-id="d03d7-247">Ogiltigt JSON har skickats.</span><span class="sxs-lookup"><span data-stu-id="d03d7-247">Invalid JSON was submitted.</span></span> <span data-ttu-id="d03d7-248">hello svarstexten kan innehålla mer information om hur tooresolve hello fel.</span><span class="sxs-lookup"><span data-stu-id="d03d7-248">hello response body might contain more information about how tooresolve hello error.</span></span> |
| <span data-ttu-id="d03d7-249">400</span><span class="sxs-lookup"><span data-stu-id="d03d7-249">400</span></span> |<span data-ttu-id="d03d7-250">Felaktig begäran</span><span class="sxs-lookup"><span data-stu-id="d03d7-250">Bad request</span></span> |<span data-ttu-id="d03d7-251">InvalidLogType</span><span class="sxs-lookup"><span data-stu-id="d03d7-251">InvalidLogType</span></span> |<span data-ttu-id="d03d7-252">typ av hello angav innehåller specialtecken eller siffror.</span><span class="sxs-lookup"><span data-stu-id="d03d7-252">hello log type specified contained special characters or numerics.</span></span> |
| <span data-ttu-id="d03d7-253">400</span><span class="sxs-lookup"><span data-stu-id="d03d7-253">400</span></span> |<span data-ttu-id="d03d7-254">Felaktig begäran</span><span class="sxs-lookup"><span data-stu-id="d03d7-254">Bad request</span></span> |<span data-ttu-id="d03d7-255">MissingApiVersion</span><span class="sxs-lookup"><span data-stu-id="d03d7-255">MissingApiVersion</span></span> |<span data-ttu-id="d03d7-256">hello API-versionen har inte angetts.</span><span class="sxs-lookup"><span data-stu-id="d03d7-256">hello API version wasn’t specified.</span></span> |
| <span data-ttu-id="d03d7-257">400</span><span class="sxs-lookup"><span data-stu-id="d03d7-257">400</span></span> |<span data-ttu-id="d03d7-258">Felaktig begäran</span><span class="sxs-lookup"><span data-stu-id="d03d7-258">Bad request</span></span> |<span data-ttu-id="d03d7-259">MissingContentType</span><span class="sxs-lookup"><span data-stu-id="d03d7-259">MissingContentType</span></span> |<span data-ttu-id="d03d7-260">hello content-type har inte angetts.</span><span class="sxs-lookup"><span data-stu-id="d03d7-260">hello content type wasn’t specified.</span></span> |
| <span data-ttu-id="d03d7-261">400</span><span class="sxs-lookup"><span data-stu-id="d03d7-261">400</span></span> |<span data-ttu-id="d03d7-262">Felaktig begäran</span><span class="sxs-lookup"><span data-stu-id="d03d7-262">Bad request</span></span> |<span data-ttu-id="d03d7-263">MissingLogType</span><span class="sxs-lookup"><span data-stu-id="d03d7-263">MissingLogType</span></span> |<span data-ttu-id="d03d7-264">hello krävs värdetyp har inte angetts.</span><span class="sxs-lookup"><span data-stu-id="d03d7-264">hello required value log type wasn’t specified.</span></span> |
| <span data-ttu-id="d03d7-265">400</span><span class="sxs-lookup"><span data-stu-id="d03d7-265">400</span></span> |<span data-ttu-id="d03d7-266">Felaktig begäran</span><span class="sxs-lookup"><span data-stu-id="d03d7-266">Bad request</span></span> |<span data-ttu-id="d03d7-267">UnsupportedContentType</span><span class="sxs-lookup"><span data-stu-id="d03d7-267">UnsupportedContentType</span></span> |<span data-ttu-id="d03d7-268">hello content-type har inte angetts för**application/json**.</span><span class="sxs-lookup"><span data-stu-id="d03d7-268">hello content type was not set too**application/json**.</span></span> |
| <span data-ttu-id="d03d7-269">403</span><span class="sxs-lookup"><span data-stu-id="d03d7-269">403</span></span> |<span data-ttu-id="d03d7-270">Tillåts inte</span><span class="sxs-lookup"><span data-stu-id="d03d7-270">Forbidden</span></span> |<span data-ttu-id="d03d7-271">InvalidAuthorization</span><span class="sxs-lookup"><span data-stu-id="d03d7-271">InvalidAuthorization</span></span> |<span data-ttu-id="d03d7-272">Det gick inte att tooauthenticate hello begäran hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d03d7-272">hello service failed tooauthenticate hello request.</span></span> <span data-ttu-id="d03d7-273">Kontrollera att hello arbetsytenyckel ID och anslutningen är giltiga.</span><span class="sxs-lookup"><span data-stu-id="d03d7-273">Verify that hello workspace ID and connection key are valid.</span></span> |
| <span data-ttu-id="d03d7-274">404</span><span class="sxs-lookup"><span data-stu-id="d03d7-274">404</span></span> |<span data-ttu-id="d03d7-275">Det gick inte att hitta</span><span class="sxs-lookup"><span data-stu-id="d03d7-275">Not Found</span></span> | | <span data-ttu-id="d03d7-276">Antingen hello URL: en är felaktig eller hello-begäran är för stor.</span><span class="sxs-lookup"><span data-stu-id="d03d7-276">Either hello URL provided is incorrect, or hello request is too large.</span></span> |
| <span data-ttu-id="d03d7-277">429</span><span class="sxs-lookup"><span data-stu-id="d03d7-277">429</span></span> |<span data-ttu-id="d03d7-278">För många begäranden</span><span class="sxs-lookup"><span data-stu-id="d03d7-278">Too Many Requests</span></span> | | <span data-ttu-id="d03d7-279">hello-tjänsten har en stor mängd data från ditt konto.</span><span class="sxs-lookup"><span data-stu-id="d03d7-279">hello service is experiencing a high volume of data from your account.</span></span> <span data-ttu-id="d03d7-280">Försök hello begäran senare.</span><span class="sxs-lookup"><span data-stu-id="d03d7-280">Please retry hello request later.</span></span> |
| <span data-ttu-id="d03d7-281">500</span><span class="sxs-lookup"><span data-stu-id="d03d7-281">500</span></span> |<span data-ttu-id="d03d7-282">Internt serverfel</span><span class="sxs-lookup"><span data-stu-id="d03d7-282">Internal Server Error</span></span> |<span data-ttu-id="d03d7-283">UnspecifiedError</span><span class="sxs-lookup"><span data-stu-id="d03d7-283">UnspecifiedError</span></span> |<span data-ttu-id="d03d7-284">hello tjänsten påträffade ett internt fel.</span><span class="sxs-lookup"><span data-stu-id="d03d7-284">hello service encountered an internal error.</span></span> <span data-ttu-id="d03d7-285">Försök hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="d03d7-285">Please retry hello request.</span></span> |
| <span data-ttu-id="d03d7-286">503</span><span class="sxs-lookup"><span data-stu-id="d03d7-286">503</span></span> |<span data-ttu-id="d03d7-287">Tjänsten är inte tillgänglig</span><span class="sxs-lookup"><span data-stu-id="d03d7-287">Service Unavailable</span></span> |<span data-ttu-id="d03d7-288">ServiceUnavailable</span><span class="sxs-lookup"><span data-stu-id="d03d7-288">ServiceUnavailable</span></span> |<span data-ttu-id="d03d7-289">hello-tjänsten är för tillfället otillgänglig tooreceive begäranden.</span><span class="sxs-lookup"><span data-stu-id="d03d7-289">hello service currently is unavailable tooreceive requests.</span></span> <span data-ttu-id="d03d7-290">Försök att utföra din begäran.</span><span class="sxs-lookup"><span data-stu-id="d03d7-290">Please retry your request.</span></span> |

## <a name="query-data"></a><span data-ttu-id="d03d7-291">Frågedata</span><span class="sxs-lookup"><span data-stu-id="d03d7-291">Query data</span></span>
<span data-ttu-id="d03d7-292">tooquery data som skickats av hello Log Analytics HTTP Data Collector API, söka efter poster med **typen** som är lika toohello **LogType** värde som du angav läggas till med **_CL**.</span><span class="sxs-lookup"><span data-stu-id="d03d7-292">tooquery data submitted by hello Log Analytics HTTP Data Collector API, search for records with **Type** that is equal toohello **LogType** value that you specified, appended with **_CL**.</span></span> <span data-ttu-id="d03d7-293">Om du använde exempelvis **MyCustomLog**, och du vill returnera alla poster med **typ = MyCustomLog_CL**.</span><span class="sxs-lookup"><span data-stu-id="d03d7-293">For example, if you used **MyCustomLog**, then you'd return all records with **Type=MyCustomLog_CL**.</span></span>

>[!NOTE]
> <span data-ttu-id="d03d7-294">Om ditt arbetsområde har uppgraderade toohello [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), hello ovan frågan skulle ändra toohello följande.</span><span class="sxs-lookup"><span data-stu-id="d03d7-294">If your workspace has been upgraded toohello [new Log Analytics query language](log-analytics-log-search-upgrade.md), then hello above query would change toohello following.</span></span>

> `MyCustomLog_CL`

## <a name="sample-requests"></a><span data-ttu-id="d03d7-295">Exempel begäranden</span><span class="sxs-lookup"><span data-stu-id="d03d7-295">Sample requests</span></span>
<span data-ttu-id="d03d7-296">I nästa avsnitt hello hittar du exempel på hur toosubmit data toohello Log Analytics HTTP Data Collector API med hjälp av olika programmeringsspråk.</span><span class="sxs-lookup"><span data-stu-id="d03d7-296">In hello next sections, you'll find samples of how toosubmit data toohello Log Analytics HTTP Data Collector API by using different programming languages.</span></span>

<span data-ttu-id="d03d7-297">För varje prov gör dessa tooset hello variabler för hello authorization-huvud:</span><span class="sxs-lookup"><span data-stu-id="d03d7-297">For each sample, do these steps tooset hello variables for hello authorization header:</span></span>

1. <span data-ttu-id="d03d7-298">Välj hello i hello Operations Management Suite-portalen **inställningar** panelen och välj sedan hello **anslutna källor** fliken.</span><span class="sxs-lookup"><span data-stu-id="d03d7-298">In hello Operations Management Suite portal, select hello **Settings** tile, and then select hello **Connected Sources** tab.</span></span>
2. <span data-ttu-id="d03d7-299">toohello höger i **arbetsyte-ID**hello kopiera-ikonen och välj sedan klistra in hello-ID som hello värde för hello **kund-ID** variabeln.</span><span class="sxs-lookup"><span data-stu-id="d03d7-299">toohello right of **Workspace ID**, select hello copy icon, and then paste hello ID as hello value of hello **Customer ID** variable.</span></span>
3. <span data-ttu-id="d03d7-300">toohello höger i **primärnyckel**hello kopiera-ikonen och välj sedan klistra in hello-ID som hello värde för hello **delad nyckel** variabeln.</span><span class="sxs-lookup"><span data-stu-id="d03d7-300">toohello right of **Primary Key**, select hello copy icon, and then paste hello ID as hello value of hello **Shared Key** variable.</span></span>

<span data-ttu-id="d03d7-301">Du kan också ändra hello variabler för hello loggtyp och JSON-data.</span><span class="sxs-lookup"><span data-stu-id="d03d7-301">Alternatively, you can change hello variables for hello log type and JSON data.</span></span>

### <a name="powershell-sample"></a><span data-ttu-id="d03d7-302">PowerShell-exempel</span><span class="sxs-lookup"><span data-stu-id="d03d7-302">PowerShell sample</span></span>
```
# Replace with your Workspace ID
$CustomerId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"  

# Replace with your Primary Key
$SharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# Specify hello name of hello record type that you'll be creating
$LogType = "MyRecordType"

# Specify a field with hello created time for hello records
$TimeStampField = "DateValue"


# Create two records with hello same set of properties toocreate
$json = @"
[{  "StringValue": "MyString1",
    "NumberValue": 42,
    "BooleanValue": true,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "9909ED01-A74C-4874-8ABF-D2678E3AE23D"
},
{   "StringValue": "MyString2",
    "NumberValue": 43,
    "BooleanValue": false,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "8809ED01-A74C-4874-8ABF-D2678E3AE23D"
}]
"@

# Create hello function toocreate hello authorization signature
Function Build-Signature ($customerId, $sharedKey, $date, $contentLength, $method, $contentType, $resource)
{
    $xHeaders = "x-ms-date:" + $date
    $stringToHash = $method + "`n" + $contentLength + "`n" + $contentType + "`n" + $xHeaders + "`n" + $resource

    $bytesToHash = [Text.Encoding]::UTF8.GetBytes($stringToHash)
    $keyBytes = [Convert]::FromBase64String($sharedKey)

    $sha256 = New-Object System.Security.Cryptography.HMACSHA256
    $sha256.Key = $keyBytes
    $calculatedHash = $sha256.ComputeHash($bytesToHash)
    $encodedHash = [Convert]::ToBase64String($calculatedHash)
    $authorization = 'SharedKey {0}:{1}' -f $customerId,$encodedHash
    return $authorization
}


# Create hello function toocreate and post hello request
Function Post-OMSData($customerId, $sharedKey, $body, $logType)
{
    $method = "POST"
    $contentType = "application/json"
    $resource = "/api/logs"
    $rfc1123date = [DateTime]::UtcNow.ToString("r")
    $contentLength = $body.Length
    $signature = Build-Signature `
        -customerId $customerId `
        -sharedKey $sharedKey `
        -date $rfc1123date `
        -contentLength $contentLength `
        -fileName $fileName `
        -method $method `
        -contentType $contentType `
        -resource $resource
    $uri = "https://" + $customerId + ".ods.opinsights.azure.com" + $resource + "?api-version=2016-04-01"

    $headers = @{
        "Authorization" = $signature;
        "Log-Type" = $logType;
        "x-ms-date" = $rfc1123date;
        "time-generated-field" = $TimeStampField;
    }

    $response = Invoke-WebRequest -Uri $uri -Method $method -ContentType $contentType -Headers $headers -Body $body -UseBasicParsing
    return $response.StatusCode

}

# Submit hello data toohello API endpoint
Post-OMSData -customerId $customerId -sharedKey $sharedKey -body ([System.Text.Encoding]::UTF8.GetBytes($json)) -logType $logType  
```

### <a name="c-sample"></a><span data-ttu-id="d03d7-303">C#-exempel</span><span class="sxs-lookup"><span data-stu-id="d03d7-303">C# sample</span></span>
```
using System;
using System.Net;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Security.Cryptography;
using System.Text;
using System.Threading.Tasks;

namespace OIAPIExample
{
    class ApiExample
    {
        // An example JSON object, with key/value pairs
        static string json = @"[{""DemoField1"":""DemoValue1"",""DemoField2"":""DemoValue2""},{""DemoField3"":""DemoValue3"",""DemoField4"":""DemoValue4""}]";

        // Update customerId tooyour Operations Management Suite workspace ID
        static string customerId = "xxxxxxxx-xxx-xxx-xxx-xxxxxxxxxxxx";

        // For sharedKey, use either hello primary or hello secondary Connected Sources client authentication key   
        static string sharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx";

        // LogName is name of hello event type that is being submitted tooLog Analytics
        static string LogName = "DemoExample";

        // You can use an optional field toospecify hello timestamp from hello data. If hello time field is not specified, Log Analytics assumes hello time is hello message ingestion time
        static string TimeStampField = "";

        static void Main()
        {
            // Create a hash for hello API signature
            var datestring = DateTime.UtcNow.ToString("r");
            string stringToHash = "POST\n" + json.Length + "\napplication/json\n" + "x-ms-date:" + datestring + "\n/api/logs";
            string hashedString = BuildSignature(stringToHash, sharedKey);
            string signature = "SharedKey " + customerId + ":" + hashedString;

            PostData(signature, datestring, json);
        }

        // Build hello API signature
        public static string BuildSignature(string message, string secret)
        {
            var encoding = new System.Text.ASCIIEncoding();
            byte[] keyByte = Convert.FromBase64String(secret);
            byte[] messageBytes = encoding.GetBytes(message);
            using (var hmacsha256 = new HMACSHA256(keyByte))
            {
                byte[] hash = hmacsha256.ComputeHash(messageBytes);
                return Convert.ToBase64String(hash);
            }
        }

        // Send a request toohello POST API endpoint
        public static void PostData(string signature, string date, string json)
        {
            try
            {
                string url = "https://" + customerId + ".ods.opinsights.azure.com/api/logs?api-version=2016-04-01";

                System.Net.Http.HttpClient client = new System.Net.Http.HttpClient();
                client.DefaultRequestHeaders.Add("Accept", "application/json");
                client.DefaultRequestHeaders.Add("Log-Type", LogName);
                client.DefaultRequestHeaders.Add("Authorization", signature);
                client.DefaultRequestHeaders.Add("x-ms-date", date);
                client.DefaultRequestHeaders.Add("time-generated-field", TimeStampField);

                System.Net.Http.HttpContent httpContent = new StringContent(json, Encoding.UTF8);
                httpContent.Headers.ContentType = new MediaTypeHeaderValue("application/json");
                Task<System.Net.Http.HttpResponseMessage> response = client.PostAsync(new Uri(url), httpContent);

                System.Net.Http.HttpContent responseContent = response.Result.Content;
                string result = responseContent.ReadAsStringAsync().Result;
                Console.WriteLine("Return Result: " + result);
            }
            catch (Exception excep)
            {
                Console.WriteLine("API Post Exception: " + excep.Message);
            }
        }
    }
}

```

### <a name="python-sample"></a><span data-ttu-id="d03d7-304">Python-exempel</span><span class="sxs-lookup"><span data-stu-id="d03d7-304">Python sample</span></span>
```
import json
import requests
import datetime
import hashlib
import hmac
import base64

# Update hello customer ID tooyour Operations Management Suite workspace ID
customer_id = 'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'

# For hello shared key, use either hello primary or hello secondary Connected Sources client authentication key   
shared_key = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# hello log type is hello name of hello event that is being submitted
log_type = 'WebMonitorTest'

# An example JSON web monitor object
json_data = [{
   "slot_ID": 12345,
    "ID": "5cdad72f-c848-4df0-8aaa-ffe033e75d57",
    "availability_Value": 100,
    "performance_Value": 6.954,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "true"
},
{   
    "slot_ID": 67890,
    "ID": "b6bee458-fb65-492e-996d-61c4d7fbb942",
    "availability_Value": 100,
    "performance_Value": 3.379,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "false"
}]
body = json.dumps(json_data)

#####################
######Functions######  
#####################

# Build hello API signature
def build_signature(customer_id, shared_key, date, content_length, method, content_type, resource):
    x_headers = 'x-ms-date:' + date
    string_to_hash = method + "\n" + str(content_length) + "\n" + content_type + "\n" + x_headers + "\n" + resource
    bytes_to_hash = bytes(string_to_hash).encode('utf-8')  
    decoded_key = base64.b64decode(shared_key)
    encoded_hash = base64.b64encode(hmac.new(decoded_key, bytes_to_hash, digestmod=hashlib.sha256).digest())
    authorization = "SharedKey {}:{}".format(customer_id,encoded_hash)
    return authorization

# Build and send a request toohello POST API
def post_data(customer_id, shared_key, body, log_type):
    method = 'POST'
    content_type = 'application/json'
    resource = '/api/logs'
    rfc1123date = datetime.datetime.utcnow().strftime('%a, %d %b %Y %H:%M:%S GMT')
    content_length = len(body)
    signature = build_signature(customer_id, shared_key, rfc1123date, content_length, method, content_type, resource)
    uri = 'https://' + customer_id + '.ods.opinsights.azure.com' + resource + '?api-version=2016-04-01'

    headers = {
        'content-type': content_type,
        'Authorization': signature,
        'Log-Type': log_type,
        'x-ms-date': rfc1123date
    }

    response = requests.post(uri,data=body, headers=headers)
    if (response.status_code >= 200 and response.status_code <= 299):
        print 'Accepted'
    else:
        print "Response code: {}".format(response.status_code)

post_data(customer_id, shared_key, body, log_type)
```

## <a name="next-steps"></a><span data-ttu-id="d03d7-305">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d03d7-305">Next steps</span></span>
- <span data-ttu-id="d03d7-306">Använd hello [loggen Sök API](log-analytics-log-search-api.md) tooretrieve data från hello logganalys-databasen.</span><span class="sxs-lookup"><span data-stu-id="d03d7-306">Use hello [Log Search API](log-analytics-log-search-api.md) tooretrieve data from hello Log Analytics repository.</span></span>
