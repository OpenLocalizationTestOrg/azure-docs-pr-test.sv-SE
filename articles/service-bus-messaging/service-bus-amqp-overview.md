---
title: aaaOverview AMQP 1.0 i Azure Service Bus | Microsoft Docs
description: "Lär dig mer om hur du använder hello avancerade Message Queuing-protokollet (AMQP) 1.0 i Azure."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 0e8d19cc-de36-478e-84ae-e089bbc2d515
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/27/2017
ms.author: sethm
ms.openlocfilehash: c35a20c50dd24845d9a4c7a0251e8240848a6f4b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="amqp-10-support-in-service-bus"></a>AMQP 1.0-stöd i Service Bus
Både hello Azure Service Bus tjänst i molnet och lokala [Service Bus för Windows Server (Service Bus 1.1)](https://msdn.microsoft.com/library/dn282144.aspx) stöd hello avancerade Message Queueing Protocol (AMQP) 1.0. AMQP aktiverar du toobuild plattformsoberoende, hybridprogram med ett öppet standardprotokoll. Du kan skapa program med hjälp av komponenter som är byggda med olika språk och ramverk och som körs på olika operativsystem. Alla dessa komponenter kan ansluta tooService Bus och sömlöst exchange strukturerade meddelanden effektivt och fullständig återgivning.

## <a name="introduction-what-is-amqp-10-and-why-is-it-important"></a>Introduktion: Vad är AMQP 1.0 och varför är det viktigt?
Meddelande-inriktade mellanprodukter har tidigare använt egna protokoll för kommunikation mellan program och mäklare. Det innebär att när du har valt en viss leverantör meddelanden broker, måste du använda den leverantör bibliotek tooconnect din klient program toothat broker. Detta resulterar i en grad av beroende av tillverkaren, eftersom portera en annan produkt tooa för programmet kräver kodändringar i alla hello anslutna program. 

Dessutom är meddelanden mäklare från olika leverantörer ansluta svårt. Detta kräver normalt på programnivå datacenterbryggning toomove meddelanden från ett system tooanother och tootranslate mellan sina egna meddelandeformat. Det här är ett vanligt krav. till exempel när du måste ange en ny enhetliga gränssnittet tooolder olika system eller integrera IT-system efter en fusion.

hello industrin är en fast flytta verksamhet. nya programmeringsspråk och programramverk introduceras i en ibland bewildering takt. På liknande sätt hello kraven i IT-system utvecklas över tid och utvecklare vill tootake nytta av hello senaste funktioner. Men stöder ibland hello valda meddelanden leverantören inte dessa plattformar. Eftersom meddelanden protokollen är skyddad, är det inte möjligt att andra tooprovide bibliotek för dessa nya plattformar. Därför måste du använda metoderna, till exempel uppbyggnad gateways och bryggor som gör att du toocontinue toouse hello meddelanden.

hello utveckling av hello avancerade Message Queuing-protokollet (AMQP) 1.0 har motiveras av dessa problem. Den kommer från JP Morgan Chase som är tungt användare av meddelande-inriktade mellanprogram som mest ekonomiska tjänster företag. hello målet är enkel: toocreate en öppen standard meddelanden protokoll som gör det möjligt toobuild meddelande-baserade program med hjälp av komponenter som skapats med olika språk och ramverk operativsystem, alla med bäst för RAS-komponenter från en antal leverantörer.

## <a name="amqp-10-technical-features"></a>Tekniska AMQP 1.0-funktioner
AMQP 1.0 är ett effektivt, tillförlitlig och överföring nivå meddelanden protokoll som du kan använda toobuild robust, plattformsoberoende, e-postprogram. hello-protokollet har ett enkelt mål: toodefine hello säkerhetsnivån hello säker, tillförlitlig och effektiv överföring av meddelanden mellan två parter. hälsningsmeddelande själva kodas med hjälp av en bärbar data representation som aktiverar heterogena avsändarna och mottagarna tooexchange strukturerad business meddelanden vid fullständig återgivning. hello nedan följer en sammanfattning av de viktigaste funktionerna för hello:

* **Effektiv**: AMQP 1.0 är ett protokoll för anslutningar som använder en binär kodning för hello protokollet instruktioner och hello business-meddelanden skickas via den. Den innehåller avancerade flödeskontroll scheman toomaximize hello användning av hello nätverks- och hello anslutna komponenter. Namn, hello-protokollet var utformad toostrike en avvägning mellan effektivitet, flexibilitet och samverkan.
* **Tillförlitliga**: hello AMQP 1.0-protokollet gör att meddelanden toobe utväxlas med olika tillförlitlighet garantier från fire-och-glömmer tooreliable exakt – en gång bekräftade leverans.
* **Flexibla**: AMQP 1.0 är ett flexibelt protokoll som kan använda toosupport olika topologier. hello kan samma protokoll användas för kommunikation med klient-till-klient och klient-till-broker broker Service broker.
* **Service Broker-modell oberoende**: hello AMQP 1.0-specifikationen inte gör några krav hello meddelanden modellen som en koordinator. Det innebär att det är möjligt tooeasily lägga till AMQP 1.0 stöd tooexisting asynkrona meddelandeköer.

## <a name="amqp-10-is-a-standard-with-a-capital-s"></a>AMQP 1.0 är en Standard (med en versal ')
AMQP 1.0 är en internationell standard godkänd av ISO och IEC som ISO/IEC 19464:2014.

AMQP 1.0 har varit under utveckling sedan 2008 genom en core företagsgrupp fler än 20, både teknik leverantörer och slutanvändarens företag. Under den tiden kan företag som användaren har bidragit sina verkliga affärsbehov och hello teknikleverantörer har utvecklats hello protokollet toomeet dessa krav. Under hela processen hello har leverantörer deltagit i arbetsgrupper som de samarbetat toovalidate hello samverkan mellan sina implementeringar.

I oktober 2011 hello utvecklingsarbete övergick tooa tekniska kommittén inom hello organisation för hello framflyttning för Structured Information Standards (OASIS) och hello OASIS AMQP 1.0 Standard gavs ut i oktober 2012. hello följande företag har deltagit i hello tekniska kommittén under hello utvecklingen av hello som standard:

* **Teknikleverantörer**: Axway programvara, Huawei tekniker, IIT programvara, INETCO system, Kaazing, Microsoft, Mitre Corporation, Primeton tekniker, förlopp programvara, Red Hat, SITA, programvara AG, Solace system, VMware, WSO2, Zenika.
* **Användaren företag**: Bank Sydamerika, kredit Suisse, Deutsche Boerse, Goldman Sachs, JPMorgan Chase.

Vissa av hello åberopade ofta fördelarna med öppna standarder inkluderar:

* Mindre risk för lås i leverantör
* Samverkan
* Bred tillgängligheten för bibliotek och verktygsuppsättning
* Skydd mot föråldring
* Tillgängligheten för kunniga personer
* Lägre och hanterbar risk

## <a name="amqp-10-and-service-bus"></a>AMQP 1.0 och Service Bus
AMQP 1.0-stöd i Azure Service Bus innebär att du nu kan utnyttja hello Service Bus queuing och publicera/prenumerera asynkrona meddelandetjänsten funktioner mellan olika plattformar med en effektiv binär-protokollet. Dessutom kan du skapa program består av komponenter som skapats med en blandning av språk och ramverk operativsystem.

hello illustrerar följande bild ett exempel på distribution i vilken Java-klienter som körs på Linux som skrivits med hello standard Service för Java-meddelande (JMS) API och .NET-klienter som körs på Windows, exchange-meddelanden via Service Bus använder AMQP 1.0.

![][0]

**Bild 1: Exempel distributionsscenariot visar meddelanden med hjälp av Service Bus och AMQP 1.0 på flera plattformar**

På den här gången hello är följande klientbibliotek kända toowork med Service Bus:

| Språk | Bibliotek |
| --- | --- |
| Java |Apache Qpid Java meddelandet Service (JMS)-klient<br/>IIT programvara SwiftMQ Java-klient |
| C |Apache Qpid Proton-C |
| PHP |Apache Qpid Proton-PHP |
| Python |Apache Qpid Proton-Python |
| C# |AMQP .net Lite |

**Bild 2: Tabell med AMQP 1.0-klientbibliotek**

## <a name="summary"></a>Sammanfattning
* AMQP 1.0 är en öppen, tillförlitlig meddelanden protokoll som du kan använda toobuild hybridprogram för flera plattformar. AMQP 1.0 är en OASIS standard.
* AMQP 1.0-stöd är nu tillgängligt i Azure Service Bus samt Service Bus för Windows Server (Service Bus 1.1). Priser är samma som för hello befintliga protokoll hello.

## <a name="next-steps"></a>Nästa steg
Redo toolearn mer? Besök hello följande länkar:

* [Använda Service Bus från .NET med AMQP]
* [Med AMQP Service Bus från Java]
* [Installera Apache Qpid Proton-C på en Azure Linux-dator]
* [AMQP i Service Bus för Windows Server]

[0]: ./media/service-bus-amqp-overview/service-bus-amqp-1.png
[Använda Service Bus från .NET med AMQP]: service-bus-amqp-dotnet.md
[Med AMQP Service Bus från Java]: service-bus-amqp-java.md
[Installera Apache Qpid Proton-C på en Azure Linux-dator]: service-bus-amqp-apache.md
[AMQP i Service Bus för Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx
