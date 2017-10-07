---
title: aaaUnderstand Azure IoT Hub-enhet twins | Microsoft Docs
description: "Utvecklarhandbok - Använd enheten twins toosynchronize tillstånd och konfiguration av data mellan IoT-hubb och dina enheter"
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 8a3da072-a5bf-46e5-8de4-24cdbb2a03fa
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: elioda
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7dade18665108ed352ff3d18e864dc34f451bbf6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understand-and-use-device-twins-in-iot-hub"></a>Förstå och använda enheten twins i IoT-hubb
## <a name="overview"></a>Översikt
*Enheten twins* är JSON-dokument som lagrar tillstånd enhetsinformation (metadata, konfigurationer och villkor). IoT-hubb kvarstår en enhet dubbla för varje enhet som du ansluter tooIoT hubb. Den här artikeln beskrivs:

* Hej strukturen för hello enheten dubbla: *taggar*, *önskade* och *rapporterade egenskaper*, och
* hello-åtgärder som appar för enheter och -servrar kan utföra på enheten twins.

> [!NOTE]
> Enheten twins är för närvarande enbart tillgänglig från enheter som ansluter tooIoT hubb med hello MQTT-protokollet. Se toohello [MQTT stöd] [ lnk-devguide-mqtt] artikel anvisningar för hur tooconvert befintliga enheten app toouse MQTT.
> 
> 

### <a name="when-toouse"></a>När toouse
Använd enhet twins till:

* Lagra enhetsspecifika metadata i hello molnet. Till exempel hello distribution platsen för en Varuautomat.
* Rapportera aktuell statusinformation, till exempel tillgängliga funktioner och villkor från din enhet. Till exempel en enhet är ansluten tooyour IoT-hubb över mobil- eller WiFi.
* Synkronisera hello tillståndet för tidskrävande arbetsflöden mellan enhetsapp och backend-app. När hello lösning tillbaka anger slutet exempelvis hello ny inbyggd programvara version tooinstall och hello enheten apprapporter hello olika stegen i hello uppdateringsprocessen.
* Fråga din enhetsmetadata, konfiguration eller tillstånd.

Se för[enhet till moln kommunikation vägledning] [ lnk-d2c-guidance] anvisningar om hur du använder rapporterade egenskaper, meddelanden från enhet till moln eller ladda upp filen.
Se för[moln till enhet kommunikation vägledning] [ lnk-c2d-guidance] anvisningar om hur du använder egenskaper, direkt metoder eller moln till enhet meddelanden.

## <a name="device-twins"></a>Enheten twins
Enheten twins lagra enhetsrelaterade information som:

* Enhet och tillbaka slutar använda toosynchronize enheten villkor och konfiguration.
* hello lösningens serverdel kan använda tooquery och rikta långvariga åtgärder.

hello livscykeln för en enhet dubbla länkas toohello motsvarande [enhetsidentitet][lnk-identity]. Enheten twins implicit skapas och tas bort när en ny enhetsidentitet skapas eller tas bort i IoT-hubb.

En enhet dubbla är en JSON-dokument som innehåller:

* **Taggar**. En del av hello JSON-dokument som hello lösningens serverdel kan läsa från och skriva till. Taggar är inte synliga toodevice appar.
* **Egenskaper för Desired**. Används tillsammans med rapporterade egenskaper toosynchronize enhetskonfigurationen eller villkor. Egenskaper kan endast anges av hello lösning tillbaka slutet och kan läsas av hello enhetsapp. Hej enhetsapp kan också få meddelanden i realtid om ändringar i hello önskade egenskaper.
* **Rapporterade egenskaper**. Används tillsammans med egenskaper toosynchronize enhetskonfigurationen eller villkor. Rapporterat egenskaper kan kan endast anges av hello enhetsapp och läsa och efterfrågas av hello lösningens serverdel.

Dessutom hello roten för hello enheten dubbla JSON-dokumentet innehåller hello skrivskyddade egenskaper från hello motsvarande enhetsidentitet lagras i hello [identitetsregistret][lnk-identity].

![][img-twin]

