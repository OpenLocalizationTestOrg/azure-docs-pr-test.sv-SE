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
# <a name="send-data-toolog-analytics-with-hello-http-data-collector-api"></a>Skicka data tooLog Analytics med hello HTTP Data Collector API: et
Den här artikeln visar hur toouse hello HTTP Data Collector API: et toosend data tooLog Analytics från en REST API-klient.  Det beskrivs hur tooformat data som samlas in av skript eller program, inkludera den i en begäran och har den begäran som auktoriserad genom logganalys.  Exempel för för PowerShell, C# och Python.

## <a name="concepts"></a>Koncept
Du kan använda hello HTTP Data Collector API: et toosend data tooLog Analytics från klienter som kan anropa REST-API.  Detta kan bero på en runbook i Azure Automation som samlar in hantering av data från Azure eller ett annat moln, eller så kan vara ett alternativ-system som använder logganalys tooconsolidate och analysera data.

Alla data i hello logganalys databasen lagras som en post med en viss posttyp.  Du kan formatera dina data toosend toohello HTTP Data Collector API som flera poster i JSON.  När du skickar hello data, skapas en enskild post i hello databas för varje post i hello nyttolasten i begäran.


![Översikt över HTTP-datainsamlare](media/log-analytics-data-collector-api/overview.png)



## <a name="create-a-request"></a>Skapa en förfrågan
toouse hello HTTP datainsamlaren API, skapa en POST-begäran som innehåller hello data toosend i JavaScript Object Notation (JSON).  Hej följande tre tabeller listan hello attribut som krävs för varje begäran. Vi beskriver varje attribut i detalj senare i hello artikeln.

### <a name="request-uri"></a>URI-begäran
| Attribut | Egenskap |
|:--- |:--- |
| Metod |POST |
| URI: N |https://\<CustomerId\>.ods.opinsights.azure.com/api/logs?api-version=2016-04-01 |
| Innehållstyp |application/json |

### <a name="request-uri-parameters"></a>URI-parametrar för begäran
| Parameter | Beskrivning |
|:--- |:--- |
| CustomerID |hello Unik identifierare för hello Microsoft Operations Management Suite-arbetsyta. |
| Resurs |hello API resursnamn: / api/logs. |
| API-Version |hello version av hello API toouse med den här begäran. Det är för närvarande 2016-04-01. |

### <a name="request-headers"></a>Huvuden för begäran
| Huvudet | Beskrivning |
|:--- |:--- |
| Auktorisering |hello auktorisering signatur. Senare i hello artikeln kan du läsa om hur toocreate en HMAC SHA256-huvud. |
| Typ |Ange hello posttyp hello data som skickas. Typ av hello stöder för närvarande endast alfanumeriska tecken. Stöder inte siffror eller specialtecken. |
| x-ms-date |hello datum hello begäran bearbetades i RFC 1123 format. |
| tid genererade fält |hello namnet på ett fält i hello data som innehåller hello tidsstämpel för hello dataobjektet. Om du anger ett fält och dess innehåll används för **TimeGenerated**. Om det här fältet har inte angetts, hello standard för **TimeGenerated** är hello tid att hello meddelandet inhämtas. hello innehållet i fältet för hello-meddelande bör följa hello ISO 8601-formatet ÅÅÅÅ-MM-ddTHH. |

## <a name="authorization"></a>Auktorisering
Alla begäran toohello Log Analytics HTTP Data Collector API måste innehålla ett authorization-huvud. tooauthenticate en begäran, måste du registrera hello begäran med hello primära eller hello sekundärnyckeln för hello-arbetsyta som gör hello-begäran. Sen skicka signaturen i hello-begäran.   

Här är hello format för hello authorization-huvud:

```
Authorization: SharedKey <WorkspaceID>:<Signature>
```

*WorkspaceID* är hello Unik identifierare för hello Operations Management Suite-arbetsyta. *Signaturen* är en [hashbaserad meddelandeautentiseringskod (HMAC)](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) som skapas från hello begäran och sedan beräknas med hjälp av hello [SHA256-algoritmen](https://msdn.microsoft.com/library/system.security.cryptography.sha256.aspx). Sedan, koda den med hjälp av Base64-kodning.

Använd det här formatet tooencode hello **SharedKey** signatur sträng:

```
StringToSign = VERB + "\n" +
                  Content-Length + "\n" +
               Content-Type + "\n" +
                  x-ms-date + "\n" +
                  "/api/logs";
```

Här är ett exempel på en signatur-sträng:

```
POST\n1024\napplication/json\nx-ms-date:Mon, 04 Apr 2016 08:00:00 GMT\n/api/logs
```

När du har hello signatur sträng, koda med hjälp av hello HMAC SHA256-algoritmen på hello UTF-8-kodad sträng och koda hello resultat som Base64. Använd följande format:

```
Signature=Base64(HMAC-SHA256(UTF8(StringToSign)))
```

hello prover i nästa avsnitt av hello ha exempel kod toohelp du skapar ett authorization-huvud.

## <a name="request-body"></a>Begärandetexten
hello innehållet i hello-meddelande måste vara i JSON. Det måste innehålla en eller flera poster med hello egenskapen namn och värdepar i det här formatet:

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

Du kan batch flera poster tillsammans i en enskild begäran med hjälp av hello följande format. Alla hello-poster måste vara hello samma posttyp.

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

## <a name="record-type-and-properties"></a>Posttyp och egenskaper
Du kan definiera en anpassad posttyp när du skickar data via hello Log Analytics HTTP Data Collector API. Du kan inte för närvarande kan skriva data tooexisting posttyper som har skapats av andra typer av data och lösningar. Logganalys läser hello inkommande data och skapar sedan egenskaper som matchar hello datatyperna hello-värden som du anger.

Varje begäran toohello Log Analytics API måste innehålla en **loggtyp** huvud med hello namn för hello posttyp. hello suffix **_CL** är automatiskt tillagda toohello namn som du anger toodistinguish den från andra logga svarstyperna som en anpassad logg. Om du anger hello namn exempelvis **MyNewRecordType**, logganalys skapas en post med hello typen **MyNewRecordType_CL**. Detta säkerställer att det inte finns några konflikter mellan användarskapade typnamn och de levereras i aktuellt eller framtida Microsoft solutions.

datatypen för tooidentify en egenskap, logganalys lägger till ett suffix toohello egenskapsnamn. Om en egenskap innehåller ett null-värde, ingår inte hello-egenskapen i posten. Den här tabellen innehåller hello egenskapsdatatyp och motsvarande suffix:

| Datatypen för egenskapen | Suffix |
|:--- |:--- |
| Sträng |_S |
| Booleskt värde |_b |
| dubbla |_D |
| Datum/tid |_Tätt |
| GUID |_g |

hello-datatyp som Log Analytics använder för varje egenskap beror på om hello posttypen för hello nya posten redan finns.

* Om hello posttypen inte finns, skapar en ny logganalys. Log Analytics använder hello JSON typen härledning toodetermine hello datatyp för varje egenskap för hello nya posten.
* Om hello posttyp finns försöker logganalys toocreate en ny post baserat på befintliga egenskaper. Om hello datatypen för en egenskap i hello nya posten inte matchar och inte går att konvertera toohello befintlig typ eller om hello post innehåller en egenskap som inte finns, Log Analytics skapar en ny egenskap har som relevanta hello-suffix.

Skicka posten skulle till exempel skapa en post med tre egenskaper **number_d**, **boolean_b**, och **string_s**:

![Exempelpost 1](media/log-analytics-data-collector-api/record-01.png)

Om du sedan skickat den här nästa post med alla värden som är formaterade som strängar, ändrar inte hello egenskaper. Dessa värden kan vara konverterade tooexisting datatyper:

![Exempelpost 2](media/log-analytics-data-collector-api/record-02.png)

Men om du har gjort den här nästa skickas sedan Log Analytics skapar hello egenskaper för ny **boolean_d** och **string_d**. Dessa värden kan inte konverteras:

![Exempelpost 3](media/log-analytics-data-collector-api/record-03.png)

Om du sedan skickat hello följande post, innan hello posttyp skapades logganalys skulle skapa en post med tre egenskaper **antal_l**, **boolean_s**, och **string_s**. I den här posten formateras hello inledande värden som en sträng:

![Exempelpost 4](media/log-analytics-data-collector-api/record-04.png)

## <a name="data-limits"></a>Databegränsningar
Det finns vissa begränsningar runt hello-data som skickats toohello API för Log Analytics datainsamling.

* Högst 30 MB per post tooLog Analytics Data Collector API: et. Det här är en storleksgräns för en enskild post. Om hello data från en enda post som är större än 30 MB, du ska dela hello data in toosmaller storlek blocken och skicka dem samtidigt.
* Högst 32 KB-gränsen för fältvärden. Om hello fältvärdet är större än 32 KB trunkeras hello data.
* Rekommenderade maximala antalet fält för en viss typ är 50. Det här är en praktisk gräns från en användbarhet och Sök upplevelse perspektiv.  

## <a name="return-codes"></a>Returkoder
hello HTTP-statuskod 200 innebär att hello-begäran har tagits emot för bearbetning. Detta anger att hello-åtgärden har slutförts.

Den här tabellen innehåller hello fullständig uppsättning statuskoder som kan returnera hello-tjänsten:

| Kod | Status | Felkod | Beskrivning |
|:--- |:--- |:--- |:--- |
| 200 |OKEJ | |hello-begäran har accepterats. |
| 400 |Felaktig begäran |InactiveCustomer |hello-arbetsytan har stängts. |
| 400 |Felaktig begäran |InvalidApiVersion |hello API-version som du angett kunde inte identifieras av hello-tjänsten. |
| 400 |Felaktig begäran |InvalidCustomerId |hello arbetsyte-ID som angetts är ogiltigt. |
| 400 |Felaktig begäran |InvalidDataFormat |Ogiltigt JSON har skickats. hello svarstexten kan innehålla mer information om hur tooresolve hello fel. |
| 400 |Felaktig begäran |InvalidLogType |typ av hello angav innehåller specialtecken eller siffror. |
| 400 |Felaktig begäran |MissingApiVersion |hello API-versionen har inte angetts. |
| 400 |Felaktig begäran |MissingContentType |hello content-type har inte angetts. |
| 400 |Felaktig begäran |MissingLogType |hello krävs värdetyp har inte angetts. |
| 400 |Felaktig begäran |UnsupportedContentType |hello content-type har inte angetts för**application/json**. |
| 403 |Tillåts inte |InvalidAuthorization |Det gick inte att tooauthenticate hello begäran hello-tjänsten. Kontrollera att hello arbetsytenyckel ID och anslutningen är giltiga. |
| 404 |Det gick inte att hitta | | Antingen hello URL: en är felaktig eller hello-begäran är för stor. |
| 429 |För många begäranden | | hello-tjänsten har en stor mängd data från ditt konto. Försök hello begäran senare. |
| 500 |Internt serverfel |UnspecifiedError |hello tjänsten påträffade ett internt fel. Försök hello-begäran. |
| 503 |Tjänsten är inte tillgänglig |ServiceUnavailable |hello-tjänsten är för tillfället otillgänglig tooreceive begäranden. Försök att utföra din begäran. |

## <a name="query-data"></a>Frågedata
tooquery data som skickats av hello Log Analytics HTTP Data Collector API, söka efter poster med **typen** som är lika toohello **LogType** värde som du angav läggas till med **_CL**. Om du använde exempelvis **MyCustomLog**, och du vill returnera alla poster med **typ = MyCustomLog_CL**.

>[!NOTE]
> Om ditt arbetsområde har uppgraderade toohello [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), hello ovan frågan skulle ändra toohello följande.

> `MyCustomLog_CL`

## <a name="sample-requests"></a>Exempel begäranden
I nästa avsnitt hello hittar du exempel på hur toosubmit data toohello Log Analytics HTTP Data Collector API med hjälp av olika programmeringsspråk.

För varje prov gör dessa tooset hello variabler för hello authorization-huvud:

1. Välj hello i hello Operations Management Suite-portalen **inställningar** panelen och välj sedan hello **anslutna källor** fliken.
2. toohello höger i **arbetsyte-ID**hello kopiera-ikonen och välj sedan klistra in hello-ID som hello värde för hello **kund-ID** variabeln.
3. toohello höger i **primärnyckel**hello kopiera-ikonen och välj sedan klistra in hello-ID som hello värde för hello **delad nyckel** variabeln.

Du kan också ändra hello variabler för hello loggtyp och JSON-data.

### <a name="powershell-sample"></a>PowerShell-exempel
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

### <a name="c-sample"></a>C#-exempel
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

### <a name="python-sample"></a>Python-exempel
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

## <a name="next-steps"></a>Nästa steg
- Använd hello [loggen Sök API](log-analytics-log-search-api.md) tooretrieve data från hello logganalys-databasen.
