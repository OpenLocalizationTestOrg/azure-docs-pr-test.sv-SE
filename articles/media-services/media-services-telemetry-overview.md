---
title: aaaAzure Media Services telemetri | Microsoft Docs
description: "Den här artikeln ger en översikt över Azure Media Services telemetri."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 95c20ec4-c782-4063-8042-b79f95741d28
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 659e1c947a77aad0e4acacb541d95714da4775ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-media-services-telemetry"></a>Azure Media Services telemetri

Azure Media Services (AMS) kan du tooaccess telemetri/mätvärdesdata för dess tjänster. hello nuvarande version av AMS kan du samla in telemetridata för live **kanal**, **StreamingEndpoint**, och live **Arkiv** entiteter. 

Telemetri är skriven tooa lagring tabell i ett Azure Storage-konto som du anger (vanligtvis använder du hello storage-konto som är associerade med AMS-kontot). 

hello telemetri system hanterar inte datalagring. Du kan ta bort hello gamla telemetridata genom att ta bort hello storage-tabeller.

Det här avsnittet beskrivs hur tooconfigure och använda hello AMS telemetri.

## <a name="configuring-telemetry"></a>Konfigurera telemetri

Du kan konfigurera telemetri på en nivå kornighet komponent. Det finns två detaljnivåer ”Normal” och ”utförlig”. För närvarande båda nivåerna returnera hello samma information. Det rekommenderas toouse ”Normal. 

Hej följande avsnitt visas hur tooenable telemetri:

[Aktivera telemetri med .NET](media-services-dotnet-telemetry.md) 

[Aktivera telemetri med övriga](media-services-rest-telemetry.md)

## <a name="consuming-telemetry-information"></a>Förbrukar telemetri information

Telemetri skrivs tooan Azure-Lagringstabellen i hello storage-konto som du angav när du har konfigurerat telemetri för hello Media Services-konto. Det här avsnittet beskrivs hello storage-tabeller för hello mått.

Du kan använda telemetridata i något av följande sätt hello:

