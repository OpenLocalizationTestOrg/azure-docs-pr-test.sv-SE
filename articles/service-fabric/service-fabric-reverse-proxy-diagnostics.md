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
# <a name="monitor-and-diagnose-request-processing-at-hello-reverse-proxy"></a><span data-ttu-id="91029-103">Övervaka och diagnostisera behandling av begäranden på hello omvänd proxy</span><span class="sxs-lookup"><span data-stu-id="91029-103">Monitor and diagnose request processing at hello reverse proxy</span></span>

<span data-ttu-id="91029-104">Från och med hello 5.7 versionen av Service Fabric omvänd proxyhändelser som är tillgängliga för samlingen.</span><span class="sxs-lookup"><span data-stu-id="91029-104">Starting with hello 5.7 release of Service Fabric, reverse proxy events are available for collection.</span></span> <span data-ttu-id="91029-105">hello-händelser finns i två kanaler med endast felhändelser relaterade toorequest bearbetningsfelet på hello omvänd proxy och andra kanalen som innehåller utförlig händelser med poster för både slutförda och misslyckade begäranden.</span><span class="sxs-lookup"><span data-stu-id="91029-105">hello events are available in two channels, one with only error events related toorequest processing failure at hello reverse proxy and second channel containing verbose events with entries for both successful and failed requests.</span></span>

