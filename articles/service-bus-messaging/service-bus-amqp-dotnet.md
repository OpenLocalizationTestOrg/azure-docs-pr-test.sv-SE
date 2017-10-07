---
title: aaaService Bus med .NET och AMQP 1.0 | Microsoft Docs
description: "Med hjälp av Azure Service Bus från .NET med AMQP"
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 332bcb13-e287-4715-99ee-3d7d97396487
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: sethm
ms.openlocfilehash: d8b40f92ba29058951556fa3db1adcf9383ee69f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-service-bus-from-net-with-amqp-10"></a>Använda Service Bus från .NET med AMQP 1.0

## <a name="downloading-hello-service-bus-sdk"></a>Hämta hello Service Bus SDK

AMQP 1.0-support är tillgänglig i hello Service Bus SDK version 2.1 eller senare. Du kan kontrollera att du har hello senaste versionen genom att hämta hello Service Bus bits från [NuGet][NuGet].

## <a name="configuring-net-applications-toouse-amqp-10"></a>Konfigurerar .NET-program toouse AMQP 1.0

Som standard kommunicerar hello Service Bus .NET-klientbibliotek med hello Service Bus-tjänst som använder en dedikerad SOAP-baserat protokoll. toouse AMQP 1.0 i stället för hello standardprotokoll kräver explicit konfigurationen på hello Service Bus-anslutningssträng som beskrivs i nästa avsnitt om hello. Förutom den här ändringen ändras programkod inte när du använder AMQP 1.0.

