---
title: "aaaAzure Service Fabric API Management översikt | Microsoft Docs"
description: "Den här artikeln är en introduktion toousing Azure API Management som en gateway tooyour Service Fabric-program."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/22/2017
ms.author: vturecek
ms.openlocfilehash: f01dc570a11e68cd4a2d878abbe6019e209e2f5b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-with-azure-api-management-overview"></a>Service Fabric med översikt över Azure API Management

Molnprogram måste vanligtvis frontend gateway-tooprovide en enda åtkomstpunkt för inkommande trafik för användare, enheter eller andra program. I Service Fabric en gateway kan alla tillståndslösa tjänster som en [ASP.NET Core programmet](service-fabric-reliable-services-communication-aspnetcore.md), eller en annan tjänst som utformats för trafik ingång [Händelsehubbar](https://docs.microsoft.com/azure/event-hubs/), [IoT-hubb](https://docs.microsoft.com/azure/iot-hub/), eller [Azure API Management](https://docs.microsoft.com/azure/api-management/).

Den här artikeln är en introduktion toousing Azure API Management som en gateway tooyour Service Fabric-program. API Management integreras direkt med Service Fabric, så att du toopublish API: er med en omfattande uppsättning regler tooyour backend-Service Fabric routningstjänster. 

## <a name="architecture"></a>Arkitektur
En gemensam Service Fabric-arkitektur använder en enda sida webbprogram som anropar HTTP tooback tjänster som exponerar HTTP APIs. Hej [Service Fabric-igång exempelprogrammet](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started) visar ett exempel på den här arkitekturen.

I detta scenario fungerar en tillståndslös webbtjänst som hello gateway i hello Service Fabric-programmet. Den här metoden kräver toowrite en webbtjänst som kan proxy HTTP-begäranden tooback tjänster, som visas i följande diagram hello:

![Service Fabric med översikt över Azure API Management-topologi][sf-web-app-stateless-gateway]

Eftersom program växer i komplexitet, så hello gateways som måste presentera ett API framför myriad backend-tjänster. Azure API Management är utformad toohandle komplexa API: er med regler för routning, komma åt kontroll, hastighetsbegränsning, övervakning, loggning och svar cachelagring med minimalt arbete från din sida. Azure API Management stöder Service Fabric tjänstidentifiering, partition upplösning och repliken markeringen toointelligently väg begäranden direkt tooback tjänster i Service Fabric så att du inte behöver toowrite tillståndslös API-gatewayen. 

I det här scenariot hello web Användargränssnittet är fortfarande hanteras via en webbtjänst när HTTP-API-anrop hanteras och dirigeras via Azure API Management som visas i följande diagram hello:

![Service Fabric med översikt över Azure API Management-topologi][sf-apim-web-app]

## <a name="application-scenarios"></a>Programscenarier

Tjänster i Service Fabric kan vara tillståndslösa och tillståndskänsliga och de kan partitioneras med någon av tre scheman: singleton, int-64-intervall och namngivna. Tjänsten slutpunktsmappning kräver identifierar en specifik partition för en specifik tjänst-instans. Vid matchning av en slutpunkt av en tjänst, både hello service-Instansnamn (till exempel `fabric:/myapp/myservice`) samt hello specifik partition av hello tjänst måste anges, förutom i hello skiftläget för singleton-partitionen.

Azure API Management kan användas med valfri kombination av tillståndslösa tjänster, tillståndskänsliga tjänster och eventuella partitioneringsschema.

## <a name="send-traffic-tooa-stateless-service"></a>Skicka trafik tooa tillståndslösa tjänsten

I hello enklaste fallet vidarebefordras trafik tooa tillståndslös tjänstinstansen. tooachieve, en API Management-åtgärd innehåller en princip för inkommande bearbetning med ett Service Fabric-backend som mappar tooa specifika tillståndslös tjänstinstansen i hello Service Fabric-serverdel. Begäranden skickas toothat tjänsten skickas tooa slumpmässiga replik av hello tillståndslös tjänstinstansen.

#### <a name="example"></a>Exempel
I följande scenario hello, ett Service Fabric-program innehåller en tillståndslös tjänst med namnet `fabric:/app/fooservice`, som Exponerar en intern HTTP-API. Instansnamn för hello-tjänsten är känt och kan vara hårdkodade direkt i hello API Management inkommande bearbetning princip. 

![Service Fabric med översikt över Azure API Management-topologi][sf-apim-static-stateless]

## <a name="send-traffic-tooa-stateful-service"></a>Skicka trafik tooa tillståndskänslig service

Liknande toohello tillståndslösa tjänsten scenariot trafik kan vidarebefordras tooa tillståndskänslig service-instans. I det här fallet en API Management-åtgärd innehåller en princip för inkommande bearbetning med ett Service Fabric-backend som mappar en begäran tooa specifik partition för en specifik *stateful* tjänstinstansen. hello partition toomap varje begäran toois beräknad via ett lambda-metod som använder indata från hello inkommande HTTP-begäran, till exempel ett värde i hello URL-sökväg. hello princip kan vara konfigurerade toosend begäranden toohello primära repliken eller tooa slumpmässiga replik för läsåtgärder.

#### <a name="example"></a>Exempel

I följande scenario hello, ett Service Fabric-program innehåller en partitionerad tillståndskänslig tjänst med namnet `fabric:/app/userservice` som Exponerar en intern HTTP-API. Instansnamn för hello-tjänsten är känt och kan vara hårdkodade direkt i hello API Management inkommande bearbetning princip.  

hello service är partitionerad med hello Int64 partitionsschema med två partitioner och flera nycklar som omfattar `Int64.MinValue` för`Int64.MaxValue`. hello backend-princip beräknar en partitionsnyckel inom det intervallet genom att konvertera hello `id` värden som anges i hello URL: en begäran sökvägen tooa 64-bitars heltal, även om en algoritm kan vara används här toocompute hello partitionsnyckel. 

![Service Fabric med översikt över Azure API Management-topologi][sf-apim-static-stateful]

## <a name="send-traffic-toomultiple-stateless-services"></a>Skicka trafik toomultiple tillståndslösa tjänster

Du kan definiera en API Management-åtgärd att maps begär toomore än en tjänstinstans i mer avancerade scenarier. I det här fallet innehåller varje åtgärd en princip som mappar förfrågningar tooa specifika tjänstinstans baserat på värden från hello inkommande HTTP-begäran, till exempel hello URL-sökvägen eller fråga sträng och hello gäller tillståndskänsliga tjänster, en partition inom hello tjänstinstans . 

tooachieve detta som en API-hantering åtgärden innehåller en princip för inkommande bearbetning med ett Service Fabric-backend som mappar tooa tillståndslös tjänstinstansen i hello Service Fabric backend-baserat på värden som hämtats från hello inkommande HTTP-begäran. Tjänstinstans för begäranden tooa skickas tooa slumpmässiga replik av hello tjänstinstansen.

#### <a name="example"></a>Exempel

I det här exemplet skapas en ny instans av tillståndslösa tjänsten för varje användare i ett program med ett dynamiskt genererat namn med hello följande formel:
 
 - `fabric:/app/users/<username>`

 Varje tjänst har ett unikt namn, men hello är inte känt direkta eftersom hello tjänster skapas i svaret toouser eller admin indata och därför kan inte vara hårdkodat i APIM principer eller regler för routning. I stället hello namnet på hello service toowhich toosend en begäran skapas i hello backend-principdefinitionen från hello `name` värden som anges i begäran hello URL-sökväg. Exempel:

  - Begäran för`/api/users/foo` är routade tooservice instans`fabric:/app/users/foo`
  - Begäran för`/api/users/bar` är routade tooservice instans`fabric:/app/users/bar`

![Service Fabric med översikt över Azure API Management-topologi][sf-apim-dynamic-stateless]

## <a name="send-traffic-toomultiple-stateful-services"></a>Skicka trafik toomultiple tillståndskänsliga tjänster

Liknande toohello tillståndslösa tjänsten exempelvis begär en API-hantering igen kan du mappa toomore än ett **stateful** service instans, i vilket fall du också behöva tooperform partition lösning för varje tillståndskänslig service instans.

tooachieve detta som en API-hantering åtgärden innehåller en princip för inkommande bearbetning med ett Service Fabric-backend som mappar tooa tillståndskänslig service-instans i hello Service Fabric backend-baserat på värden som hämtats från hello inkommande HTTP-begäran. Toomapping en begäran toospecific tjänstinstans, hello begäran kan dessutom också vara mappade tooa specifik partition inom hello tjänstinstans och eventuellt tooeither hello primära repliken eller en slumpmässig sekundär replik i hello partition.

#### <a name="example"></a>Exempel

I det här exemplet skapas en ny tillståndskänslig service-instans för varje användare av programmet hello med ett dynamiskt genererat namn med hello följande formel:
 
 - `fabric:/app/users/<username>`

 Varje tjänst har ett unikt namn, men hello är inte känt direkta eftersom hello tjänster skapas i svaret toouser eller admin indata och därför kan inte vara hårdkodat i APIM principer eller regler för routning. I stället hello namnet på hello service toowhich toosend en begäran skapas i hello backend-principdefinitionen från hello `name` mervärde hello URL-begäran sökväg. Exempel:

  - Begäran för`/api/users/foo` är routade tooservice instans`fabric:/app/users/foo`
  - Begäran för`/api/users/bar` är routade tooservice instans`fabric:/app/users/bar`

Varje tjänstinstans också är partitionerad med hello Int64 partitionsschema med två partitioner och flera nycklar som omfattar `Int64.MinValue` för`Int64.MaxValue`. hello backend-princip beräknar en partitionsnyckel inom det intervallet genom att konvertera hello `id` värden som anges i hello URL: en begäran sökvägen tooa 64-bitars heltal, även om en algoritm kan vara används här toocompute hello partitionsnyckel. 

![Service Fabric med översikt över Azure API Management-topologi][sf-apim-dynamic-stateful]

## <a name="next-steps"></a>Nästa steg

Följ hello [snabbstartsguiden](service-fabric-api-management-quick-start.md) tooset upp din första Service Fabric-kluster med API-hantering och flödar förfrågningar via API tooyour hanteringstjänster.

<!-- links -->

<!-- pics -->
[sf-apim-web-app]: ./media/service-fabric-api-management-overview/sf-apim-web-app.png
[sf-web-app-stateless-gateway]: ./media/service-fabric-api-management-overview/sf-web-app-stateless-gateway.png
[sf-apim-static-stateless]: ./media/service-fabric-api-management-overview/sf-apim-static-stateless.png
[sf-apim-static-stateful]: ./media/service-fabric-api-management-overview/sf-apim-static-stateful.png
[sf-apim-dynamic-stateless]: ./media/service-fabric-api-management-overview/sf-apim-dynamic-stateless.png
[sf-apim-dynamic-stateful]: ./media/service-fabric-api-management-overview/sf-apim-dynamic-stateful.png