<span data-ttu-id="91029-106">Se för[samla in händelser för omvänd proxy](service-fabric-diagnostics-event-aggregation-wad.md#collect-reverse-proxy-events) tooenable samla in händelser från dessa kanaler i lokala grupper och Azure Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="91029-106">Refer too[Collect reverse proxy events](service-fabric-diagnostics-event-aggregation-wad.md#collect-reverse-proxy-events) tooenable collecting events from these channels in local and Azure Service Fabric clusters.</span></span>

## <a name="troubleshoot-using-diagnostics-logs"></a><span data-ttu-id="91029-107">Felsöka med diagnostik-loggar</span><span class="sxs-lookup"><span data-stu-id="91029-107">Troubleshoot using diagnostics logs</span></span>
<span data-ttu-id="91029-108">Här följer några exempel på hur toointerpret hello vanliga fel loggar att en kan uppstå:</span><span class="sxs-lookup"><span data-stu-id="91029-108">Here are some examples on how toointerpret hello common failure logs that one can encounter:</span></span>

1. <span data-ttu-id="91029-109">Omvänd proxy returnerar Svarets statuskod 504 (Timeout).</span><span class="sxs-lookup"><span data-stu-id="91029-109">Reverse proxy returns response status code 504 (Timeout).</span></span>

    <span data-ttu-id="91029-110">En orsak kan bero på att toohello tjänsten misslyckas tooreply inom hello tidsgränsen för begäran.</span><span class="sxs-lookup"><span data-stu-id="91029-110">One reason could be due toohello service failing tooreply within hello request timeout period.</span></span>
<span data-ttu-id="91029-111">hello händelseloggarna första nedan hello information om hello begäran tas emot på hello omvänd proxy.</span><span class="sxs-lookup"><span data-stu-id="91029-111">hello first event below logs hello details of hello request received at hello reverse proxy.</span></span> <span data-ttu-id="91029-112">hello andra händelsen indikerar att hello-begäran misslyckades vid vidarebefordran tooservice förfallodatum för ”internt fel = ERROR_WINHTTP_TIMEOUT”</span><span class="sxs-lookup"><span data-stu-id="91029-112">hello second event indicates that hello request failed while forwarding tooservice, due too"internal error = ERROR_WINHTTP_TIMEOUT"</span></span> 

    <span data-ttu-id="91029-113">hello nyttolasten innehåller:</span><span class="sxs-lookup"><span data-stu-id="91029-113">hello payload includes:</span></span>

    *  <span data-ttu-id="91029-114">**traceId**: detta GUID kan användas toocorrelate alla hello händelser motsvarande tooa enskild begäran.</span><span class="sxs-lookup"><span data-stu-id="91029-114">**traceId**: This GUID can be used toocorrelate all hello events corresponding tooa single request.</span></span> <span data-ttu-id="91029-115">I hello under två händelser hello traceId = **2f87b722-e254-4ac2-a802-fd315c1a0271**, innebär att de tillhör toohello samma begäran.</span><span class="sxs-lookup"><span data-stu-id="91029-115">In hello below two events, hello traceId = **2f87b722-e254-4ac2-a802-fd315c1a0271**, implying they belong toohello same request.</span></span>
    *  <span data-ttu-id="91029-116">**requestUrl**: hello URL (omvänd proxy-URL) toowhich hello-begäran har skickats.</span><span class="sxs-lookup"><span data-stu-id="91029-116">**requestUrl**: hello URL (Reverse proxy URL) toowhich hello request was sent.</span></span>
    *  <span data-ttu-id="91029-117">**verbet**: HTTP-verbet.</span><span class="sxs-lookup"><span data-stu-id="91029-117">**verb**: HTTP verb.</span></span>
    *  <span data-ttu-id="91029-118">**remoteAddress**: adressen för klienten skickar hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="91029-118">**remoteAddress**: Address of client sending hello request.</span></span>
    *  <span data-ttu-id="91029-119">**resolvedServiceUrl**: endpoint URL toowhich hello inkommande begäran har lösts.</span><span class="sxs-lookup"><span data-stu-id="91029-119">**resolvedServiceUrl**: Service endpoint URL toowhich hello incoming request was resolved.</span></span> 
    *  <span data-ttu-id="91029-120">**Felinformation**: Mer information om hello-fel.</span><span class="sxs-lookup"><span data-stu-id="91029-120">**errorDetails**: Additional information about hello failure.</span></span>

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

2. <span data-ttu-id="91029-121">Omvänd proxy returnerar Svarets statuskod 404 (inget hittas).</span><span class="sxs-lookup"><span data-stu-id="91029-121">Reverse proxy returns response status code 404 (Not Found).</span></span> 
    
    <span data-ttu-id="91029-122">Här är en exempel-händelse där omvänd proxy returnerar 404 eftersom den inte gick toofind hello matchar tjänstslutpunkten.</span><span class="sxs-lookup"><span data-stu-id="91029-122">Here is an example event where reverse proxy returns 404 since it failed toofind hello matching service endpoint.</span></span>
    <span data-ttu-id="91029-123">hello är nyttolast artikeln här:</span><span class="sxs-lookup"><span data-stu-id="91029-123">hello payload  entries of interest here are:</span></span>
    *  <span data-ttu-id="91029-124">**processRequestPhase**: Anger hello fas under bearbetning av begäran när hello felet inträffade, ***TryGetEndpoint*** engångsfaktorautentisering</span><span class="sxs-lookup"><span data-stu-id="91029-124">**processRequestPhase**: Indicates hello phase during request processing when hello failure occurred, ***TryGetEndpoint*** i.e</span></span> <span data-ttu-id="91029-125">När du försöker toofetch hello tjänsten endpoint tooforward till.</span><span class="sxs-lookup"><span data-stu-id="91029-125">while trying toofetch hello service endpoint tooforward to.</span></span> 
    *  <span data-ttu-id="91029-126">**Felinformation**: Visar hello endpoint sökvillkor.</span><span class="sxs-lookup"><span data-stu-id="91029-126">**errorDetails**: Lists hello endpoint search criteria.</span></span> <span data-ttu-id="91029-127">Här kan du se att hello listenerName angetts = **FrontEndListener** hello replik slutpunktslista innehåller endast en lyssnare med hello namnet **OldListener**.</span><span class="sxs-lookup"><span data-stu-id="91029-127">Here you can see that hello listenerName specified = **FrontEndListener** whereas hello replica endpoint list only contains a listener with hello name **OldListener**.</span></span>
    
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
    <span data-ttu-id="91029-128">Ett annat exempel där omvänd proxy kan returnera 404 gick inte att hitta är: ApplicationGateway\Http konfigurationsparameter **SecureOnlyMode** tootrue anges med hello omvänd proxy lyssnar på **HTTPS**, hello replik slutpunkterna är dock osäkra (avlyssnar HTTP).</span><span class="sxs-lookup"><span data-stu-id="91029-128">Another example where reverse proxy can return 404 Not Found is: ApplicationGateway\Http configuration parameter **SecureOnlyMode** is set tootrue with hello reverse proxy listening on **HTTPS**, however all of hello replica endpoints are unsecure (listening on HTTP).</span></span>
    <span data-ttu-id="91029-129">Omvänd proxy returnerar 404 eftersom det går inte att hitta en slutpunkt som lyssnade på HTTPS tooforward hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="91029-129">Reverse proxy returns 404 since it cannot find an endpoint listening on HTTPS tooforward hello request.</span></span> <span data-ttu-id="91029-130">Analysera hello parametrar i hello händelsenyttolast hjälper toonarrow ned hello problemet:</span><span class="sxs-lookup"><span data-stu-id="91029-130">Analyzing hello parameters in hello event payload helps toonarrow down hello issue:</span></span>
    
    ```
        "errorDetails": "SecureOnlyMode = true, gateway protocol = https, listenerName = NewListener, replica endpoint = {\"Endpoints\":{\"OldListener\":\"Http:\/\/localhost:8491\/LocationApp\/\", \"NewListener\":\"Http:\/\/localhost:8492\/LocationApp\/\"}}"
    ```

3. <span data-ttu-id="91029-131">Begäran toohello omvänd proxy misslyckas med ett timeout-fel.</span><span class="sxs-lookup"><span data-stu-id="91029-131">Request toohello reverse proxy fails with a timeout error.</span></span> 
    <span data-ttu-id="91029-132">hello händelseloggarna innehåller en händelse med hello emot Frågedetaljer (visas inte här).</span><span class="sxs-lookup"><span data-stu-id="91029-132">hello event logs contain an event with hello received request details (not shown here).</span></span>
    <span data-ttu-id="91029-133">hello nästa händelse visar att hello tjänsten svarade oväntat med statuskod 404 och omvänd proxy initierar ett nytt lösa.</span><span class="sxs-lookup"><span data-stu-id="91029-133">hello next event shows that hello service responded with a 404 status code and reverse proxy initiates a re-resolve.</span></span> 

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
    <span data-ttu-id="91029-134">När du samlar alla hello-händelser finns tåg av händelser som visar var Lös och vidarebefordra försök.</span><span class="sxs-lookup"><span data-stu-id="91029-134">When collecting all hello events, you see a train of events showing every resolve and forward attempt.</span></span>
    <span data-ttu-id="91029-135">hello sista händelsen i hello serien visar hello bearbetningen misslyckades med en tidsgräns, tillsammans med hello antalet lyckade Lös försök.</span><span class="sxs-lookup"><span data-stu-id="91029-135">hello last event in hello series shows hello request processing has failed with a timeout, along with hello number of successful resolve attempts.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="91029-136">Det rekommenderas tookeep hello utförlig kanal händelseinsamling inaktiverad som standard och aktivera det för felsökning på grundval av behov.</span><span class="sxs-lookup"><span data-stu-id="91029-136">It is recommended tookeep hello  verbose channel event collection disabled by default and enable it for troubleshooting on a need basis.</span></span>

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
    
    <span data-ttu-id="91029-137">Om samlingen är aktiverad för kritiska/felhändelser bara finns en händelse med information om hello timeout och hello antal Lös försök.</span><span class="sxs-lookup"><span data-stu-id="91029-137">If collection is enabled for critical/error events only, you see one event with details about hello timeout and hello number of resolve attempts.</span></span> 
    
    <span data-ttu-id="91029-138">Om tjänsten hello avser toosend 404 status kod tillbaka toohello användare, bör det åtföljas av ett ”X ServiceFabric”-huvud.</span><span class="sxs-lookup"><span data-stu-id="91029-138">If hello service intends toosend a 404 status code back toohello user, it should be accompanied by an "X-ServiceFabric" header.</span></span> <span data-ttu-id="91029-139">När du har korrigerat detta ser omvänd proxy-vidarebefordran hello status kod tillbaka toohello klienten.</span><span class="sxs-lookup"><span data-stu-id="91029-139">After fixing this, you will see that reverse proxy forwards hello status code back toohello client.</span></span>  

4. <span data-ttu-id="91029-140">Fall när hello-klient har kopplat från hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="91029-140">Cases when hello client has disconnected hello request.</span></span>

    <span data-ttu-id="91029-141">hello nedan händelse registreras när omvänd proxy vidarebefordrar hello svar tooclient men hello klienten kopplas:</span><span class="sxs-lookup"><span data-stu-id="91029-141">hello below event is recorded when reverse proxy is forwarding hello response tooclient but hello client disconnects:</span></span>

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
> <span data-ttu-id="91029-142">Bearbetningen av händelser relaterade toowebsocket en begäran loggas inte.</span><span class="sxs-lookup"><span data-stu-id="91029-142">Events related toowebsocket request processing are not currently logged.</span></span> <span data-ttu-id="91029-143">Detta ska läggas till i hello nästa utgåva.</span><span class="sxs-lookup"><span data-stu-id="91029-143">This will be added in hello next release.</span></span>

## <a name="next-steps"></a><span data-ttu-id="91029-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="91029-144">Next steps</span></span>
* <span data-ttu-id="91029-145">[Aggregering av händelse och med Windows Azure-diagnostik](service-fabric-diagnostics-event-aggregation-wad.md) för att aktivera Logginsamling i Azure-kluster.</span><span class="sxs-lookup"><span data-stu-id="91029-145">[Event aggregation and collection using Windows Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md) for enabling log collection in Azure clusters.</span></span>
* <span data-ttu-id="91029-146">tooview Service Fabric-händelser i Visual Studio, se [övervaka och diagnostisera lokalt](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span><span class="sxs-lookup"><span data-stu-id="91029-146">tooview Service Fabric events in Visual Studio, see [Monitor and diagnose locally](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span></span>
* <span data-ttu-id="91029-147">Se för[konfigurera omvänd proxy tooconnect toosecure services](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/ReverseProxySecureSample#configure-reverse-proxy-to-connect-to-secure-services) för Azure Resource Manager mallen exempel tooconfigure säker omvänd proxy med hello olika service certifikat verifieringsalternativ.</span><span class="sxs-lookup"><span data-stu-id="91029-147">Refer too[Configure reverse proxy tooconnect toosecure services](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/ReverseProxySecureSample#configure-reverse-proxy-to-connect-to-secure-services) for Azure Resource Manager template samples tooconfigure secure reverse proxy with hello different service certificate validation options.</span></span>
* <span data-ttu-id="91029-148">Läs [Service Fabric omvänd proxy](service-fabric-reverseproxy.md) toolearn mer.</span><span class="sxs-lookup"><span data-stu-id="91029-148">Read [Service Fabric reverse proxy](service-fabric-reverseproxy.md) toolearn more.</span></span>