hello som följande exempel visar en enhet dubbla JSON-dokumentet:

        {
            "deviceId": "devA",
            "generationId": "123",
            "status": "enabled",
            "statusReason": "provisioned",
            "connectionState": "connected",
            "connectionStateUpdatedTime": "2015-02-28T16:24:48.789Z",
            "lastActivityTime": "2015-02-30T16:24:48.789Z",

            "tags": {
                "$etag": "123",
                "deploymentLocation": {
                    "building": "43",
                    "floor": "1"
                }
            },
            "properties": {
                "desired": {
                    "telemetryConfig": {
                        "sendFrequency": "5m"
                    },
                    "$metadata" : {...},
                    "$version": 1
                },
                "reported": {
                    "telemetryConfig": {
                        "sendFrequency": "5m",
                        "status": "success"
                    }
                    "batteryLevel": 55,
                    "$metadata" : {...},
                    "$version": 4
                }
            }
        }

I hello rotobjektet hello Systemegenskaper och behållarobjekt för `tags` och båda `reported` och `desired` egenskaper. Hej `properties` behållaren innehåller vissa skrivskyddad element (`$metadata`, `$etag`, och `$version`) beskrivs i hello [enhetens dubbla metadata] [ lnk-twin-metadata] och [ Optimistisk samtidighet] [ lnk-concurrency] avsnitt.

### <a name="reported-property-example"></a>Rapporterat egenskapen exempel
I föregående exempel hello hello enheten dubbla innehåller en `batteryLevel` egenskap som rapporteras av hello enhetsapp. Den här egenskapen gör det möjligt tooquery och fungerar på enheter utifrån hello senaste rapporterade nivå. Andra exempel är hello app reporting enheten enhetsfunktioner eller alternativ för nätverksanslutning.

> [!NOTE]
> Rapporterat egenskaper förenkla scenarier där hello lösningens serverdel är intresserad av hello senast kända värdet för en egenskap. Använd [meddelanden från enhet till moln] [ lnk-d2c] om hello lösningens serverdel måste tooprocess enhetstelemetrin i hello form av tidsstämplad händelser, till exempel tidsserier.

### <a name="desired-property-example"></a>Exempel på önskade egenskapen
I föregående exempel hello hello `telemetryConfig` enheten dubbla önskad och rapporterade egenskaper används av hello lösningens serverdel och hello app toosynchronize hello telemetri enhetskonfiguration för den här enheten. Exempel:

1. hello lösningens serverdel egenskapen hello önskad med hello desired configuration värde. Här är hello-delen av hello dokument med hello önskad egenskapsuppsättning:
   
        ...
        "desired": {
            "telemetryConfig": {
                "sendFrequency": "5m"
            },
            ...
        },
        ...
2. Hej enhetsapp meddelas hello ändra omedelbart om ansluten eller vid hello först återansluta. Hej enhetsapp rapporterar hello uppdateras konfigurationen (eller ett feltillstånd med hello `status` egenskap). Här är hello-delen av hello rapporterade egenskaper:
   
        ...
        "reported": {
            "telemetryConfig": {
                "sendFrequency": "5m",
                "status": "success"
            }
            ...
        }
        ...
3. hello lösningens serverdel kan spåra hello resultaten av hello konfigurationsbegäran mellan många olika enheter, av [frågar] [ lnk-query] twins för enheten.

> [!NOTE]
> hello föregående kodfragment är exempel, optimerade för läsbarhet enkelriktade tooencode en enhetskonfiguration och dess status. IoT-hubb medför inte ett visst schema för hello enheten dubbla önskad och rapporterade egenskaper i hello enheten twins.
> 
> 

Du kan använda twins toosynchronize långvariga åtgärder, t.ex uppdateringar av inbyggd programvara. Mer information om hur toouse egenskaper toosynchronize och spåra långvarig åtgärd på enheter, se [Använd önskad egenskaper tooconfigure enheter][lnk-twin-properties].

## <a name="back-end-operations"></a>Backend-åtgärder
hello lösningens serverdel fungerar på hello enheten dubbla med hello följande atomiska åtgärder som exponeras via HTTP:

1. **Hämta enheten dubbla med id**. Den här åtgärden returnerar hello enheten dubbla dokument, inklusive taggar och önskas, rapporteras och Systemegenskaper.
2. **Delvis uppdatera enheten dubbla**. Den här åtgärden kan hello taggar för hello lösning serverdel toopartially eller önskade egenskaper i en delad enhet. hello deluppdatering uttrycks som ett JSON-dokument som läggs till eller uppdaterar en egenskap. Egenskaper som angetts för`null` tas bort. hello följande exempel skapas en ny önskad egenskap med värdet `{"newProperty": "newValue"}`, skriver över befintliga hello-värdet för `existingProperty` med `"otherNewValue"`, och tar bort `otherOldProperty`. Några andra ändringar görs tooexisting önskad egenskaperna eller taggarna:
   
        {
            "properties": {
                "desired": {
                    "newProperty": {
                        "nestedProperty": "newValue"
                    },
                    "existingProperty": "otherNewValue",
                    "otherOldProperty": null
                }
            }
        }