Det finns några API-funktioner som inte stöds när du använder AMQP i hello aktuella versionen. Dessa funktioner som inte stöds nämns senare under hello [stöds inte funktioner, begränsningar och beteendebaserade skillnader](#unsupported-features-restrictions-and-behavioral-differences). Vissa avancerade konfigurationsinställningar hello har också en annan betydelse när du använder AMQP.

### <a name="configuration-using-appconfig"></a>Konfiguration med hjälp av App.config

Det är bra för program toouse hello App.config-filen toostore konfigurationsinställningar. Du kan använda App.config toostore hello Service Bus-anslutningssträng för Service Bus-program. En exempel App.config-fil är följande:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
    </appSettings>
</configuration>
```

Hej värdet för hello `Microsoft.ServiceBus.ConnectionString` inställningen är hello Service Bus-anslutningssträng som används tooconfigure hello anslutning tooService Bus. hello format är följande:

`Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp`

Där `[namespace]` och `SharedAccessKey` hämtas från hello [Azure-portalen] [ Azure portal] när du skapar en Service Bus-namnrymd. Mer information finns i [skapa ett namnområde för Service Bus använder hello Azure-portalen][Create a Service Bus namespace using hello Azure portal].

När du använder AMQP, bifoga hello-anslutningssträngen med `;TransportType=Amqp`. Notation instruerar hello klienten biblioteket toomake dess anslutning tooService Bus använder AMQP 1.0.

## <a name="message-serialization"></a>Meddelande-serialisering

När du använder hello standardprotokoll hello serialisering standardbeteendet för hello klientbiblioteket för .NET är toouse hello [DataContractSerializer] [ DataContractSerializer] skriver tooserialize en [BrokeredMessage ] [ BrokeredMessage] instans för överföring mellan hello klientbiblioteket och hello Service Bus-tjänst. När du använder hello AMQP transportläge hello klientbiblioteket använder hello AMQP typsystemet för serialisering av hello [asynkrona meddelanden] [ BrokeredMessage] i en AMQP-meddelande. Den här serialisering kan hello meddelandet toobe emot och tolkas av ett mottagande program som körs kan vara på en annan plattform, till exempel ett Java-program som använder hello JMS API tooaccess Service Bus.

När du skapar en [BrokeredMessage] [ BrokeredMessage] instans, du kan tillhandahålla ett .NET-objekt som parameter toohello konstruktorn tooserve som hello del av hello-meddelande. För objekt som kan vara mappade tooAMQP primitiva typer, serialiseras hello brödtext till AMQP-datatyper. Om hello-objekt inte kan mappas direkt till en primitiv typ AMQP; det vill säga en anpassad typ som definieras av programmet hello sedan hello objekt serialiseras med hello [DataContractSerializer][DataContractSerializer], och hello serialiseras byte skickas i ett AMQP data visas.

toofacilitate samverkan med icke-.NET-klienter använder endast .NET-typer som kan serialiseras direkt till AMQP typer för hello hello-meddelande. hello i den följande tabellen beskrivs de typer och hello motsvarande mappning toohello AMQP typsystemet.

| Objekttyp för .NET brödtext | Mappade AMQP typ | Typen av AMQP brödtext avsnitt |
| --- | --- | --- |
| bool |Booleskt värde |AMQP värde |
| Mottagna byte |ubyte |AMQP värde |
| ushort |ushort |AMQP värde |
| uint |uint |AMQP värde |
| ulong |ulong |AMQP värde |
| sbyte |Mottagna byte |AMQP värde |
| kort |kort |AMQP värde |
| int |int |AMQP värde |
| lång |lång |AMQP värde |
| flyttal |flyttal |AMQP värde |
| dubbla |dubbla |AMQP värde |
| Decimal |decimal128 |AMQP värde |
| Char |Char |AMQP värde |
| Datum och tid |tidsstämpel |AMQP värde |
| GUID |UUID |AMQP värde |
| byte] |Binär |AMQP värde |
| Sträng |Sträng |AMQP värde |
| System.Collections.IList |lista |AMQP värde: objekt i samlingen hello kan bara vara de som definieras i den här tabellen. |
| System.Array |matris |AMQP värde: objekt i samlingen hello kan bara vara de som definieras i den här tabellen. |
| System.Collections.IDictionary |karta |AMQP värde: objekt i samlingen hello kan bara vara de som definieras i den här tabellen. Obs: endast strängnycklar stöds. |
| URI: N |Beskrivs sträng (se följande tabell hello) |AMQP värde |
| DateTimeOffset |Beskrivs långt (se följande tabell hello) |AMQP värde |
| TimeSpan |Beskrivs långt (se följande hello) |AMQP värde |
| Dataströmmen |Binär |AMQP Data (kan vara flera). hello Data avsnitt innehåller hello rå byte från hello Stream-objektet. |
| Andra objekt |Binär |AMQP Data (kan vara flera). Innehåller hello serialiseras binärt av hello-objekt som använder hello DataContractSerializer eller en serialiserare som tillhandahålls av programmet hello. |

| .NET-typ | Mappade AMQP beskrivs typen | Anteckningar |
| --- | --- | --- |
| URI: N |`<type name=”uri” class=restricted source=”string”> <descriptor name=”com.microsoft:uri” /></type>` |Uri.AbsoluteUri |
| DateTimeOffset |`<type name=”datetime-offset” class=restricted source=”long”> <descriptor name=”com.microsoft:datetime-offset” /></type>` |DateTimeOffset.UtcTicks |
| TimeSpan |`<type name=”timespan” class=restricted source=”long”> <descriptor name=”com.microsoft:timespan” /></type> ` |TimeSpan.Ticks |

## <a name="unsupported-features-restrictions-and-behavioral-differences"></a>Funktioner som inte stöds, begränsningar och beteendebaserade skillnader

hello följande funktioner i hello Service Bus .NET-API är för närvarande stöds inte när du använder AMQP:

* Transaktioner
* Skicka via överföring mål

Det finns också några mindre skillnader vad gäller hello funktionssätt hello Service Bus .NET-API när du använder AMQP, jämfört med toohello standardprotokoll:

* Hej [OperationTimeout] [ OperationTimeout] egenskapen ignoreras.
* `MessageReceiver.Receive(TimeSpan.Zero)`implementeras som `MessageReceiver.Receive(TimeSpan.FromSeconds(10))`.
* Slutför meddelanden av Lås token kan endast göras av hello meddelandet mottagare som ursprungligen emot hälsningsmeddelande.

## <a name="controlling-amqp-protocol-settings"></a>Kontrollera inställningarna för AMQP-protokollet

Hej [.NET API: er](/dotnet/api/) exponera flera inställningar toocontrol hello funktionssätt hello AMQP-protokollet:

* **[MessageReceiver.PrefetchCount](/dotnet/api/microsoft.servicebus.messaging.messagereceiver.prefetchcount?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessageReceiver_PrefetchCount)**: kontroller hello inledande kredit tillämpas tooa länk. hello standardvärdet är 0.
* **[MessagingFactorySettings.AmqpTransportSettings.MaxFrameSize](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.maxframesize?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_MaxFrameSize)**: kontroller hello AMQP ram maxstorleken erbjuds under hello förhandling vid anslutningen öppen. hello standardinställningen är 65 536 byte.
* **[MessagingFactorySettings.AmqpTransportSettings.BatchFlushInterval](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.batchflushinterval?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_BatchFlushInterval)**: om överföringar är batchable det här värdet fastställer hello Maximal fördröjning för att skicka disposition. Ärvs av avsändare/mottagare som standard. Enskilda avsändare/mottagare kan åsidosätta hello standard, vilket är 20 millisekunder.
* **[MessagingFactorySettings.AmqpTransportSettings.UseSslStreamSecurity](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.usesslstreamsecurity?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_UseSslStreamSecurity)**: styr om AMQP anslutningar upprättas via en SSL-anslutning. hello standardvärdet är **SANT**.

## <a name="next-steps"></a>Nästa steg

Redo toolearn mer? Besök hello följande länkar:

* [Översikt över Service Bus AMQP]
* [AMQP 1.0-stöd för Service Bus partitionerad köer och ämnen]
* [AMQP i Service Bus för Windows Server]

[Create a Service Bus namespace using hello Azure portal]: service-bus-create-namespace-portal.md
[DataContractSerializer]: https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer.aspx
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage?view=azureservicebus-4.0.0
[Microsoft.ServiceBus.Messaging.MessagingFactory.AcceptMessageSession]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory.acceptmessagesession?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessagingFactory_AcceptMessageSession
[OperationTimeout]: /dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout
[NuGet]: http://nuget.org/packages/WindowsAzure.ServiceBus/
[Azure portal]: https://portal.azure.com
[Översikt över Service Bus AMQP]: service-bus-amqp-overview.md
[AMQP 1.0-stöd för Service Bus partitionerad köer och ämnen]: service-bus-partitioned-queues-and-topics-amqp-overview.md
[AMQP i Service Bus för Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx
