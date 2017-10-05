---
title: Logga Analytics HTTP datainsamlaren API | Microsoft Docs
description: "Du kan använda Log Analytics HTTP Data Collector API för att lägga till POST JSON-data i logganalys-databasen från klienter som kan anropa REST-API. Den här artikeln beskriver hur du använder API: et och har exempel på hur du publicerar data med hjälp av olika programmeringsspråk."
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
ms.openlocfilehash: b0c45ff8c1d4c9d35fbb3c8839b38a20df277055
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="send-data-to-log-analytics-with-the-http-data-collector-api"></a><span data-ttu-id="a932d-104">Skicka data till logganalys med HTTP-Data Collector API</span><span class="sxs-lookup"><span data-stu-id="a932d-104">Send data to Log Analytics with the HTTP Data Collector API</span></span>
<span data-ttu-id="a932d-105">Den här artikeln visar hur du använder HTTP-Data Collector API för att skicka data till logganalys från en REST API-klient.</span><span class="sxs-lookup"><span data-stu-id="a932d-105">This article shows you how to use the HTTP Data Collector API to send data to Log Analytics from a REST API client.</span></span>  <span data-ttu-id="a932d-106">Det beskriver hur du formatera data som samlas in av skript eller program, inkludera den i en begäran och har den begäran som auktoriserad genom logganalys.</span><span class="sxs-lookup"><span data-stu-id="a932d-106">It describes how to format data collected by your script or application, include it in a request, and have that request authorized by Log Analytics.</span></span>  <span data-ttu-id="a932d-107">Exempel för för PowerShell, C# och Python.</span><span class="sxs-lookup"><span data-stu-id="a932d-107">Examples are provided for PowerShell, C#, and Python.</span></span>

## <a name="concepts"></a><span data-ttu-id="a932d-108">Koncept</span><span class="sxs-lookup"><span data-stu-id="a932d-108">Concepts</span></span>
<span data-ttu-id="a932d-109">Du kan använda HTTP Data Collector-API: et för att skicka data till logganalys från klienter som kan anropa REST-API.</span><span class="sxs-lookup"><span data-stu-id="a932d-109">You can use the HTTP Data Collector API to send data to Log Analytics from any client that can call a REST API.</span></span>  <span data-ttu-id="a932d-110">Detta kan bero på en runbook i Azure Automation som samlar in hantering av data från Azure eller ett annat moln, eller så kan vara ett alternativ-system som använder Log Analytics för att sammanställa och analysera data.</span><span class="sxs-lookup"><span data-stu-id="a932d-110">This might be a runbook in Azure Automation that collects management data from Azure or another cloud, or it might be an alternate management system that uses Log Analytics to consolidate and analyze data.</span></span>

<span data-ttu-id="a932d-111">Alla data i logganalys-databasen lagras som en post med en viss posttyp.</span><span class="sxs-lookup"><span data-stu-id="a932d-111">All data in the Log Analytics repository is stored as a record with a particular record type.</span></span>  <span data-ttu-id="a932d-112">Du kan formatera dina data ska skickas till http-Data Collector API som flera poster i JSON.</span><span class="sxs-lookup"><span data-stu-id="a932d-112">You format your data to send to the HTTP Data Collector API as multiple records in JSON.</span></span>  <span data-ttu-id="a932d-113">När du skickar data, skapas en enskild post i databasen för varje post i nyttolasten i begäran.</span><span class="sxs-lookup"><span data-stu-id="a932d-113">When you submit the data, an individual record is created in the repository for each record in the request payload.</span></span>


![Översikt över HTTP-datainsamlare](media/log-analytics-data-collector-api/overview.png)



## <a name="create-a-request"></a><span data-ttu-id="a932d-115">Skapa en förfrågan</span><span class="sxs-lookup"><span data-stu-id="a932d-115">Create a request</span></span>
<span data-ttu-id="a932d-116">Om du vill använda HTTP Data Collector-API: et kan du skapa en POST-begäran som innehåller data som ska skickas i JavaScript Object Notation (JSON).</span><span class="sxs-lookup"><span data-stu-id="a932d-116">To use the HTTP Data Collector API, you create a POST request that includes the data to send in JavaScript Object Notation (JSON).</span></span>  <span data-ttu-id="a932d-117">I följande tre tabeller anges de attribut som krävs för varje begäran.</span><span class="sxs-lookup"><span data-stu-id="a932d-117">The next three tables list the attributes that are required for each request.</span></span> <span data-ttu-id="a932d-118">Vi beskriver varje attribut i detalj senare i artikeln.</span><span class="sxs-lookup"><span data-stu-id="a932d-118">We describe each attribute in more detail later in the article.</span></span>

### <a name="request-uri"></a><span data-ttu-id="a932d-119">URI-begäran</span><span class="sxs-lookup"><span data-stu-id="a932d-119">Request URI</span></span>
| <span data-ttu-id="a932d-120">Attribut</span><span class="sxs-lookup"><span data-stu-id="a932d-120">Attribute</span></span> | <span data-ttu-id="a932d-121">Egenskap</span><span class="sxs-lookup"><span data-stu-id="a932d-121">Property</span></span> |
|:--- |:--- |
| <span data-ttu-id="a932d-122">Metod</span><span class="sxs-lookup"><span data-stu-id="a932d-122">Method</span></span> |<span data-ttu-id="a932d-123">POST</span><span class="sxs-lookup"><span data-stu-id="a932d-123">POST</span></span> |
| <span data-ttu-id="a932d-124">URI: N</span><span class="sxs-lookup"><span data-stu-id="a932d-124">URI</span></span> |<span data-ttu-id="a932d-125">https://\<CustomerId\>.ods.opinsights.azure.com/api/logs?api-version=2016-04-01</span><span class="sxs-lookup"><span data-stu-id="a932d-125">https://\<CustomerId\>.ods.opinsights.azure.com/api/logs?api-version=2016-04-01</span></span> |
| <span data-ttu-id="a932d-126">Innehållstyp</span><span class="sxs-lookup"><span data-stu-id="a932d-126">Content type</span></span> |<span data-ttu-id="a932d-127">application/json</span><span class="sxs-lookup"><span data-stu-id="a932d-127">application/json</span></span> |

### <a name="request-uri-parameters"></a><span data-ttu-id="a932d-128">URI-parametrar för begäran</span><span class="sxs-lookup"><span data-stu-id="a932d-128">Request URI parameters</span></span>
| <span data-ttu-id="a932d-129">Parameter</span><span class="sxs-lookup"><span data-stu-id="a932d-129">Parameter</span></span> | <span data-ttu-id="a932d-130">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a932d-130">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="a932d-131">CustomerID</span><span class="sxs-lookup"><span data-stu-id="a932d-131">CustomerID</span></span> |<span data-ttu-id="a932d-132">Den unika identifieraren för Microsoft Operations Management Suite-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="a932d-132">The unique identifier for the Microsoft Operations Management Suite workspace.</span></span> |
| <span data-ttu-id="a932d-133">Resurs</span><span class="sxs-lookup"><span data-stu-id="a932d-133">Resource</span></span> |<span data-ttu-id="a932d-134">Resursnamnet API: / api/logs.</span><span class="sxs-lookup"><span data-stu-id="a932d-134">The API resource name: /api/logs.</span></span> |
| <span data-ttu-id="a932d-135">API-Version</span><span class="sxs-lookup"><span data-stu-id="a932d-135">API Version</span></span> |<span data-ttu-id="a932d-136">Versionen av API: et för användning med denna begäran.</span><span class="sxs-lookup"><span data-stu-id="a932d-136">The version of the API to use with this request.</span></span> <span data-ttu-id="a932d-137">Det är för närvarande 2016-04-01.</span><span class="sxs-lookup"><span data-stu-id="a932d-137">Currently, it's 2016-04-01.</span></span> |

### <a name="request-headers"></a><span data-ttu-id="a932d-138">Huvuden för begäran</span><span class="sxs-lookup"><span data-stu-id="a932d-138">Request headers</span></span>
| <span data-ttu-id="a932d-139">Huvudet</span><span class="sxs-lookup"><span data-stu-id="a932d-139">Header</span></span> | <span data-ttu-id="a932d-140">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a932d-140">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="a932d-141">Auktorisering</span><span class="sxs-lookup"><span data-stu-id="a932d-141">Authorization</span></span> |<span data-ttu-id="a932d-142">Signaturen för auktorisering.</span><span class="sxs-lookup"><span data-stu-id="a932d-142">The authorization signature.</span></span> <span data-ttu-id="a932d-143">Senare i artikeln kan du läsa om hur du skapar ett HMAC SHA256-huvud.</span><span class="sxs-lookup"><span data-stu-id="a932d-143">Later in the article, you can read about how to create an HMAC-SHA256 header.</span></span> |
| <span data-ttu-id="a932d-144">Typ</span><span class="sxs-lookup"><span data-stu-id="a932d-144">Log-Type</span></span> |<span data-ttu-id="a932d-145">Ange posttyp för de data som skickas.</span><span class="sxs-lookup"><span data-stu-id="a932d-145">Specify the record type of the data that is being submitted.</span></span> <span data-ttu-id="a932d-146">Loggtyp stöder för närvarande endast alfanumeriska tecken.</span><span class="sxs-lookup"><span data-stu-id="a932d-146">Currently, the log type supports only alpha characters.</span></span> <span data-ttu-id="a932d-147">Stöder inte siffror eller specialtecken.</span><span class="sxs-lookup"><span data-stu-id="a932d-147">It does not support numerics or special characters.</span></span> |
| <span data-ttu-id="a932d-148">x-ms-date</span><span class="sxs-lookup"><span data-stu-id="a932d-148">x-ms-date</span></span> |<span data-ttu-id="a932d-149">Det datum då begäran bearbetades i RFC 1123 format.</span><span class="sxs-lookup"><span data-stu-id="a932d-149">The date that the request was processed, in RFC 1123 format.</span></span> |
| <span data-ttu-id="a932d-150">tid genererade fält</span><span class="sxs-lookup"><span data-stu-id="a932d-150">time-generated-field</span></span> |<span data-ttu-id="a932d-151">Namnet på ett fält i de data som innehåller dataobjektet tidsstämpel.</span><span class="sxs-lookup"><span data-stu-id="a932d-151">The name of a field in the data that contains the timestamp of the data item.</span></span> <span data-ttu-id="a932d-152">Om du anger ett fält och dess innehåll används för **TimeGenerated**.</span><span class="sxs-lookup"><span data-stu-id="a932d-152">If you specify a field then its contents are used for **TimeGenerated**.</span></span> <span data-ttu-id="a932d-153">Om det här fältet har inte angetts, standard för **TimeGenerated** är den tid som meddelandet inhämtas.</span><span class="sxs-lookup"><span data-stu-id="a932d-153">If this field isn’t specified, the default for **TimeGenerated** is the time that the message is ingested.</span></span> <span data-ttu-id="a932d-154">Innehållet i fältet meddelande bör följa ISO 8601-formatet ÅÅÅÅ-MM-ddTHH.</span><span class="sxs-lookup"><span data-stu-id="a932d-154">The contents of the message field should follow the ISO 8601 format YYYY-MM-DDThh:mm:ssZ.</span></span> |

## <a name="authorization"></a><span data-ttu-id="a932d-155">Auktorisering</span><span class="sxs-lookup"><span data-stu-id="a932d-155">Authorization</span></span>
<span data-ttu-id="a932d-156">Alla förfrågningar till Log Analytics HTTP Data Collector API måste innehålla ett authorization-huvud.</span><span class="sxs-lookup"><span data-stu-id="a932d-156">Any request to the Log Analytics HTTP Data Collector API must include an authorization header.</span></span> <span data-ttu-id="a932d-157">För att autentisera en begäran, måste du registrera begäran med primärt eller den sekundära nyckeln för den arbetsyta som begäran kommer ifrån.</span><span class="sxs-lookup"><span data-stu-id="a932d-157">To authenticate a request, you must sign the request with either the primary or the secondary key for the workspace that is making the request.</span></span> <span data-ttu-id="a932d-158">Sedan överföra signaturen som en del av begäran.</span><span class="sxs-lookup"><span data-stu-id="a932d-158">Then, pass that signature as part of the request.</span></span>   

<span data-ttu-id="a932d-159">Här är formatet för authorization-huvud:</span><span class="sxs-lookup"><span data-stu-id="a932d-159">Here's the format for the authorization header:</span></span>

```
Authorization: SharedKey <WorkspaceID>:<Signature>
```

<span data-ttu-id="a932d-160">*WorkspaceID* är den unika identifieraren för Operations Management Suite-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="a932d-160">*WorkspaceID* is the unique identifier for the Operations Management Suite workspace.</span></span> <span data-ttu-id="a932d-161">*Signaturen* är en [hashbaserad meddelandeautentiseringskod (HMAC)](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) som skapas från begäran och sedan beräknas med hjälp av den [SHA256-algoritmen](https://msdn.microsoft.com/library/system.security.cryptography.sha256.aspx).</span><span class="sxs-lookup"><span data-stu-id="a932d-161">*Signature* is a [Hash-based Message Authentication Code (HMAC)](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) that is constructed from the request and then computed by using the [SHA256 algorithm](https://msdn.microsoft.com/library/system.security.cryptography.sha256.aspx).</span></span> <span data-ttu-id="a932d-162">Sedan, koda den med hjälp av Base64-kodning.</span><span class="sxs-lookup"><span data-stu-id="a932d-162">Then, you encode it by using Base64 encoding.</span></span>

<span data-ttu-id="a932d-163">Använd följande format för att koda den **SharedKey** signatur sträng:</span><span class="sxs-lookup"><span data-stu-id="a932d-163">Use this format to encode the **SharedKey** signature string:</span></span>

```
StringToSign = VERB + "\n" +
                  Content-Length + "\n" +
               Content-Type + "\n" +
                  x-ms-date + "\n" +
                  "/api/logs";
```

<span data-ttu-id="a932d-164">Här är ett exempel på en signatur-sträng:</span><span class="sxs-lookup"><span data-stu-id="a932d-164">Here's an example of a signature string:</span></span>

```
POST\n1024\napplication/json\nx-ms-date:Mon, 04 Apr 2016 08:00:00 GMT\n/api/logs
```

<span data-ttu-id="a932d-165">När du har en signatur-sträng, koda med hjälp av HMAC SHA256-algoritmen på UTF-8-kodad sträng och koda resultatet som Base64.</span><span class="sxs-lookup"><span data-stu-id="a932d-165">When you have the signature string, encode it by using the HMAC-SHA256 algorithm on the UTF-8-encoded string, and then encode the result as Base64.</span></span> <span data-ttu-id="a932d-166">Använd följande format:</span><span class="sxs-lookup"><span data-stu-id="a932d-166">Use this format:</span></span>

```
Signature=Base64(HMAC-SHA256(UTF8(StringToSign)))
```

<span data-ttu-id="a932d-167">Exemplen i nästa avsnitt har exempelkod för att skapa ett authorization-huvud.</span><span class="sxs-lookup"><span data-stu-id="a932d-167">The samples in the next sections have sample code to help you create an authorization header.</span></span>

## <a name="request-body"></a><span data-ttu-id="a932d-168">Begärandetexten</span><span class="sxs-lookup"><span data-stu-id="a932d-168">Request body</span></span>
<span data-ttu-id="a932d-169">Innehållet i meddelandet måste vara i JSON.</span><span class="sxs-lookup"><span data-stu-id="a932d-169">The body of the message must be in JSON.</span></span> <span data-ttu-id="a932d-170">Det måste innehålla en eller flera poster med egenskapen namn och värdepar i det här formatet:</span><span class="sxs-lookup"><span data-stu-id="a932d-170">It must include one or more records with the property name and value pairs in this format:</span></span>

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

<span data-ttu-id="a932d-171">Du kan batch flera poster tillsammans i en enskild begäran med hjälp av följande format.</span><span class="sxs-lookup"><span data-stu-id="a932d-171">You can batch multiple records together in a single request by using the following format.</span></span> <span data-ttu-id="a932d-172">Alla poster måste vara samma posttyp.</span><span class="sxs-lookup"><span data-stu-id="a932d-172">All the records must be the same record type.</span></span>

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

## <a name="record-type-and-properties"></a><span data-ttu-id="a932d-173">Posttyp och egenskaper</span><span class="sxs-lookup"><span data-stu-id="a932d-173">Record type and properties</span></span>
<span data-ttu-id="a932d-174">Du kan definiera en anpassad posttyp när du skickar data via Log Analytics HTTP Data Collector API.</span><span class="sxs-lookup"><span data-stu-id="a932d-174">You define a custom record type when you submit data through the Log Analytics HTTP Data Collector API.</span></span> <span data-ttu-id="a932d-175">För närvarande skrivning inte av data till befintliga-posttyper som har skapats av andra typer av data och lösningar.</span><span class="sxs-lookup"><span data-stu-id="a932d-175">Currently, you can't write data to existing record types that were created by other data types and solutions.</span></span> <span data-ttu-id="a932d-176">Logganalys läser inkommande data och skapar sedan egenskaper som matchar datatyper med värden som du anger.</span><span class="sxs-lookup"><span data-stu-id="a932d-176">Log Analytics reads the incoming data and then creates properties that match the data types of the values that you enter.</span></span>

<span data-ttu-id="a932d-177">Varje begäran Log Analytics-API: et måste innehålla en **loggtyp** huvud med namnet för typ av post.</span><span class="sxs-lookup"><span data-stu-id="a932d-177">Each request to the Log Analytics API must include a **Log-Type** header with the name for the record type.</span></span> <span data-ttu-id="a932d-178">Suffixet **_CL** läggs automatiskt till namnet du anger för att skilja den från andra typer av loggen som en anpassad logg.</span><span class="sxs-lookup"><span data-stu-id="a932d-178">The suffix **_CL** is automatically appended to the name you enter to distinguish it from other log types as a custom log.</span></span> <span data-ttu-id="a932d-179">Om du anger namnet till exempel **MyNewRecordType**, logganalys skapas en post med typen **MyNewRecordType_CL**.</span><span class="sxs-lookup"><span data-stu-id="a932d-179">For example, if you enter the name **MyNewRecordType**, Log Analytics creates a record with the type **MyNewRecordType_CL**.</span></span> <span data-ttu-id="a932d-180">Detta säkerställer att det inte finns några konflikter mellan användarskapade typnamn och de levereras i aktuellt eller framtida Microsoft solutions.</span><span class="sxs-lookup"><span data-stu-id="a932d-180">This helps ensure that there are no conflicts between user-created type names and those shipped in current or future Microsoft solutions.</span></span>

<span data-ttu-id="a932d-181">Lägger till ett suffix till egenskapsnamnet för att identifiera en egenskapens datatyp logganalys.</span><span class="sxs-lookup"><span data-stu-id="a932d-181">To identify a property's data type, Log Analytics adds a suffix to the property name.</span></span> <span data-ttu-id="a932d-182">Om en egenskap innehåller ett null-värde, ingår inte egenskapen i posten.</span><span class="sxs-lookup"><span data-stu-id="a932d-182">If a property contains a null value, the property is not included in that record.</span></span> <span data-ttu-id="a932d-183">Den här tabellen innehåller egenskapsdatatyp och motsvarande suffix:</span><span class="sxs-lookup"><span data-stu-id="a932d-183">This table lists the property data type and corresponding suffix:</span></span>

| <span data-ttu-id="a932d-184">Datatypen för egenskapen</span><span class="sxs-lookup"><span data-stu-id="a932d-184">Property data type</span></span> | <span data-ttu-id="a932d-185">Suffix</span><span class="sxs-lookup"><span data-stu-id="a932d-185">Suffix</span></span> |
|:--- |:--- |
| <span data-ttu-id="a932d-186">Sträng</span><span class="sxs-lookup"><span data-stu-id="a932d-186">String</span></span> |<span data-ttu-id="a932d-187">_S</span><span class="sxs-lookup"><span data-stu-id="a932d-187">_s</span></span> |
| <span data-ttu-id="a932d-188">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="a932d-188">Boolean</span></span> |<span data-ttu-id="a932d-189">_b</span><span class="sxs-lookup"><span data-stu-id="a932d-189">_b</span></span> |
| <span data-ttu-id="a932d-190">dubbla</span><span class="sxs-lookup"><span data-stu-id="a932d-190">Double</span></span> |<span data-ttu-id="a932d-191">_D</span><span class="sxs-lookup"><span data-stu-id="a932d-191">_d</span></span> |
| <span data-ttu-id="a932d-192">Datum/tid</span><span class="sxs-lookup"><span data-stu-id="a932d-192">Date/time</span></span> |<span data-ttu-id="a932d-193">_Tätt</span><span class="sxs-lookup"><span data-stu-id="a932d-193">_t</span></span> |
| <span data-ttu-id="a932d-194">GUID</span><span class="sxs-lookup"><span data-stu-id="a932d-194">GUID</span></span> |<span data-ttu-id="a932d-195">_g</span><span class="sxs-lookup"><span data-stu-id="a932d-195">_g</span></span> |

<span data-ttu-id="a932d-196">Datatypen som Log Analytics använder för varje egenskap beror på om posttypen för den nya posten redan finns.</span><span class="sxs-lookup"><span data-stu-id="a932d-196">The data type that Log Analytics uses for each property depends on whether the record type for the new record already exists.</span></span>

* <span data-ttu-id="a932d-197">Om typ av post inte finns, skapar en ny logganalys.</span><span class="sxs-lookup"><span data-stu-id="a932d-197">If the record type does not exist, Log Analytics creates a new one.</span></span> <span data-ttu-id="a932d-198">Log Analytics använder JSON-typhärledning för att fastställa datatypen för varje egenskap för den nya posten.</span><span class="sxs-lookup"><span data-stu-id="a932d-198">Log Analytics uses the JSON type inference to determine the data type for each property for the new record.</span></span>
* <span data-ttu-id="a932d-199">Om typ av post finns försöker logganalys skapa en ny post baserat på befintliga egenskaper.</span><span class="sxs-lookup"><span data-stu-id="a932d-199">If the record type does exist, Log Analytics attempts to create a new record based on existing properties.</span></span> <span data-ttu-id="a932d-200">Om datatypen för en egenskap i den nya posten matchar inte och kan inte konverteras till den befintliga typen, eller om posten innehåller en egenskap som inte finns, Log Analytics skapar en ny egenskap som har suffixet relevanta.</span><span class="sxs-lookup"><span data-stu-id="a932d-200">If the data type for a property in the new record doesn’t match and can’t be converted to the existing type, or if the record includes a property that doesn’t exist, Log Analytics creates a new property that has the relevant suffix.</span></span>

<span data-ttu-id="a932d-201">Skicka posten skulle till exempel skapa en post med tre egenskaper **number_d**, **boolean_b**, och **string_s**:</span><span class="sxs-lookup"><span data-stu-id="a932d-201">For example, this submission entry would create a record with three properties, **number_d**, **boolean_b**, and **string_s**:</span></span>

![Exempelpost 1](media/log-analytics-data-collector-api/record-01.png)

<span data-ttu-id="a932d-203">Om du sedan skickat den här nästa post med alla värden som är formaterade som strängar, skulle det inte att ändra egenskaperna.</span><span class="sxs-lookup"><span data-stu-id="a932d-203">If you then submitted this next entry, with all values formatted as strings, the properties would not change.</span></span> <span data-ttu-id="a932d-204">Dessa värden kan konverteras till befintliga datatyper:</span><span class="sxs-lookup"><span data-stu-id="a932d-204">These values can be converted to existing data types:</span></span>

![Exempelpost 2](media/log-analytics-data-collector-api/record-02.png)

<span data-ttu-id="a932d-206">Men om du har gjort den här nästa skickas sedan Log Analytics skapar nya egenskaper **boolean_d** och **string_d**.</span><span class="sxs-lookup"><span data-stu-id="a932d-206">But, if you then made this next submission, Log Analytics would create the new properties **boolean_d** and **string_d**.</span></span> <span data-ttu-id="a932d-207">Dessa värden kan inte konverteras:</span><span class="sxs-lookup"><span data-stu-id="a932d-207">These values can't be converted:</span></span>

![Exempelpost 3](media/log-analytics-data-collector-api/record-03.png)

<span data-ttu-id="a932d-209">Om du sedan skickat följande post innan typ av post skapades logganalys skulle skapa en post med tre egenskaper **antal_l**, **boolean_s**, och **string_s**.</span><span class="sxs-lookup"><span data-stu-id="a932d-209">If you then submitted the following entry, before the record type was created, Log Analytics would create a record with three properties, **number_s**, **boolean_s**, and **string_s**.</span></span> <span data-ttu-id="a932d-210">I den här posten formateras var och en av de initiala värdena som en sträng:</span><span class="sxs-lookup"><span data-stu-id="a932d-210">In this entry, each of the initial values is formatted as a string:</span></span>

![Exempelpost 4](media/log-analytics-data-collector-api/record-04.png)

## <a name="data-limits"></a><span data-ttu-id="a932d-212">Databegränsningar</span><span class="sxs-lookup"><span data-stu-id="a932d-212">Data limits</span></span>
<span data-ttu-id="a932d-213">Det finns vissa begränsningar runt data som skickats till samlingen Log Analytics-Data API.</span><span class="sxs-lookup"><span data-stu-id="a932d-213">There are some constraints around the data posted to the Log Analytics Data collection API.</span></span>

* <span data-ttu-id="a932d-214">Högst 30 MB per post till Log Analytics Data Collector API: et.</span><span class="sxs-lookup"><span data-stu-id="a932d-214">Maximum of 30 MB per post to Log Analytics Data Collector API.</span></span> <span data-ttu-id="a932d-215">Det här är en storleksgräns för en enskild post.</span><span class="sxs-lookup"><span data-stu-id="a932d-215">This is a size limit for a single post.</span></span> <span data-ttu-id="a932d-216">Om data från en enda bokför som överskrider 30 MB, bör du dela upp data till mindre storlek segment och skicka dem samtidigt.</span><span class="sxs-lookup"><span data-stu-id="a932d-216">If the data from a single post that exceeds 30 MB, you should split the data up to smaller sized chunks and send them concurrently.</span></span>
* <span data-ttu-id="a932d-217">Högst 32 KB-gränsen för fältvärden.</span><span class="sxs-lookup"><span data-stu-id="a932d-217">Maximum of 32 KB limit for field values.</span></span> <span data-ttu-id="a932d-218">Om värdet är större än 32 KB, kommer data att trunkeras.</span><span class="sxs-lookup"><span data-stu-id="a932d-218">If the field value is greater than 32 KB, the data will be truncated.</span></span>
* <span data-ttu-id="a932d-219">Rekommenderade maximala antalet fält för en viss typ är 50.</span><span class="sxs-lookup"><span data-stu-id="a932d-219">Recommended maximum number of fields for a given type is 50.</span></span> <span data-ttu-id="a932d-220">Det här är en praktisk gräns från en användbarhet och Sök upplevelse perspektiv.</span><span class="sxs-lookup"><span data-stu-id="a932d-220">This is a practical limit from a usability and search experience perspective.</span></span>  

## <a name="return-codes"></a><span data-ttu-id="a932d-221">Returkoder</span><span class="sxs-lookup"><span data-stu-id="a932d-221">Return codes</span></span>
<span data-ttu-id="a932d-222">HTTP-statuskod 200 innebär att begäran har tagits emot för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="a932d-222">The HTTP status code 200 means that the request has been received for processing.</span></span> <span data-ttu-id="a932d-223">Detta anger att åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="a932d-223">This indicates that the operation completed successfully.</span></span>

<span data-ttu-id="a932d-224">Den här tabellen innehåller en fullständig uppsättning statuskoder som tjänsten kan returnera:</span><span class="sxs-lookup"><span data-stu-id="a932d-224">This table lists the complete set of status codes that the service might return:</span></span>

| <span data-ttu-id="a932d-225">Kod</span><span class="sxs-lookup"><span data-stu-id="a932d-225">Code</span></span> | <span data-ttu-id="a932d-226">Status</span><span class="sxs-lookup"><span data-stu-id="a932d-226">Status</span></span> | <span data-ttu-id="a932d-227">Felkod</span><span class="sxs-lookup"><span data-stu-id="a932d-227">Error code</span></span> | <span data-ttu-id="a932d-228">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a932d-228">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a932d-229">200</span><span class="sxs-lookup"><span data-stu-id="a932d-229">200</span></span> |<span data-ttu-id="a932d-230">OKEJ</span><span class="sxs-lookup"><span data-stu-id="a932d-230">OK</span></span> | |<span data-ttu-id="a932d-231">Begäran har accepterats.</span><span class="sxs-lookup"><span data-stu-id="a932d-231">The request was successfully accepted.</span></span> |
| <span data-ttu-id="a932d-232">400</span><span class="sxs-lookup"><span data-stu-id="a932d-232">400</span></span> |<span data-ttu-id="a932d-233">Felaktig begäran</span><span class="sxs-lookup"><span data-stu-id="a932d-233">Bad request</span></span> |<span data-ttu-id="a932d-234">InactiveCustomer</span><span class="sxs-lookup"><span data-stu-id="a932d-234">InactiveCustomer</span></span> |<span data-ttu-id="a932d-235">Arbetsytan har stängts.</span><span class="sxs-lookup"><span data-stu-id="a932d-235">The workspace has been closed.</span></span> |
| <span data-ttu-id="a932d-236">400</span><span class="sxs-lookup"><span data-stu-id="a932d-236">400</span></span> |<span data-ttu-id="a932d-237">Felaktig begäran</span><span class="sxs-lookup"><span data-stu-id="a932d-237">Bad request</span></span> |<span data-ttu-id="a932d-238">InvalidApiVersion</span><span class="sxs-lookup"><span data-stu-id="a932d-238">InvalidApiVersion</span></span> |<span data-ttu-id="a932d-239">API-version som du angett kunde inte identifieras av tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a932d-239">The API version that you specified was not recognized by the service.</span></span> |
| <span data-ttu-id="a932d-240">400</span><span class="sxs-lookup"><span data-stu-id="a932d-240">400</span></span> |<span data-ttu-id="a932d-241">Felaktig begäran</span><span class="sxs-lookup"><span data-stu-id="a932d-241">Bad request</span></span> |<span data-ttu-id="a932d-242">InvalidCustomerId</span><span class="sxs-lookup"><span data-stu-id="a932d-242">InvalidCustomerId</span></span> |<span data-ttu-id="a932d-243">Arbetsyte-ID som angetts är ogiltigt.</span><span class="sxs-lookup"><span data-stu-id="a932d-243">The workspace ID specified is invalid.</span></span> |
| <span data-ttu-id="a932d-244">400</span><span class="sxs-lookup"><span data-stu-id="a932d-244">400</span></span> |<span data-ttu-id="a932d-245">Felaktig begäran</span><span class="sxs-lookup"><span data-stu-id="a932d-245">Bad request</span></span> |<span data-ttu-id="a932d-246">InvalidDataFormat</span><span class="sxs-lookup"><span data-stu-id="a932d-246">InvalidDataFormat</span></span> |<span data-ttu-id="a932d-247">Ogiltigt JSON har skickats.</span><span class="sxs-lookup"><span data-stu-id="a932d-247">Invalid JSON was submitted.</span></span> <span data-ttu-id="a932d-248">Svarstexten kan innehålla mer information om hur du åtgärdar felet.</span><span class="sxs-lookup"><span data-stu-id="a932d-248">The response body might contain more information about how to resolve the error.</span></span> |
| <span data-ttu-id="a932d-249">400</span><span class="sxs-lookup"><span data-stu-id="a932d-249">400</span></span> |<span data-ttu-id="a932d-250">Felaktig begäran</span><span class="sxs-lookup"><span data-stu-id="a932d-250">Bad request</span></span> |<span data-ttu-id="a932d-251">InvalidLogType</span><span class="sxs-lookup"><span data-stu-id="a932d-251">InvalidLogType</span></span> |<span data-ttu-id="a932d-252">Loggtypen som angetts innehåller specialtecken eller siffror.</span><span class="sxs-lookup"><span data-stu-id="a932d-252">The log type specified contained special characters or numerics.</span></span> |
| <span data-ttu-id="a932d-253">400</span><span class="sxs-lookup"><span data-stu-id="a932d-253">400</span></span> |<span data-ttu-id="a932d-254">Felaktig begäran</span><span class="sxs-lookup"><span data-stu-id="a932d-254">Bad request</span></span> |<span data-ttu-id="a932d-255">MissingApiVersion</span><span class="sxs-lookup"><span data-stu-id="a932d-255">MissingApiVersion</span></span> |<span data-ttu-id="a932d-256">API-versionen har inte angetts.</span><span class="sxs-lookup"><span data-stu-id="a932d-256">The API version wasn’t specified.</span></span> |
| <span data-ttu-id="a932d-257">400</span><span class="sxs-lookup"><span data-stu-id="a932d-257">400</span></span> |<span data-ttu-id="a932d-258">Felaktig begäran</span><span class="sxs-lookup"><span data-stu-id="a932d-258">Bad request</span></span> |<span data-ttu-id="a932d-259">MissingContentType</span><span class="sxs-lookup"><span data-stu-id="a932d-259">MissingContentType</span></span> |<span data-ttu-id="a932d-260">Content-type har inte angetts.</span><span class="sxs-lookup"><span data-stu-id="a932d-260">The content type wasn’t specified.</span></span> |
| <span data-ttu-id="a932d-261">400</span><span class="sxs-lookup"><span data-stu-id="a932d-261">400</span></span> |<span data-ttu-id="a932d-262">Felaktig begäran</span><span class="sxs-lookup"><span data-stu-id="a932d-262">Bad request</span></span> |<span data-ttu-id="a932d-263">MissingLogType</span><span class="sxs-lookup"><span data-stu-id="a932d-263">MissingLogType</span></span> |<span data-ttu-id="a932d-264">Loggtyp obligatoriskt värde har angetts.</span><span class="sxs-lookup"><span data-stu-id="a932d-264">The required value log type wasn’t specified.</span></span> |
| <span data-ttu-id="a932d-265">400</span><span class="sxs-lookup"><span data-stu-id="a932d-265">400</span></span> |<span data-ttu-id="a932d-266">Felaktig begäran</span><span class="sxs-lookup"><span data-stu-id="a932d-266">Bad request</span></span> |<span data-ttu-id="a932d-267">UnsupportedContentType</span><span class="sxs-lookup"><span data-stu-id="a932d-267">UnsupportedContentType</span></span> |<span data-ttu-id="a932d-268">Content-type har inte angetts **application/json**.</span><span class="sxs-lookup"><span data-stu-id="a932d-268">The content type was not set to **application/json**.</span></span> |
| <span data-ttu-id="a932d-269">403</span><span class="sxs-lookup"><span data-stu-id="a932d-269">403</span></span> |<span data-ttu-id="a932d-270">Tillåts inte</span><span class="sxs-lookup"><span data-stu-id="a932d-270">Forbidden</span></span> |<span data-ttu-id="a932d-271">InvalidAuthorization</span><span class="sxs-lookup"><span data-stu-id="a932d-271">InvalidAuthorization</span></span> |<span data-ttu-id="a932d-272">Det gick inte att autentisera begäran.</span><span class="sxs-lookup"><span data-stu-id="a932d-272">The service failed to authenticate the request.</span></span> <span data-ttu-id="a932d-273">Kontrollera att arbetsytans ID och anslutningen är giltig.</span><span class="sxs-lookup"><span data-stu-id="a932d-273">Verify that the workspace ID and connection key are valid.</span></span> |
| <span data-ttu-id="a932d-274">404</span><span class="sxs-lookup"><span data-stu-id="a932d-274">404</span></span> |<span data-ttu-id="a932d-275">Det gick inte att hitta</span><span class="sxs-lookup"><span data-stu-id="a932d-275">Not Found</span></span> | | <span data-ttu-id="a932d-276">Antingen den URL som är felaktig eller begäran är för stor.</span><span class="sxs-lookup"><span data-stu-id="a932d-276">Either the URL provided is incorrect, or the request is too large.</span></span> |
| <span data-ttu-id="a932d-277">429</span><span class="sxs-lookup"><span data-stu-id="a932d-277">429</span></span> |<span data-ttu-id="a932d-278">För många begäranden</span><span class="sxs-lookup"><span data-stu-id="a932d-278">Too Many Requests</span></span> | | <span data-ttu-id="a932d-279">En stor mängd data från ditt konto har uppstått med tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a932d-279">The service is experiencing a high volume of data from your account.</span></span> <span data-ttu-id="a932d-280">Försök begäran senare.</span><span class="sxs-lookup"><span data-stu-id="a932d-280">Please retry the request later.</span></span> |
| <span data-ttu-id="a932d-281">500</span><span class="sxs-lookup"><span data-stu-id="a932d-281">500</span></span> |<span data-ttu-id="a932d-282">Internt serverfel</span><span class="sxs-lookup"><span data-stu-id="a932d-282">Internal Server Error</span></span> |<span data-ttu-id="a932d-283">UnspecifiedError</span><span class="sxs-lookup"><span data-stu-id="a932d-283">UnspecifiedError</span></span> |<span data-ttu-id="a932d-284">Tjänsten påträffade ett internt fel.</span><span class="sxs-lookup"><span data-stu-id="a932d-284">The service encountered an internal error.</span></span> <span data-ttu-id="a932d-285">Försök att utföra begäran.</span><span class="sxs-lookup"><span data-stu-id="a932d-285">Please retry the request.</span></span> |
| <span data-ttu-id="a932d-286">503</span><span class="sxs-lookup"><span data-stu-id="a932d-286">503</span></span> |<span data-ttu-id="a932d-287">Tjänsten är inte tillgänglig</span><span class="sxs-lookup"><span data-stu-id="a932d-287">Service Unavailable</span></span> |<span data-ttu-id="a932d-288">ServiceUnavailable</span><span class="sxs-lookup"><span data-stu-id="a932d-288">ServiceUnavailable</span></span> |<span data-ttu-id="a932d-289">Tjänsten är för närvarande inte ta emot begäranden.</span><span class="sxs-lookup"><span data-stu-id="a932d-289">The service currently is unavailable to receive requests.</span></span> <span data-ttu-id="a932d-290">Försök att utföra din begäran.</span><span class="sxs-lookup"><span data-stu-id="a932d-290">Please retry your request.</span></span> |

## <a name="query-data"></a><span data-ttu-id="a932d-291">Frågedata</span><span class="sxs-lookup"><span data-stu-id="a932d-291">Query data</span></span>
<span data-ttu-id="a932d-292">Att fråga efter data skickas av Log Analytics HTTP Data Collector API, söka efter poster med **typen** som är lika med den **LogType** värde som du angav läggas till med **_CL**.</span><span class="sxs-lookup"><span data-stu-id="a932d-292">To query data submitted by the Log Analytics HTTP Data Collector API, search for records with **Type** that is equal to the **LogType** value that you specified, appended with **_CL**.</span></span> <span data-ttu-id="a932d-293">Om du använde exempelvis **MyCustomLog**, och du vill returnera alla poster med **typ = MyCustomLog_CL**.</span><span class="sxs-lookup"><span data-stu-id="a932d-293">For example, if you used **MyCustomLog**, then you'd return all records with **Type=MyCustomLog_CL**.</span></span>

>[!NOTE]
> <span data-ttu-id="a932d-294">Om ditt arbetsområde har uppgraderats till den [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), sedan frågan ovan skulle ändra till följande.</span><span class="sxs-lookup"><span data-stu-id="a932d-294">If your workspace has been upgraded to the [new Log Analytics query language](log-analytics-log-search-upgrade.md), then the above query would change to the following.</span></span>

> `MyCustomLog_CL`

## <a name="sample-requests"></a><span data-ttu-id="a932d-295">Exempel begäranden</span><span class="sxs-lookup"><span data-stu-id="a932d-295">Sample requests</span></span>
<span data-ttu-id="a932d-296">I följande avsnitt hittar du exempel på hur du skickar data till Log Analytics HTTP Data Collector API med hjälp av olika programmeringsspråk.</span><span class="sxs-lookup"><span data-stu-id="a932d-296">In the next sections, you'll find samples of how to submit data to the Log Analytics HTTP Data Collector API by using different programming languages.</span></span>

<span data-ttu-id="a932d-297">För varje prov gör du dessa steg för att ange variabler för authorization-huvud:</span><span class="sxs-lookup"><span data-stu-id="a932d-297">For each sample, do these steps to set the variables for the authorization header:</span></span>

1. <span data-ttu-id="a932d-298">I Operations Management Suite-portal, väljer du den **inställningar** panelen och välj sedan den **anslutna källor** fliken.</span><span class="sxs-lookup"><span data-stu-id="a932d-298">In the Operations Management Suite portal, select the **Settings** tile, and then select the **Connected Sources** tab.</span></span>
2. <span data-ttu-id="a932d-299">Till höger om **arbetsyte-ID**kopiera-ikonen och välj sedan klistra in ID som värde för den **kund-ID** variabeln.</span><span class="sxs-lookup"><span data-stu-id="a932d-299">To the right of **Workspace ID**, select the copy icon, and then paste the ID as the value of the **Customer ID** variable.</span></span>
3. <span data-ttu-id="a932d-300">Till höger om **primärnyckel**kopiera-ikonen och välj sedan klistra in ID som värde för den **delad nyckel** variabeln.</span><span class="sxs-lookup"><span data-stu-id="a932d-300">To the right of **Primary Key**, select the copy icon, and then paste the ID as the value of the **Shared Key** variable.</span></span>

<span data-ttu-id="a932d-301">Du kan också ändra variablerna för den typ av och JSON-data.</span><span class="sxs-lookup"><span data-stu-id="a932d-301">Alternatively, you can change the variables for the log type and JSON data.</span></span>

### <a name="powershell-sample"></a><span data-ttu-id="a932d-302">PowerShell-exempel</span><span class="sxs-lookup"><span data-stu-id="a932d-302">PowerShell sample</span></span>
```
# Replace with your Workspace ID
$CustomerId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"  

# Replace with your Primary Key
$SharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# Specify the name of the record type that you'll be creating
$LogType = "MyRecordType"

# Specify a field with the created time for the records
$TimeStampField = "DateValue"


# Create two records with the same set of properties to create
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

# Create the function to create the authorization signature
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


# Create the function to create and post the request
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

# Submit the data to the API endpoint
Post-OMSData -customerId $customerId -sharedKey $sharedKey -body ([System.Text.Encoding]::UTF8.GetBytes($json)) -logType $logType  
```

### <a name="c-sample"></a><span data-ttu-id="a932d-303">C#-exempel</span><span class="sxs-lookup"><span data-stu-id="a932d-303">C# sample</span></span>
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

        // Update customerId to your Operations Management Suite workspace ID
        static string customerId = "xxxxxxxx-xxx-xxx-xxx-xxxxxxxxxxxx";

        // For sharedKey, use either the primary or the secondary Connected Sources client authentication key   
        static string sharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx";

        // LogName is name of the event type that is being submitted to Log Analytics
        static string LogName = "DemoExample";

        // You can use an optional field to specify the timestamp from the data. If the time field is not specified, Log Analytics assumes the time is the message ingestion time
        static string TimeStampField = "";

        static void Main()
        {
            // Create a hash for the API signature
            var datestring = DateTime.UtcNow.ToString("r");
            string stringToHash = "POST\n" + json.Length + "\napplication/json\n" + "x-ms-date:" + datestring + "\n/api/logs";
            string hashedString = BuildSignature(stringToHash, sharedKey);
            string signature = "SharedKey " + customerId + ":" + hashedString;

            PostData(signature, datestring, json);
        }

        // Build the API signature
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

        // Send a request to the POST API endpoint
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

### <a name="python-sample"></a><span data-ttu-id="a932d-304">Python-exempel</span><span class="sxs-lookup"><span data-stu-id="a932d-304">Python sample</span></span>
```
import json
import requests
import datetime
import hashlib
import hmac
import base64

# Update the customer ID to your Operations Management Suite workspace ID
customer_id = 'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'

# For the shared key, use either the primary or the secondary Connected Sources client authentication key   
shared_key = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# The log type is the name of the event that is being submitted
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

# Build the API signature
def build_signature(customer_id, shared_key, date, content_length, method, content_type, resource):
    x_headers = 'x-ms-date:' + date
    string_to_hash = method + "\n" + str(content_length) + "\n" + content_type + "\n" + x_headers + "\n" + resource
    bytes_to_hash = bytes(string_to_hash).encode('utf-8')  
    decoded_key = base64.b64decode(shared_key)
    encoded_hash = base64.b64encode(hmac.new(decoded_key, bytes_to_hash, digestmod=hashlib.sha256).digest())
    authorization = "SharedKey {}:{}".format(customer_id,encoded_hash)
    return authorization

# Build and send a request to the POST API
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

## <a name="next-steps"></a><span data-ttu-id="a932d-305">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a932d-305">Next steps</span></span>
- <span data-ttu-id="a932d-306">Använd den [loggen Sök API](log-analytics-log-search-api.md) att hämta data från logganalys-databasen.</span><span class="sxs-lookup"><span data-stu-id="a932d-306">Use the [Log Search API](log-analytics-log-search-api.md) to retrieve data from the Log Analytics repository.</span></span>