3. **Ersätt egenskaper**. Den här åtgärden aktiverar hello lösning serverdel toocompletely skriva över alla befintliga önskade egenskaper och ersätta ett nytt JSON-dokument för `properties/desired`.
4. **Ersätt taggar**. Den här åtgärden aktiverar hello lösning serverdel toocompletely skriva över alla befintliga taggar och ersätta ett nytt JSON-dokument för `tags`.
5. **Ta emot meddelanden med dubbla**. Den här åtgärden tillåter hello lösning serverdel toobe meddelad när hello dubbla ändras. toodo så IoT-lösningen behöver toocreate en väg och tooset hello datakällan lika för*twinChangeEvents*. Inga dubbla meddelanden skickas som standard, som är det inför finns ingen sådan vägar. Om ändringshastigheten hello är för hög eller av andra orsaker, till exempel internt fel hello IoT-hubb kan skicka endast ett meddelande som innehåller alla ändringar. Så om ditt program måste tillförlitliga granskning och loggning av alla mellanliggande tillstånd, sedan fortfarande rekommenderas att du använder D2C meddelanden. hello dubbla meddelandet innehåller egenskaperna och innehållet.

    - Egenskaper

    | Namn | Värde |
    | --- | --- |
    $content-typ | application/json |
    $iothub-enqueuedtime |  Tid när hello-meddelande skickades |
    $iothub-meddelande-källa | twinChangeEvents |
    $content-kodning | UTF-8 |
    deviceId | ID för hello-enhet |
    hubName | Namnet på IoT-hubb |
    operationTimestamp | [ISO8601] tidsstämpeln för åtgärden |
    iothub-meddelande-schema | deviceLifecycleNotification |
    opType | ”replaceTwin” eller ”updateTwin” |

    Meddelandet Systemegenskaper föregås hello `'$'` symbolen.

    - Innehåll
        
    Det här avsnittet innehåller alla hello dubbla ändringar i en JSON-format. Hello samma format används som en korrigering, med skillnaden hello som den kan innehålla alla dubbla avsnitt: taggar, properties.reported, properties.desired och att den innehåller hello ”$metadata” element. Exempel:
    ```
    {
        "properties": {
            "desired": {
                "$metadata": {
                    "$lastUpdated": "2016-02-30T16:24:48.789Z"
                },
                "$version": 1
            },
            "reported": {
                "$metadata": {
                    "$lastUpdated": "2016-02-30T16:24:48.789Z"
                },
                "$version": 1
            }
        }
    }
    ``` 

Alla hello stöd för föregående [Optimistisk samtidighet] [ lnk-concurrency] och kräver hello **ServiceConnect** behörighet, som definierats i hello [säkerhet ] [ lnk-security] artikel.

Dessutom tillbaka toothese åtgärder, hello lösning slutet kan:

* Fråga hello enheten twins med hello SQL-liknande [IoT-hubb frågespråket][lnk-query].
* Utföra åtgärder på stora mängder enheten twins med [jobb][lnk-jobs].

## <a name="device-operations"></a>Åtgärder för enhet
hello enhetsapp fungerar på hello enheten dubbla med hello följande atomiska åtgärder:

1. **Hämta enheten dubbla**. Den här åtgärden returnerar hello enheten dubbla dokumentet (inklusive taggar och önskas, rapporteras och Systemegenskaper) för hello ansluten enhet.
2. **Uppdatera delvis rapporterade egenskaper**. Den här åtgärden aktiverar hello deluppdatering av hello rapporterade hello anslutna enhetens egenskaper. Den här åtgärden använder hello uppdatera samma JSON-format som hello lösning tillbaka användningsområden för en deluppdatering av egenskaper.
3. **Se egenskaper**. hello anslutna enheter kan välja toobe meddelanden om uppdateringar toohello önskade egenskaper innan de inträffar. hello enheten tar emot hello samma form av uppdatering (eller delvis ersättning) som körs av hello lösningens serverdel.

