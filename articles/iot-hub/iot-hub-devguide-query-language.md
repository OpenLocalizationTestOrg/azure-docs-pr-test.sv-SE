---
title: "aaaUnderstand hello Azure IoT Hub-frågespråket | Microsoft Docs"
description: "Utvecklarhandbok - beskrivning av hello SQL-liknande IoT-hubb-frågespråket tooretrieve information om enheten twins och jobb från din IoT-hubb som används."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 851a9ed3-b69e-422e-8a5d-1d79f91ddf15
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/17
ms.author: elioda
ms.openlocfilehash: 01a7c8ffdf44c6c27b834739d02c8fef1dd3d3fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reference---iot-hub-query-language-for-device-twins-jobs-and-message-routing"></a>Referens - frågespråket för IoT-hubb för enheten twins, jobb och meddelanderoutning

IoT-hubb innehåller en kraftfull SQL-liknande språk tooretrieve information angående [enhet twins] [ lnk-twins] och [jobb][lnk-jobs], och [meddelanderoutning][lnk-devguide-messaging-routes]. Den här artikeln beskriver:

* En introduktion toohello viktigaste funktionerna i hello IoT-hubb frågespråket och
* hello detaljerad beskrivning av hello-språket.

## <a name="get-started-with-device-twin-queries"></a>Kom igång med enheten dubbla frågor
[Enheten twins] [ lnk-twins] godtyckliga JSON-objekt kan innehålla både taggar och egenskaper. IoT-hubb kan du tooquery enhet twins som enda JSON-dokument som innehåller alla dubbla enhetsinformation.
Anta exempelvis att din IoT-hubb enheten twins har hello följande struktur:

```json
{
    "deviceId": "myDeviceId",
    "etag": "AAAAAAAAAAc=",
    "tags": {
        "location": {
            "region": "US",
            "plant": "Redmond43"
        }
    },
    "properties": {
        "desired": {
            "telemetryConfig": {
                "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",
                "sendFrequencyInSecs": 300
            },
            "$metadata": {
            ...
            },
            "$version": 4
        },
        "reported": {
            "connectivity": {
                "type": "cellular"
            },
            "telemetryConfig": {
                "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",
                "sendFrequencyInSecs": 300,
                "status": "Success"
            },
            "$metadata": {
            ...
            },
            "$version": 7
        }
    }
}
```

IoT-hubb visar hello enheten twins som en dokumentsamling som heter **enheter**.
Hello följande fråga hämtar så hello hela uppsättningen med enheten twins:

```sql
SELECT * FROM devices
```

> [!NOTE]
> [Azure IoT-SDK] [ lnk-hub-sdks] stöd för sidindelning av stora resultat.

IoT-hubb kan tooretrieve enhet twins med valfri villkor för filtrering. Till exempel

```sql
SELECT * FROM devices
WHERE tags.location.region = 'US'
```

hämtar hello enheten twins med hello **location.region** grupp för**USA**.
Booleska operatorer och aritmetiskt jämförelser stöds, till exempel

```sql
SELECT * FROM devices
WHERE tags.location.region = 'US'
    AND properties.reported.telemetryConfig.sendFrequencyInSecs >= 60
```

hämtar alla enhet twins finns i hello oss konfigurerade toosend telemetri mindre ofta än varje minut. Bekvämlighets skull, är det också möjligt toouse matriskonstanter med hello **IN** och **ni** (inte i) operatörer. Till exempel

```sql
SELECT * FROM devices
WHERE properties.reported.connectivity IN ['wired', 'wifi']
```

hämtar alla enheten twins som rapporteras Wi-Fi eller trådbunden anslutning. Det är ofta nödvändiga tooidentify alla twins för enheten som innehåller en specifik egenskap. IoT-hubb stöder hello funktionen `is_defined()` för detta ändamål. Till exempel

```SQL
SELECT * FROM devices
WHERE is_defined(properties.reported.connectivity)
```

Hämta alla enheten twins som definierar hello `connectivity` rapporterade egenskapen. Se toohello [WHERE-satsen] [ lnk-query-where] avsnitt hello fullständig referens i hello filtreringsfunktioner.

Gruppering och aggregeringar stöds också. Till exempel

```sql
SELECT properties.reported.telemetryConfig.status AS status,
    COUNT() AS numberOfDevices
FROM devices
GROUP BY properties.reported.telemetryConfig.status
```

Returnerar hello antal hello enheter i varje status för konfiguration av telemetri.

```json
[
    {
        "numberOfDevices": 3,
        "status": "Success"
    },
    {
        "numberOfDevices": 2,
        "status": "Pending"
    },
    {
        "numberOfDevices": 1,
        "status": "Error"
    }
]
```

hello illustrerar föregående exempel en situation där tre enheter rapporteras lyckad konfiguration, två fortfarande använda hello konfiguration och en rapporterade ett fel.

### <a name="c-example"></a>C#-exempel
hello frågefunktioner som exponeras av hello [C# service SDK] [ lnk-hub-sdks] i hello **RegistryManager** klass.
Här är ett exempel på en enkel fråga:

```csharp
var query = registryManager.CreateQuery("SELECT * FROM devices", 100);
while (query.HasMoreResults)
{
    var page = await query.GetNextAsTwinAsync();
    foreach (var twin in page)
    {
        // do work on twin object
    }
}
```

Observera hur hello **frågan** instans-objekt med en storlek (upp too1000) och sedan flera sidor kan hämtas genom att anropa hello **GetNextAsTwinAsync** metoder flera gånger.
Observera att hello frågeobjekt visar flera **nästa\***, beroende på hello deserialisering alternativet krävs av hello fråga, till exempel dubbla eller jobb enhetsobjekt, eller vanligt JSON toobe används när du använder projektioner.

### <a name="nodejs-example"></a>Node.js-exempel
hello frågefunktioner som exponeras av hello [Azure IoT service SDK för Node.js] [ lnk-hub-sdks] i hello **registret** objekt.
Här är ett exempel på en enkel fråga:

```nodejs
var query = registry.createQuery('SELECT * FROM devices', 100);
var onResults = function(err, results) {
    if (err) {
        console.error('Failed toofetch hello results: ' + err.message);
    } else {
        // Do something with hello results
        results.forEach(function(twin) {
            console.log(twin.deviceId);
        });

        if (query.hasMoreResults) {
            query.nextAsTwin(onResults);
        }
    }
};
query.nextAsTwin(onResults);
```

Observera hur hello **frågan** instans-objekt med en storlek (upp too1000) och sedan flera sidor kan hämtas genom att anropa hello **nextAsTwin** metoder flera gånger.
Observera att hello frågeobjekt visar flera **nästa\***, beroende på hello deserialisering alternativet krävs av hello fråga, till exempel dubbla eller jobb enhetsobjekt, eller vanligt JSON toobe används när du använder projektioner.

### <a name="limitations"></a>Begränsningar
> [!IMPORTANT]
> Frågeresultatet kan ha några minuter för fördröjning med avseende toohello senaste värden i enheten twins. Om frågar enskild enhet twins-ID: t är det alltid bättre toouse hello hämta enheten dubbla API, som alltid innehåller hello senaste värden och som har högre throttling-begränsning.

För närvarande jämförelser stöds endast mellan primitiva typer (inga objekt), till exempel `... WHERE properties.desired.config = properties.reported.config` stöds endast om dessa egenskaper har primitiva värden.

## <a name="get-started-with-jobs-queries"></a>Kom igång med jobb-frågor
[Jobb] [ lnk-jobs] ger ett sätt tooexecute åtgärder på uppsättningar av enheter. Varje enhet dubbla innehåller information om hello hello jobb som den är en del i en samling som heter **jobb**.
Logiskt

```json
{
    "deviceId": "myDeviceId",
    "etag": "AAAAAAAAAAc=",
    "tags": {
        ...
    },
    "properties": {
        ...
    },
    "jobs": [
        {
            "deviceId": "myDeviceId",
            "jobId": "myJobId",
            "jobType": "scheduleTwinUpdate",
            "status": "completed",
            "startTimeUtc": "2016-09-29T18:18:52.7418462",
            "endTimeUtc": "2016-09-29T18:20:52.7418462",
            "createdDateTimeUtc": "2016-09-29T18:18:56.7787107Z",
            "lastUpdatedDateTimeUtc": "2016-09-29T18:18:56.8894408Z",
            "outcome": {
                "deviceMethodResponse": null
            }
        },
        ...
    ]
}
```

Den här samlingen är för närvarande frågbar som **devices.jobs** i hello frågespråk för IoT-hubb.

> [!IMPORTANT]
> För närvarande returneras hello jobb egenskap aldrig när du frågar enheten twins (frågor som innehåller 'från enheter-). Endast kan nås direkt med frågor med `FROM devices.jobs`.
>
>

Till exempel tooget alla jobb (tidigare och schemalagda) som påverkar en enstaka enhet, kan du använda hello följande fråga:

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.deviceId = 'myDeviceId'
```

Observera hur den här frågan ger hello enhetsspecifika status (och hello direkta metoden svar) för varje jobb som returneras.
Det är också möjligt toofilter med valfri boolesk villkor för alla objektegenskaper i hello **devices.jobs** samling.
Hej exempelvis följande fråga:

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.deviceId = 'myDeviceId'
    AND devices.jobs.jobType = 'scheduleTwinUpdate'
    AND devices.jobs.status = 'completed'
    AND devices.jobs.createdTimeUtc > '2016-09-01'
```

hämtar alla slutförts enheten dubbla Uppdateringsjobb för enheten **myDeviceId** som skapats efter September 2016.

Det är också möjligt tooretrieve hello per enhet resultat för ett enda jobb.

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.jobId = 'myJobId'
```

### <a name="limitations"></a>Begränsningar
För närvarande en sökning på **devices.jobs** stöder inte:

* Projektioner därför bara `SELECT *` är möjligt.
* Villkor som refererar toohello enheten dubbla i tillägg toojob egenskaper (se hello föregående avsnitt).
* Utföra aggregeringar, till exempel antal, avg, gruppera efter.

## <a name="get-started-with-device-to-cloud-message-routes-query-expressions"></a>Kom igång med enhet till moln meddelandet vägar frågeuttryck

Med hjälp av [enhet till moln vägar][lnk-devguide-messaging-routes], kan du konfigurera IoT-hubb toodispatch enhet till moln meddelanden toodifferent slutpunkter som är baserade på uttryck som utvärderas mot enskilda meddelanden.

hello flödet [villkoret] [ lnk-query-expressions] använder hello samma IoT-hubb-frågespråket i villkoren för dubbla och jobb. Väg villkor utvärderas på hello meddelandehuvuden och brödtext. Din routning frågeuttryck kan innebära endast meddelanderubriker, endast hello meddelandetexten, eller båda meddelande sidhuvuden och meddelande. IoT-hubb förutsätter ett visst schema för hello sidhuvuden och meddelandetexten i ordning tooroute meddelanden och hello följande avsnitt beskrivs vad som krävs för IoT-hubb tooproperly flöde:

### <a name="routing-on-message-headers"></a>Routning på meddelanderubriker

IoT-hubb förutsätter hello följande JSON-representation av meddelandehuvuden för meddelanderoutning:

```json
{
    "$messageId": "",
    "$enqueuedTime": "",
    "$to": "",
    "$expiryTimeUtc": "",
    "$correlationId": "",
    "$userId": "",
    "$ack": "",
    "$connectionDeviceId": "",
    "$connectionDeviceGenerationId": "",
    "$connectionAuthMethod": "",
    "$content-type": "",
    "$content-encoding": "",

    "userProperty1": "",
    "userProperty2": ""
}
```

Meddelandet Systemegenskaper föregås hello `'$'` symbolen.
Egenskaper för användare nås alltid med deras namn. Om ett användarnamn för egenskapen händer toocoincide med en systemegenskap (exempelvis `$to`), hello användaregenskap hämtas med hello `$to` uttryck.
Du kan alltid komma åt hello Systemegenskapen med hakparenteser `{}`: du kan exempelvis använda hello uttryck `{$to}` tooaccess hello Systemegenskapen `to`. Inom hakparenteser egenskapsnamn hämta alltid hello motsvarande Systemegenskapen.

Kom ihåg att egenskapsnamn är skiftlägeskänsliga.

> [!NOTE]
> Alla egenskaper är strängar. Systemegenskaper, enligt beskrivningen i hello [Utvecklarhandbok][lnk-devguide-messaging-format], är inte tillgängliga toouse i frågor.
>

Om du använder till exempel en `messageType` egenskap, kanske du vill tooroute alla telemetri tooone slutpunkt och alla aviseringar tooanother slutpunkt. Du kan skriva hello efter uttrycket tooroute hello telemetri:

```sql
messageType = 'telemetry'
```

Och hello efter uttrycket tooroute hello varningsmeddelanden:

```sql
messageType = 'alert'
```

Booleskt uttryck och funktioner stöds också. Den här funktionen kan du toodistinguish mellan allvarlighetsgrad, till exempel:

```sql
messageType = 'alerts' AND as_number(severity) <= 2
```

Se toohello [uttryck och villkor] [ lnk-query-expressions] avsnittet hello fullständig lista över stöds operatorer och funktioner.

### <a name="routing-on-message-bodies"></a>Routning på meddelandetext

IoT-hubb kan endast vidarebefordra baserat på meddelandetexten innehållet om hello-meddelandetexten är korrekt formaterad JSON kodning i UTF-8, UTF-16 eller UTF-32. Du måste ange hello innehållstypen för hello-meddelande för`application/json` och hello innehåll kodning tooone hello stöds UTF-kodningar i hello meddelandet huvuden tooallow IoT-hubb tooroute hello-meddelande utifrån hello brödtext innehållet. Om någon av hello huvuden anges försöker IoT-hubben inte tooevaluate alla frågeuttryck som involverar hello brödtext mot hello-meddelande. Om meddelandet inte är ett JSON-meddelande eller om hello-meddelande inte anger hello content-type och Innehållskodning, kan du fortfarande använda meddelanderoutning baserat på hello meddelandehuvuden tooroute hello-meddelande.

Du kan använda `$body` i hello fråga uttryck tooroute hello-meddelande. Du kan använda en enkel body-referens, brödtext matrisreferens eller flera body-referenser i hello frågeuttryck. Din frågeuttryck kan också kombinera en brödtext referens med en referens för sidhuvudet. Hello följande är till exempel alla giltig fråga uttryck:

```sql
$body.message.Weather.Location.State = 'WA'
$body.Weather.HistoricalData[0].Month = 'Feb'
$body.Weather.Temperature = 50 AND $body.message.Weather.IsEnabled
length($body.Weather.Location.State) = 2
$body.Weather.Temperature = 50 AND Status = 'Active'
```

## <a name="basics-of-an-iot-hub-query"></a>Grunderna i en fråga för IoT-hubb
Alla IoT-hubb fråga består av en väljer och från-satser och av valfri var och en GROUP BY satser. Varje fråga körs på en mängd JSON-dokument, till exempel enhet twins. hello FROM-satsen anger hello dokumentet samling toobe hävdade på (**enheter** eller **devices.jobs**). Sedan hello filter i hello där tillämpas. Med aggregeringar, grupperas hello resultat i det här steget som anges i hello GROUP BY-sats och en rad genereras för varje grupp som anges i hello SELECT-satsen.

```sql
SELECT <select_list>
FROM <from_specification>
[WHERE <filter_condition>]
[GROUP BY <group_specification>]
```

## <a name="from-clause"></a>FROM-satsen
Hej **från < from_specification >** satsen anta bara två värden: **från enheter**, tooquery enhet twins eller **från devices.jobs**, tooquery jobb per enhet information.

## <a name="where-clause"></a>WHERE-satsen
Hej **där < filter_condition >** -satsen är valfria. Anger ett eller flera villkor att hello JSON-dokument i hello från samlingen måste uppfylla toobe som ingår i hello resultat. JSON-dokument måste utvärderas hello anges villkor för ”true” toobe som ingår i hello resultat.

hello tillåtna villkor som beskrivs i avsnittet [uttryck och villkor][lnk-query-expressions].

## <a name="select-clause"></a>SELECT-satsen
hello SELECT-satsen (**väljer < select_list >**) är obligatoriskt och anger vilka värden hämtas från hello fråga. Den anger hello JSON värden toobe toogenerate nya JSON-objekt.
För varje element i hello filtrerade (och alternativt grupperade) delmängd av hello från samlingen, hello Projektionsfasen genererar ett nytt JSON-objekt med hello värdena som anges i hello SELECT-satsen.

Nedan följer hello grammatik hello SELECT-satsen:

```
SELECT [TOP <max number>] <projection list>

<projection_list> ::=
    '*'
    | <projection_element> AS alias [, <projection_element> AS alias]+

<projection_element> :==
    attribute_name
    | <projection_element> '.' attribute_name
    | <aggregate>

<aggregate> :==
    count()
    | avg(<projection_element>)
    | sum(<projection_element>)
    | min(<projection_element>)
    | max(<projection_element>)
```

där **attribute_name** refererar tooany-egenskapen för hello JSON-dokumentet i hello från samlingen. Några exempel på SELECT-satser kan hittas i hello [komma igång med enheten dubbla frågor] [ lnk-query-getstarted] avsnitt.

För närvarande markeringen satser skiljer sig **Välj \***  stöds endast i sammanställd frågor för enheten twins.

## <a name="group-by-clause"></a>GROUP BY-sats
Hej **GROUP BY < group_specification >** -satsen är ett valfritt steg som kan utföras när hello filter som angetts i hello WHERE-satsen och välj innan hello projektionen som anges i hello. Grupperas dokument baserat på hello-värdet för ett attribut. Dessa grupper är används toogenerate samman värden som anges i hello SELECT-satsen.

Ett exempel på en fråga med GROUP BY är:

```sql
SELECT properties.reported.telemetryConfig.status AS status,
    COUNT() AS numberOfDevices
FROM devices
GROUP BY properties.reported.telemetryConfig.status
```

hello formella syntaxen för GROUP BY är:

```
GROUP BY <group_by_element>
<group_by_element> :==
    attribute_name
    | < group_by_element > '.' attribute_name
```

där **attribute_name** refererar tooany-egenskapen för hello JSON-dokumentet i hello från samlingen.

Hello-GROUP BY-sats stöds för närvarande bara när du frågar enheten twins.

## <a name="expressions-and-conditions"></a>Uttryck och villkor
På en hög nivå, en *uttryck*:

* Utvärderar tooan instans av en JSON-typ (till exempel boolesk, tal, sträng, matris eller objekt) och
* Definieras av manipulera data från hello enheten JSON-dokumentet och konstanter med hjälp av inbyggda operatorer och funktioner.

*Villkor* är uttryck som utvärderas tooa booleskt värde. En konstant som skiljer sig boolesk **true** betraktas som **FALSKT** (inklusive **null**, **Odefinierad**, valfri objektet eller matrisen instans alla sträng och tydligt hello boolesk **FALSKT**).

hello syntaxen för uttryck är:

```
<expression> ::=
    <constant> |
    attribute_name |
    <function_call> |
    <expression> binary_operator <expression> |
    <create_array_expression> |
    '(' <expression> ')'

<function_call> ::=
    <function_name> '(' expression ')'

<constant> ::=
    <undefined_constant>
    | <null_constant>
    | <number_constant>
    | <string_constant>
    | <array_constant>

<undefined_constant> ::= undefined
<null_constant> ::= null
<number_constant> ::= decimal_literal | hexadecimal_literal
<string_constant> ::= string_literal
<array_constant> ::= '[' <constant> [, <constant>]+ ']'
```

Där:

| Symbol | Definition |
| --- | --- |
| attribute_name | En egenskap av hello JSON-dokumentet i hello **FROM** samling. |
| binary_operator | En binär operator som anges i hello [operatörer](#operators) avsnitt. |
| function_name| En funktion som anges i hello [funktioner](#functions) avsnitt. |
| decimal_literal |Ett flyttal uttryckt i decimalformat. |
| hexadecimal_literal |Ett tal anges av hello sträng: 0 x' följt av en sträng med hexadecimala siffror. |
| string_literal |Stränglitteraler är Unicode-strängar som representeras av en följd av noll eller flera Unicode-tecken eller escape-sekvenser. Stränglitteraler omges av enkla citattecken (apostrof: ') eller dubbla citattecken (citattecken ”:). Tillåtna visar: `\'`, `\"`, `\\`, `\uXXXX` för Unicode-tecken som definieras av 4 hexadecimala siffror. |

### <a name="operators"></a>Operatorer
följande operatorer hello stöds:

| Familj | Operatorer |
| --- | --- |
| Aritmetiska |+,-,*,/,% |
| Logiska |OCH, ELLER INTE |
| Jämförelse |=, !=, <, >, <=, >=, <> |

### <a name="functions"></a>Funktioner
När du frågar jobb och twins hello stöds endast är funktionen:

| Funktionen | Beskrivning |
| -------- | ----------- |
| IS_DEFINED(Property) | Returnerar ett booleskt värde som anger om egenskapen hello har tilldelats ett värde (inklusive `null`). |

Hello följande matematiska funktioner stöds i vägar villkor:

| Funktionen | Beskrivning |
| -------- | ----------- |
| ABS(x) | Returnerar hello absolut (positivt) värdet för hello angetts numeriskt uttryck. |
| EXP(x) | Returnerar hello exponentiell värdet för hello angivna numeriska uttrycket (e ^ x). |
| Power(x,y) | Returnerar hello hello anges värdet uttryck toohello angetts power (x ^ y).|
| Square(x) | Returnerar hello rektangulär hello angetts numeriskt värde. |
| CEILING(x) | Returnerar hello minsta heltal som är större än eller lika med för att hello uttryck. |
| FLOOR(x) | Returnerar hello största heltal som är mindre än eller lika toohello angivna numeriska uttrycket. |
| Sign(x) | Returnerar hello positiv (+ 1), noll (0) eller minustecken (-1) för hello angetts numeriskt uttryck.|
| Rot(x) | Returnerar hello rektangulär hello angetts numeriskt värde. |

Hello efter typ kontrollerar och kastar funktioner stöds i vägar villkor:

| Funktionen | Beskrivning |
| -------- | ----------- |
| AS_NUMBER | Konverterar hello Indatasträngen tooa tal. `noop`Om indata är ett tal. `Undefined` om strängen inte representerar ett tal.|
| IS_ARRAY | Returnerar ett booleskt värde som anger om hello typ av hello angivna uttrycket är en matris. |
| IS_BOOL | Returnerar ett booleskt värde som anger om hello typ av hello angivna uttrycket är ett booleskt värde. |
| IS_DEFINED | Returnerar ett booleskt värde som anger om egenskapen hello har tilldelats ett värde. |
| IS_NULL | Returnerar ett booleskt värde som anger om hello typ av hello angivna uttrycket är null. |
| IS_NUMBER | Returnerar ett booleskt värde som anger om hello typ av hello angivna uttrycket är ett tal. |
| IS_OBJECT | Returnerar ett booleskt värde som anger om hello typ av hello angivna uttrycket är ett JSON-objekt. |
| IS_PRIMITIVE | Returnerar ett booleskt värde som anger om hello hello angetts uttrycket är en primitiv (string, Boolean, numeriska eller `null`). |
| IS_STRING | Returnerar ett booleskt värde som anger om hello hello angetts uttryck är en sträng. |

Hello följande strängfunktioner stöds i vägar villkor:

| Funktionen | Beskrivning |
| -------- | ----------- |
| CONCAT(x,...) | Returnerar en sträng som är hello resultat sammanfoga två eller flera strängvärden. |
| LENGTH(x) | Returnerar hello antalet tecken i hello angetts stränguttryck.|
| LOWER(x) | Returnerar ett stränguttryck efter konvertering versal data toolowercase. |
| UPPER(x) | Returnerar ett stränguttryck efter konvertering gemen data toouppercase. |
| SUBSTRING (sträng, start [, längd]) | Returnerar en del av ett stränguttryck som börjar vid hello angetts nollbaserade teckenposition och fortsätter toohello angiven längd eller toohello slutet av hello strängen. |
| INDEX_OF (string, fragment) | Returnerar hello startposition för hello första förekomsten av hello andra stränguttryck inom hello första angivet stränguttryck eller -1 om hello strängen inte hittas.|
| STARTS_WITH (x, y) | Returnerar ett booleskt värde som anger om hello första stränguttryck börjar med hello andra. |
| ENDS_WITH (x, y) | Returnerar ett booleskt värde som anger om hello första stränguttryck slutar med hello andra. |
| CONTAINS(x,y) | Returnerar ett booleskt värde som anger om hello första stränguttryck innehåller hello andra. |

## <a name="next-steps"></a>Nästa steg
Lär dig hur tooexecute frågor i dina appar med hjälp av [Azure IoT SDK][lnk-hub-sdks].

[lnk-query-where]: iot-hub-devguide-query-language.md#where-clause
[lnk-query-expressions]: iot-hub-devguide-query-language.md#expressions-and-conditions
[lnk-query-getstarted]: iot-hub-devguide-query-language.md#get-started-with-device-twin-queries

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-devguide-messaging-routes]: iot-hub-devguide-messages-read-custom.md
[lnk-devguide-messaging-format]: iot-hub-devguide-messages-construct.md
[lnk-devguide-messaging-routes]: ./iot-hub-devguide-messages-read-custom.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
