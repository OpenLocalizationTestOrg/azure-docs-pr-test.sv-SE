---
title: "aaaAzure Service Fabric omvänd proxy diagnostik | Microsoft Docs"
description: "Lär dig hur toomonitor och diagnostisera behandling av begäranden på hello omvänd proxy."
services: service-fabric
documentationcenter: .net
author: kavyako
manager: vipulm
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 08/08/2017
ms.author: kavyako
ms.openlocfilehash: 9687b9688dc26ba619cbdfab1b1f49a3035345c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-diagnose-request-processing-at-hello-reverse-proxy"></a>Övervaka och diagnostisera behandling av begäranden på hello omvänd proxy

Från och med hello 5.7 versionen av Service Fabric omvänd proxyhändelser som är tillgängliga för samlingen. hello-händelser finns i två kanaler med endast felhändelser relaterade toorequest bearbetningsfelet på hello omvänd proxy och andra kanalen som innehåller utförlig händelser med poster för både slutförda och misslyckade begäranden.

Se för[samla in händelser för omvänd proxy](service-fabric-diagnostics-event-aggregation-wad.md#collect-reverse-proxy-events) tooenable samla in händelser från dessa kanaler i lokala grupper och Azure Service Fabric-kluster.

## <a name="troubleshoot-using-diagnostics-logs"></a>Felsöka med diagnostik-loggar
Här följer några exempel på hur toointerpret hello vanliga fel loggar att en kan uppstå:

1. Omvänd proxy returnerar Svarets statuskod 504 (Timeout).

    En orsak kan bero på att toohello tjänsten misslyckas tooreply inom hello tidsgränsen för begäran.
hello händelseloggarna första nedan hello information om hello begäran tas emot på hello omvänd proxy. hello andra händelsen indikerar att hello-begäran misslyckades vid vidarebefordran tooservice förfallodatum för ”internt fel = ERROR_WINHTTP_TIMEOUT” 

    hello nyttolasten innehåller:

    *  **traceId**: detta GUID kan användas toocorrelate alla hello händelser motsvarande tooa enskild begäran. I hello under två händelser hello traceId = **2f87b722-e254-4ac2-a802-fd315c1a0271**, innebär att de tillhör toohello samma begäran.
    *  **requestUrl**: hello URL (omvänd proxy-URL) toowhich hello-begäran har skickats.
    *  **verbet**: HTTP-verbet.
    *  **remoteAddress**: adressen för klienten skickar hello-begäran.
    *  **resolvedServiceUrl**: endpoint URL toowhich hello inkommande begäran har lösts. 
    *  **Felinformation**: Mer information om hello-fel.

    ```
    {
      "Timestamp": "2017-07-20T15:57:59.9871163-07:00",
      "ProviderName": "Microsoft-ServiceFabric",
      "Id": 51477,
      "Message": "2f87b722-e254-4ac2-a802-fd315c1a0271 Request url = https://localhost:19081/LocationApp/LocationFEService?zipcode=98052, verb = GET, remote (client) address = ::1, resolved service url = Https://localhost:8491/LocationApp/?zipcode=98052, request processing start time =     15:58:00.074114 (745,608.196 MSec) ",
      "ProcessId": 57696,
      "Level": "Informational",
      "Keywords": "0x1000000000000021",
      "EventName": "ReverseProxy",
      "ActivityID": null,
      "RelatedActivityID": null,
      "Payload": {
        "traceId": "2f87b722-e254-4ac2-a802-fd315c1a0271",
        "requestUrl": "https://localhost:19081/LocationApp/LocationFEService?zipcode=98052",
        "verb": "GET",
        "remoteAddress": "::1",
        "resolvedServiceUrl": "Https://localhost:8491/LocationApp/?zipcode=98052",
        "requestStartTime": "2017-07-20T15:58:00.0741142-07:00"
      }
    }

    {
      "Timestamp": "2017-07-20T16:00:01.3173605-07:00",
      ...
      "Message": "2f87b722-e254-4ac2-a802-fd315c1a0271 Error while forwarding request tooservice: response status code = 504, description = Reverse proxy Timeout, phase = FinishSendRequest, internal error = ERROR_WINHTTP_TIMEOUT ",
      ...
      "Payload": {
        "traceId": "2f87b722-e254-4ac2-a802-fd315c1a0271",
        "statusCode": 504,
        "description": "Reverse Proxy Timeout",
        "sendRequestPhase": "FinishSendRequest",
        "errorDetails": "internal error = ERROR_WINHTTP_TIMEOUT"
      }
    }
    ```

2. Omvänd proxy returnerar Svarets statuskod 404 (inget hittas). 
    
    Här är en exempel-händelse där omvänd proxy returnerar 404 eftersom den inte gick toofind hello matchar tjänstslutpunkten.
    hello är nyttolast artikeln här:
    *  **processRequestPhase**: Anger hello fas under bearbetning av begäran när hello felet inträffade, ***TryGetEndpoint*** engångsfaktorautentisering När du försöker toofetch hello tjänsten endpoint tooforward till. 
    *  **Felinformation**: Visar hello endpoint sökvillkor. Här kan du se att hello listenerName angetts = **FrontEndListener** hello replik slutpunktslista innehåller endast en lyssnare med hello namnet **OldListener**.
    
    ```
    {
      ...
      "Message": "c1cca3b7-f85d-4fef-a162-88af23604343 Error while processing request, cannot forward tooservice: request url = https://localhost:19081/LocationApp/LocationFEService?ListenerName=FrontEndListener&zipcode=98052, verb = GET, remote (client) address = ::1, request processing start time = 16:43:02.686271 (3,448,220.353 MSec), error = FABRIC_E_ENDPOINT_NOT_FOUND, message = , phase = TryGetEndoint, SecureOnlyMode = false, gateway protocol = https, listenerName = FrontEndListener, replica endpoint = {\"Endpoints\":{\"\":\"Https:\/\/localhost:8491\/LocationApp\/\"}} ",
      "ProcessId": 57696,
      "Level": "Warning",
      "EventName": "ReverseProxy",
      "Payload": {
        "traceId": "c1cca3b7-f85d-4fef-a162-88af23604343",
        "requestUrl": "https://localhost:19081/LocationApp/LocationFEService?ListenerName=NewListener&zipcode=98052",
        ...
        "processRequestPhase": "TryGetEndoint",
        "errorDetails": "SecureOnlyMode = false, gateway protocol = https, listenerName = FrontEndListener, replica endpoint = {\"Endpoints\":{\"OldListener\":\"Https:\/\/localhost:8491\/LocationApp\/\"}}"
      }
    }
    ```
    Ett annat exempel där omvänd proxy kan returnera 404 gick inte att hitta är: ApplicationGateway\Http konfigurationsparameter **SecureOnlyMode** tootrue anges med hello omvänd proxy lyssnar på **HTTPS**, hello replik slutpunkterna är dock osäkra (avlyssnar HTTP).
    Omvänd proxy returnerar 404 eftersom det går inte att hitta en slutpunkt som lyssnade på HTTPS tooforward hello-begäran. Analysera hello parametrar i hello händelsenyttolast hjälper toonarrow ned hello problemet:
    
    ```
        "errorDetails": "SecureOnlyMode = true, gateway protocol = https, listenerName = NewListener, replica endpoint = {\"Endpoints\":{\"OldListener\":\"Http:\/\/localhost:8491\/LocationApp\/\", \"NewListener\":\"Http:\/\/localhost:8492\/LocationApp\/\"}}"
    ```

3. Begäran toohello omvänd proxy misslyckas med ett timeout-fel. 
    hello händelseloggarna innehåller en händelse med hello emot Frågedetaljer (visas inte här).
    hello nästa händelse visar att hello tjänsten svarade oväntat med statuskod 404 och omvänd proxy initierar ett nytt lösa. 

    ```
    {
      ...
      "Message": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e Request tooservice returned: status code = 404, status description = , Reresolving ",
      "Payload": {
        "traceId": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e",
        "statusCode": 404,
        "statusDescription": ""
      }
    }
    {
      ...
      "Message": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e Re-resolved service url = Https://localhost:8491/LocationApp/?zipcode=98052 ",
      "Payload": {
        "traceId": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e",
        "requestUrl": "Https://localhost:8491/LocationApp/?zipcode=98052"
      }
    }
    ```
    När du samlar alla hello-händelser finns tåg av händelser som visar var Lös och vidarebefordra försök.
    hello sista händelsen i hello serien visar hello bearbetningen misslyckades med en tidsgräns, tillsammans med hello antalet lyckade Lös försök.
    
    > [!NOTE]
    > Det rekommenderas tookeep hello utförlig kanal händelseinsamling inaktiverad som standard och aktivera det för felsökning på grundval av behov.

    ```
    {
      ...
      "Message": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e Error while processing request: number of successful resolve attempts = 12, error = FABRIC_E_TIMEOUT, message = , phase = ResolveServicePartition,  ",
      "EventName": "ReverseProxy",
      ...
      "Payload": {
        "traceId": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e",
        "resolveCount": 12,
        "errorval": -2147017729,
        "errorMessage": "",
        "processRequestPhase": "ResolveServicePartition",
        "errorDetails": ""
      }
    }
    ```
    
    Om samlingen är aktiverad för kritiska/felhändelser bara finns en händelse med information om hello timeout och hello antal Lös försök. 
    
    Om tjänsten hello avser toosend 404 status kod tillbaka toohello användare, bör det åtföljas av ett ”X ServiceFabric”-huvud. När du har korrigerat detta ser omvänd proxy-vidarebefordran hello status kod tillbaka toohello klienten.  

4. Fall när hello-klient har kopplat från hello-begäran.

    hello nedan händelse registreras när omvänd proxy vidarebefordrar hello svar tooclient men hello klienten kopplas:

    ```
    {
      ...
      "Message": "6e2571a3-14a8-4fc7-93bb-c202c23b50b8 Unable toosend response tooclient: phase = SendResponseHeaders, error = -805306367, internal error = ERROR_SUCCESS ",
      "ProcessId": 57696,
      "Level": "Warning",
      ...
      "EventName": "ReverseProxy",
      "Payload": {
        "traceId": "6e2571a3-14a8-4fc7-93bb-c202c23b50b8",
        "sendResponsePhase": "SendResponseHeaders",
        "errorval": -805306367,
        "winHttpError": "ERROR_SUCCESS"
      }
    }
    ```

> [!NOTE]
> Bearbetningen av händelser relaterade toowebsocket en begäran loggas inte. Detta ska läggas till i hello nästa utgåva.

## <a name="next-steps"></a>Nästa steg
* [Aggregering av händelse och med Windows Azure-diagnostik](service-fabric-diagnostics-event-aggregation-wad.md) för att aktivera Logginsamling i Azure-kluster.
* tooview Service Fabric-händelser i Visual Studio, se [övervaka och diagnostisera lokalt](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).
* Se för[konfigurera omvänd proxy tooconnect toosecure services](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/ReverseProxySecureSample#configure-reverse-proxy-to-connect-to-secure-services) för Azure Resource Manager mallen exempel tooconfigure säker omvänd proxy med hello olika service certifikat verifieringsalternativ.
* Läs [Service Fabric omvänd proxy](service-fabric-reverseproxy.md) toolearn mer.