Alla hello föregående operationer kräver hello **DeviceConnect** behörighet, som definierats i hello [säkerhet] [ lnk-security] artikel.

Hej [Azure IoT-enhet SDK] [ lnk-sdks] gör det enkelt toouse hello föregående åtgärder från många språk och plattformar. Mer information om hello detaljer för IoT-hubb primitiver för synkronisering av egenskaper finns i [enheten återanslutning flödet][lnk-reconnection].

> [!NOTE]
> Enheten twins är för närvarande enbart tillgänglig från enheter som ansluter tooIoT hubb med hello MQTT-protokollet.
> 
> 

## <a name="reference-topics"></a>Referensinformation:
hello ger följande Referensinformation dig mer om styra åtkomst tooyour IoT-hubb.

## <a name="tags-and-properties-format"></a>Taggar och egenskaper format
Taggar, önskade och rapporterade egenskaper är JSON-objekt med hello följande begränsningar:

* Alla nycklar i JSON-objekt är skiftlägeskänsliga 64 byte UTF-8, UNICODE-strängar. Tillåtna tecken undanta Unicode-kontrolltecken (segment C0 och C1) och `'.'`, `' '`, och `'$'`.
* Alla värden i JSON-objekt kan vara av följande typer av JSON hello: boolean, nummer, sträng, objekt. Matriser är inte tillåtna.
* JSON-objekt i taggar, önskade och rapporterade egenskaper kan ha högst 5. Exempelvis är hello följande objekt giltiga:

        {
            ...
            "tags": {
                "one": {
                    "two": {
                        "three": {
                            "four": {
                                "five": {
                                    "property": "value"
                                }
                            }
                        }
                    }
                }
            },
            ...
        }

* Alla strängvärden kan innehålla högst 512 byte i längd.

## <a name="device-twin-size"></a>Enheten dubbla storlek
IoT-hubb tillämpar en begränsning på 8KB storleken på hello värden för `tags`, `properties/desired`, och `properties/reported`, exklusive skrivskyddade element.
hello storlek beräknas genom att räkna alla tecken utom UNICODE styra tecken (segment C0 och C1) och blanksteg `' '` när den visas utanför en strängkonstant.
IoT-hubb avvisar alla åtgärder som skulle öka hello storleken på dessa dokument över hello gräns med ett fel.

## <a name="device-twin-metadata"></a>Enhetens dubbla metadata
IoT-hubb underhåller hello tidsstämpel hello senaste uppdateringen för varje JSON-objekt i enheten dubbla önskad och rapporterade egenskaper. hello tidsstämplar i UTC och kodats i hello [ISO8601] format `YYYY-MM-DDTHH:MM:SS.mmmZ`.
Exempel:

        {
            ...
            "properties": {
                "desired": {
                    "telemetryConfig": {
                        "sendFrequency": "5m"
                    },
                    "$metadata": {
                        "telemetryConfig": {
                            "sendFrequency": {
                                "$lastUpdated": "2016-03-30T16:24:48.789Z"
                            },
                            "$lastUpdated": "2016-03-30T16:24:48.789Z"
                        },
                        "$lastUpdated": "2016-03-30T16:24:48.789Z"
                    },
                    "$version": 23
                },
                "reported": {
                    "telemetryConfig": {
                        "sendFrequency": "5m",
                        "status": "success"
                    }
                    "batteryLevel": "55%",
                    "$metadata": {
                        "telemetryConfig": {
                            "sendFrequency": "5m",
                            "status": {
                                "$lastUpdated": "2016-03-31T16:35:48.789Z"
                            },
                            "$lastUpdated": "2016-03-31T16:35:48.789Z"
                        }
                        "batteryLevel": {
                            "$lastUpdated": "2016-04-01T16:35:48.789Z"
                        },
                        "$lastUpdated": "2016-04-01T16:24:48.789Z"
                    },
                    "$version": 123
                }
            }
            ...
        }

Den här informationen sparas på varje nivå (inte bara hello löv av hello JSON-strukturen) toopreserve uppdateringar som tar bort objektnycklar.

## <a name="optimistic-concurrency"></a>Optimistisk samtidighet
Taggar, önskad och rapporterade egenskaper alla stöd för Optimistisk samtidighet.
Taggar har en ETag enligt [RFC7232], som representerar hello taggen JSON-representation. Du kan använda ETags i villkorlig uppdateringsåtgärder från hello lösning serverdel tooensure konsekvenskontroll.