- Läsa data direkt från Azure Table Storage (t.ex. med hello Storage SDK: N). Hello beskrivning av telemetri storage-tabeller finns hello **förbrukar telemetri information** i [detta](https://msdn.microsoft.com/library/mt742089.aspx) avsnittet.

Eller

- Använda hello-stöd i hello Media Services .NET SDK för att läsa in lagringsdata, enligt beskrivningen i [detta](media-services-dotnet-telemetry.md) avsnittet. 


hello telemetri schemat som beskrivs nedan är utformad toogive goda prestanda inom hello gränserna för Azure Table Storage:

- Data har partitionerats med konto-ID och tjänsten tooallow telemetri från varje tjänst toobe efterfrågas oberoende av varandra.
- Partitioner innehålla hello datum toogive en rimlig övre gräns för hello partitionsstorlek.
- Raden nycklar frågas i tid i omvänd ordning tooallow hello senaste telemetri objekt toobe för en viss tjänst.

Det här ger många hello vanliga frågor toobe effektivt:

- Parallella, oberoende hämtning av data för olika tjänster.
- Hämta alla data för en viss tjänst i ett datumintervall.
- Hämtar hello senaste data för en tjänst.

### <a name="telemetry-table-storage-output-schema"></a>Telemetri tabellschemat lagring utdata

Telemetridata lagras i mängd i en tabell, ”TelemetryMetrics20160321” där ”20160321” är datum för hello skapat tabellen. Telemetri systemet skapar en separat tabell för varje dag baserat på UTC 00:00. hello tabellen används toostore återkommande värden som infognings bithastighet inom en viss period tid, antal skickade byte osv. 

Egenskap|Värde|Exempel/Anteckningar
---|---|---
PartitionKey|{konto-ID} _ {enhets-ID}|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66 < br /<br/>Hej konto-ID ingår i hello partition viktiga toosimplify arbetsflöden där flera Media Services-konton skriver toohello samma lagringskonto.
RowKey|{sekunder toomidnight} _ {slumpmässigt värde}|01688_00199<br/><br/>Hej radnyckel börjar med hello antal sekunder toomidnight tooallow övre n format frågor inom en partition. Mer information finns i [detta](../cosmos-db/table-storage-design-guide.md#log-tail-pattern) artikel. 
tidsstämpel|Datum/tid|Automatisk tidsstämpel från hello Azure-tabellen 2016-09-09T22:43:42.241Z
Typ|hello typ av hello-enhet som tillhandahåller telemetridata|Kanal-StreamingEndpoint-Arkiv<br/><br/>Händelsetypen är bara ett strängvärde.
Namn|hello namnet på hello telemetri händelse|ChannelHeartbeat/StreamingEndpointRequestLog
ObservedTime|hello tid hello telemetri händelsen inträffade (UTC)|2016-09-09T22:42:36.924Z<br/><br/>hello sett tid tillhandahålls av hello entitet skicka hello telemetri (till exempel en kanal). Det kan finnas synkroniseringsproblem med mellan komponenter så att det här värdet är ungefärlig tid
ServiceID|{tjänsten ID}|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
Entitet-specifika egenskaper|Som definierats av hello-händelse|StreamName: stream1, bithastighet 10123 vid...<br/><br/>hello återstående egenskaper har definierats för hello angivna händelsetyp. Azure innehållet är nyckel/värde-par.  (det vill säga olika rader i tabellen hello har olika uppsättningar med egenskaper).

### <a name="entity-specific-schema"></a>Företagsspecifika schema

Det finns tre typer av företagsspecifika telemetriska poster varje pushas den med följande frekvens hello:

- Strömningsslutpunkter: var 30 sekunder
- Live kanaler: varje minut
- Live Arkiv: varje minut

**Strömmande slutpunkt**

Egenskap|Värde|Exempel
---|---|---
PartitionKey|PartitionKey|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66
RowKey|RowKey|01688_00199
tidsstämpel|tidsstämpel|Automatisk tidsstämpel från Azure Table 2016-09-09T22:43:42.241Z
Typ|Typ|StreamingEndpoint
Namn|Namn|StreamingEndpointRequestLog
ObservedTime|ObservedTime|2016-09-09T22:42:36.924Z
ServiceID|Tjänst-ID|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
Värdnamn|Värdnamnet för hello slutpunkt|builddemoserver.Origin.mediaservices.Windows.NET
statusCode|Registrerar HTTP-status|200
ResultCode|Information om resultat|S_OK
RequestCount|Totalt antal begäranden i hello aggregering|3
BytesSent|Sammanställda byte som skickats|2987358
ServerLatency|Genomsnittlig server svarstid (inklusive lagring)|129
E2ELatency|Genomsnittlig svarstid för slutpunkt till slutpunkt|250

**Live kanal**

Egenskap|Värde|Exempel/Anteckningar
---|---|---
PartitionKey|PartitionKey|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66
RowKey|RowKey|01688_00199
tidsstämpel|tidsstämpel|Automatisk tidsstämpel från hello Azure-tabellen 2016-09-09T22:43:42.241Z
Typ|Typ|Kanal
Namn|Namn|ChannelHeartbeat
ObservedTime|ObservedTime|2016-09-09T22:42:36.924Z
ServiceID|Tjänst-ID|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
TrackType|Typ av spåra video ljud/text|ljud och
TrackName|Namnet på hello spåra|video/audio_1
Bithastighet|Spåra bithastighet|785000
CustomAttributes||   
IncomingBitrate|Faktiska inkommande bithastighet|784548
OverlapCount|Mata in överlapp av hello|0
DiscontinuityCount|Avvikelse för spår|0
LastTimestamp|Tidsstämpel för senaste infogade data|1800488800
NonincreasingCount|Antal fragment ignoreras på grund av toonon-ökande tidsstämpel|2
UnalignedKeyFrames|Om vi har tagit emot fragmenten (över kvalitet) där nyckeln ramar justeras inte |True
UnalignedPresentationTime|Om vi har tagit emot fragmenten (alla kvalitet nivåer/spår) där presentation tiden justeras inte|True
UnexpectedBitrate|Värdet är true, == beräknade/faktisk bithastighet för ljud och video spåra > 40 000 bit/s och IncomingBitrate eller IncomingBitrate och actualBitrate skiljer sig med 50% 0 |True
Felfri|True, om <br/>overlapCount, <br/>DiscontinuityCount, <br/>NonIncreasingCount, <br/>UnalignedKeyFrames, <br/>UnalignedPresentationTime, <br/>UnexpectedBitrate<br/> är alla 0|True<br/><br/>Felfri är en sammansatt funktion som returnerar värdet false när något av följande villkor hold hello:<br/><br/>-OverlapCount > 0<br/>-DiscontinuityCount > 0<br/>-NonincreasingCount > 0<br/>-UnalignedKeyFrames == True<br/>-UnalignedPresentationTime == True<br/>-UnexpectedBitrate == True

**Live-Arkiv**

Egenskap|Värde|Exempel/Anteckningar
---|---|---
PartitionKey|PartitionKey|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66
RowKey|RowKey|01688_00199
tidsstämpel|tidsstämpel|Automatisk tidsstämpel från hello Azure-tabellen 2016-09-09T22:43:42.241Z
Typ|Typ|Arkiv
Namn|Namn|ArchiveHeartbeat
ObservedTime|ObservedTime|2016-09-09T22:42:36.924Z
ServiceID|Tjänst-ID|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
ManifestName|URL: en för programmet|Asset-eb149703-ed0a-483c-91c4-e4066e72cce3/a0a5cfbf-71ec-4BD2-8c01-a92a2b38c9ba.ISM
TrackName|Namnet på hello spåra|Audio_1
TrackType|Typ av hello spåra|Ljud och video
CustomAttribute|Hexadecimala strängen som skiljer mellan olika spåra med samma namn och bithastighet (multi kameravinkel)|
Bithastighet|Spåra bithastighet|785000
Felfri|True, om FragmentDiscardedCount == 0 & & ArchiveAcquisitionError == False|True (dessa två värden finns inte i hello mått men de finns i hello källa händelse)<br/><br/>Felfri är en sammansatt funktion som returnerar värdet false när något av följande villkor hold hello:<br/><br/>-FragmentDiscardedCount > 0<br/>-ArchiveAcquisitionError == True

## <a name="general-qa"></a>Allmänna frågor och svar

### <a name="how-tooconsume-metrics-data"></a>Hur tooconsume mått data?

Mått data lagras som en serie av Azure-tabeller i hello kundens lagringskonto. Den här informationen kan användas med hello följande verktyg:

- AMS SDK
- Microsoft Azure Lagringsutforskaren (stöder export toocomma-kommateckenavgränsade och bearbetade i Excel)
- REST API

### <a name="how-toofind-average-bandwidth-consumption"></a>Hur toofind genomsnittlig bandbreddsanvändning?

hello genomsnittlig bandbreddsanvändning är hello genomsnittliga BytesSent över en tidsperiod.

### <a name="how-toodefine-streaming-unit-count"></a>Hur räkna toodefine enhet?

Antal enheter för strömning hello kan definieras som hello belastning genomströmning från hello service strömningsslutpunkter dividerat med hello belastning genomflödet av en strömmande slutpunkt. hello belastning användbara dataflödet för en strömmande slutpunkten är 160 Mbit/s.
Anta exempelvis att hello belastning genomströmning från en kundservice är 40 Mbit/s (hello maximivärdet för BytesSent över en tidsperiod). Antal enheter för strömning hello är lika too(40 MBps) * (8 bitar/byte) /(160 Mbps) = 2 strömmande enheter.

### <a name="how-toofind-average-requestssecond"></a>Hur toofind medelvärde begäranden per sekund?

toofind hello Genomsnittligt antal begäranden per sekund, beräkna hello Genomsnittligt antal begäranden (RequestCount) över en tidsperiod.

### <a name="how-toodefine-channel-health"></a>Hur kanalen toodefine hälsa?

Kanal hälsa kan definieras som en sammansatt boolesk funktion som är false när något av följande villkor hello håller:

- OverlapCount > 0
- DiscontinuityCount > 0
- NonincreasingCount > 0
- UnalignedKeyFrames == True 
- UnalignedPresentationTime == True 
- UnexpectedBitrate == True


### <a name="how-toodetect-discontinuities"></a>Hur toodetect avbrott?

toodetect avbrott hitta alla kanal poster där DiscontinuityCount > 0. hello motsvarande ObservedTime tidsstämpel anger hello gånger då hello avbrott inträffade.

### <a name="how-toodetect-timestamp-overlaps"></a>Hur toodetect tidsstämpel överlappar?

toodetect tidsstämpel överlappningar hitta alla kanal poster där OverlapCount > 0. hello motsvarande ObservedTime tidsstämpel anger hello gånger på vilka hello tidsstämpel överlappar inträffade.

### <a name="how-toofind-streaming-request-failures-and-reasons"></a>Hur toofind strömning begära fel och orsaker?

toofind strömmande begäran fel och orsaker, hitta alla Strömningsslutpunkt poster där ResultCode är inte lika tooS_OK. hello motsvarande StatusCode fält anger hello varför hello begäran misslyckades.

### <a name="how-tooconsume-data-with-external-tools"></a>Hur tooconsume data med externa verktyg?

Telemetriska data kan bearbetas och visualiseras med hello följande verktyg:

- PowerBI
- Application Insights
- Azure-Monitor (tidigare lådan)
- AMS Live instrumentpanelen
- Azure-portalen (väntar version)

### <a name="how-toomanage-data-retention"></a>Hur toomanage data finns kvar?

hello telemetri system ger inte några kvarhållning datahantering eller automatisk borttagning av gamla poster. Därför behöver toomanage och ta bort poster med gamla manuellt från hello lagringstabellen. Du kan se toostorage SDK för hur toodo den.

## <a name="next-steps"></a>Nästa steg

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback

[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