Enheten dubbla önskad och rapporterade egenskaper har inte ETags, men har en `$version` värdet som garanterat toobe inkrementell. På liknande sätt kan tooan ETag hello version användas av hello uppdaterar part tooenforce konsekvenskontroll av uppdateringar. Till exempel en enhetsapp för en rapporterade egenskap eller hello lösningens serverdel för en önskad egenskap.

Versioner är också användbart när en observing agent (till exempel hello enhetsapp sett hello önskade egenskaper) måste stämma lopp mellan hello resultatet av en hämtningen och ett uppdateringsmeddelande. Hej avsnittet [enheten återanslutning flödet] [ lnk-reconnection] innehåller mer information.

## <a name="device-reconnection-flow"></a>Enheten återanslutning flöde
IoT-hubb bevaras inte egenskaper uppdateringsmeddelanden för frånkopplade enheter. Följer att en enhet som ansluter måste hämta hello fullständig egenskaper dokument i tillägget toosubscribing för meddelanden om uppdateringar. Angivna hello möjligheten att lopp mellan uppdateringsmeddelanden och fullständig hämtning säkerställas hello följande flödet:

1. Enhetsapp ansluter tooan IoT-hubb.
2. Enhetsapp prenumererar för önskade egenskaper meddelanden om uppdateringar.
3. Enheten appen hämtar hello hela dokumentet för egenskaper.

Hej enhetsapp kan ignorera alla meddelanden med `$version` mindre än eller lika med hello version av hello fullständig hämtade dokumentet. Den här metoden är möjligt eftersom IoT-hubb garanterar att det alltid öka versioner.

> [!NOTE]
> Den här logiken har redan implementerats i hello [Azure IoT-enhet SDK][lnk-sdks]. Den här beskrivningen är användbart om hello enheten appen kan inte använda någon av Azure IoT-enhet SDK: er och måste programmet hello MQTT gränssnittet direkt.
> 
> 

## <a name="additional-reference-material"></a>Ytterligare referensmaterialet
Andra referensavsnitten i hello IoT-hubb Utvecklarhandbok inkluderar:

* Hej [IoT-hubbslutpunkter] [ lnk-endpoints] artikeln hello olika slutpunkter som varje IoT-hubb visar för körning och hanteringsåtgärder.
* Hej [begränsning och kvoter] [ lnk-quotas] artikeln hello kvoter som gäller toohello IoT-hubb-tjänsten och hello bandbreddsbegränsning beteende tooexpect när du använder hello-tjänsten.
* Hej [Azure IoT-enheten och tjänsten SDK] [ lnk-sdks] artikeln visar hello olika språk SDK: du kan använda när du utvecklar appar för både enheten och tjänsten som interagerar med IoT-hubben.
* Hej [IoT-hubb frågespråk för enheten twins, jobb och meddelanderoutning] [ lnk-query] artikeln hello IoT-hubb frågespråk som du kan använda tooretrieve information från IoT-hubb om enheten twins och jobb .
* Hej [IoT-hubb MQTT stöd] [ lnk-devguide-mqtt] artikeln innehåller mer information om stöd för IoT-hubb för hello MQTT protokoll.

## <a name="next-steps"></a>Nästa steg
Nu du har lärt dig om enheten twins, kanske du är intresserad av i följande avsnitt i IoT-hubb developer hello:

* [Anropa en metod som är direkt på en enhet][lnk-methods]
* [Schema-jobb på flera enheter][lnk-jobs]

Om du vill tootry titt på hello begrepp som beskrivs i den här artikeln får du är intresserad av hello följande IoT-hubb Självstudier:

* [Hur toouse hello enheten dubbla][lnk-twin-tutorial]
* [Hur toouse enheten dubbla egenskaper][lnk-twin-properties]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-identity]: iot-hub-devguide-identity-registry.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-security]: iot-hub-devguide-security.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md

[ISO8601]: https://en.wikipedia.org/wiki/ISO_8601
[RFC7232]: https://tools.ietf.org/html/rfc7232
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-twin-metadata]: iot-hub-devguide-device-twins.md#device-twin-metadata
[lnk-concurrency]: iot-hub-devguide-device-twins.md#optimistic-concurrency
[lnk-reconnection]: iot-hub-devguide-device-twins.md#device-reconnection-flow

[img-twin]: media/iot-hub-devguide-device-twins/twin.png
