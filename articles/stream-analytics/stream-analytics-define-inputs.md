---
title: "Dataanslutningen: dataströmmen indata från en händelseström | Microsoft Docs"
description: "Lär dig mer om hur du konfigurerar en data-anslutning tooStream Analytics som kallas ”indata'. Indata innehåller en dataström från händelser och även referensdata."
keywords: "dataströmmen, dataanslutning, händelseströmmen"
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 8155823c-9dd8-4a6b-8393-34452d299b68
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 07/05/2017
ms.author: samacha
ms.openlocfilehash: be2008f159061c5c9be9d0314c27fa67193e3269
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="data-connection-learn-about-data-stream-inputs-from-events-toostream-analytics"></a>Dataanslutningen: Lär dig mer om data dataströmmen indata från händelser tooStream Analytics
hello data anslutning tooa Stream Analytics-jobbet är en dataström med händelser från en datakälla som är refererad tooas hello jobbet *inkommande*. Stream Analytics har förstklassigt integrering med Azure strömmen datakällor, inklusive [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/), [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/), och [Azure Blob storage](https://azure.microsoft.com/services/storage/blobs/). Dessa indata källor kan vara från hello samma Azure-prenumeration som analytics-jobb, eller från en annan prenumeration.

## <a name="data-input-types-data-stream-and-reference-data"></a>Data indatatyper: Data ström och referensdata data
När data skickas tooa datakälla, har den förbrukas av hello Stream Analytics-jobbet och bearbetas i realtid. Indata är indelade i två typer: data strömma indata och referera till indata.

### <a name="data-stream-inputs"></a>Dataströmmen indata
En dataström är en unbounded sekvens av händelser över tid. Stream Analytics-jobb måste innehålla minst en inkommande dataström. Händelsehubbar, IoT-hubb och Blob storage stöds som inkommande datakällor för dataströmmen. Händelsehubbar är används toocollect händelseströmmar från flera enheter och tjänster. Dessa strömmar kan innehålla sociala medier aktivitetsfeeds, lager handel information eller data från sensorer. IoT-hubbar är optimerad toocollect data från anslutna enheter i scenarier i Sakernas Internet (IoT).  BLOB-lagring kan användas som en Indatakällan för att föra in stora mängder data som en dataström som loggfiler.  

### <a name="reference-data"></a>Referensdata
Stream Analytics stöder också indata som kallas *referensdata*. Det är extra data som är fasta eller ändrar långsamt. Den används vanligtvis för att utföra korrelation och sökningar. Exempelvis kan du ansluta till data i hello data dataströmmen inkommande toodata i hello referensdata ungefär som utför en SQL-anslutning till toolook statiska värden. Azure Blob storage är för närvarande stöds endast hello Indatakällan för referensdata. Referens för datakällan blobbar begränsas too100 MB i storlek.

hur toocreate referens indata, se toolearn [Använd referensdata](stream-analytics-use-reference-data.md).  

## <a name="create-data-stream-input-from-event-hubs"></a>Skapa en inkommande dataström från Event Hubs

Händelsehubbar i Azure ger skalbara händelse ingestors att publicera och prenumerera. En händelsehubb kan samla in miljontals händelser per sekund, så att du kan bearbeta och analysera hello stora mängder data som produceras av dina anslutna enheter och program. Händelsehubbar och Stream Analytics tillsammans ger dig en lösning för slutpunkt till slutpunkt för realtidsanalys – Händelsehubbar kan du feed händelser till Azure i realtid och Stream Analytics-jobb kan bearbeta dessa händelser i realtid. Exempelvis kan du skicka web klick, sensoravläsningar eller online logga händelser tooEvent Hubs. Du kan sedan skapa Stream Analytics-jobb toouse Händelsehubbar som hello indata dataströmmar för realtid filtrering, sammanställa och korrelation.

hello standard tidsstämpeln för händelser som kommer från Händelsehubbar i Stream Analytics är hello tidsstämpel som hello händelse anlänt i hello händelsehubb, vilket är `EventEnqueuedUtcTime`. tooprocess hello data som en dataström med en tidsstämpel i hello händelsenyttolast måste du använda hello [TIMESTAMP BY](https://msdn.microsoft.com/library/azure/dn834998.aspx) nyckelord.

### <a name="consumer-groups"></a>Konsumentgrupper
Du bör konfigurera varje inkommande toohave för Stream Analytics event hub sin egen konsumentgrupp. Indata kan läsas av mer än en läsare nedströms när ett jobb som innehåller en självkoppling eller när den har flera inmatningar. Den här situationen påverkar hello antalet läsare i en enskild konsument-grupp. tooavoid överstiger hello Händelsehubbar gräns på fem läsare per konsumentgrupp per partition, är det en bästa praxis toodesignate konsumenter grupp för varje Stream Analytics-jobbet. Det finns en gräns på 20 konsumentgrupper per händelsehubb. Mer information finns i [Programmeringsguide för Event Hubs](../event-hubs/event-hubs-programming-guide.md).

### <a name="configure-an-event-hub-as-a-data-stream-input"></a>Konfigurera en händelsehubb som en dataström som indata
hello i tabellen nedan beskrivs varje egenskap i hello **nya indata** bladet i hello Azure-portalen när du konfigurerar en händelsehubb som indata.

| Egenskap | Beskrivning |
| --- | --- |
| **Ett inmatat alias** |Ett eget namn som du använder i hello jobbet frågan tooreference detta indata. |
| **Service bus-namnrymd** |Ett Azure Service Bus-namnområde som är en behållare för en uppsättning meddelandeentiteter. När du skapar en ny händelsehubb kan skapa du också en Service Bus-namnrymd. |
| **Namnet på händelsehubben** |hello namnet på hello event hub toouse som indata. |
| **Namnet på händelsehubben princip** |Hej princip för delad åtkomst som ger åtkomst till toohello händelsehubb. Varje princip för delad åtkomst har ett namn, behörigheter som du ställa in och åtkomstnycklar. |
| **Event hub konsumentgrupp** (valfritt) |hello konsumenten grupp toouse tooingest data från hello händelsehubb. Om ingen konsumentgrupp har angetts använder hello Stream Analytics-jobbet hello förinställd konsumentgrupp. Vi rekommenderar att du använder en distinkta konsumentgrupp för varje Stream Analytics-jobbet. |
| **Händelsen serialiseringsformat** |hello serialiseringsformat (JSON, CSV eller Avro) för hello inkommande dataström. |
| **Kodning** | UTF-8 är för närvarande hello stöds endast kodningsformat. |

När data kommer från en händelsehubb, har du åtkomst toohello följande fält med metadata i Stream Analytics-fråga:

| Egenskap | Beskrivning |
| --- | --- |
| **EventProcessedUtcTime** |hello datum och tid har att hello-händelsen bearbetats av Stream Analytics. |
| **EventEnqueuedUtcTime** |hello datum och tid togs hello händelsen emot av Händelsehubbar. |
| **Partitions-ID** |hello nollbaserade partitions-ID för inkommande hello-kort. |

Till exempel kan med hjälp av dessa fält, du skriva en fråga som hello följande exempel:

````
SELECT
    EventProcessedUtcTime,
    EventEnqueuedUtcTime,
    PartitionId
FROM Input
````

## <a name="create-data-stream-input-from-iot-hub"></a>Skapa en inkommande dataström från IoT-hubb
Azure Iot Hub är en mycket skalbar publicera och prenumerera händelseinmatare som är optimerade för IoT-scenarier.

hello standard tidsstämpeln för händelser som kommer från en IoT-hubb i Stream Analytics är hello tidsstämpel som hello händelse anlänt i hello IoT-hubb som är `EventEnqueuedUtcTime`. tooprocess hello data som en dataström med en tidsstämpel i hello händelsenyttolast måste du använda hello [TIMESTAMP BY](https://msdn.microsoft.com/library/azure/dn834998.aspx) nyckelord.

> [!NOTE]
> Endast meddelanden som skickas med en `DeviceClient` egenskapen kan bearbetas.
> 
> 

### <a name="consumer-groups"></a>Konsumentgrupper
Du bör konfigurera varje Stream Analytics IoT-hubb inkommande toohave sin egen konsumentgrupp. Indata kan läsas av mer än en läsare nedströms när ett jobb som innehåller en självkoppling eller när den har flera inmatningar. Den här situationen påverkar hello antalet läsare i en enskild konsument-grupp. tooavoid överstiger hello Azure IoT Hub gräns på fem läsare per konsumentgrupp per partition, är det en bästa praxis toodesignate konsumenter grupp för varje Stream Analytics-jobbet.

### <a name="configure-an-iot-hub-as-a-data-stream-input"></a>Konfigurera en IoT-hubb som en dataström som indata
hello i tabellen nedan beskrivs varje egenskap i hello **nya indata** bladet i hello Azure-portalen när du konfigurerar en IoT-hubb som indata.

| Egenskap | Beskrivning |
| --- | --- |
| **Ett inmatat alias** |Ett eget namn som du använder i hello jobbet frågan tooreference detta indata.|
| **IoT-hubb** |hello namnet på hello IoT-hubb toouse som indata. |
| **Slutpunkt** |hello-slutpunkt för hello IoT-hubb.|
| **Principnamn för delad åtkomst** |hello delad åtkomstprincip som ger åtkomst toohello IoT-hubb. Varje princip för delad åtkomst har ett namn, behörigheter som du ställa in och åtkomstnycklar. |
| **Princip för delade åtkomstnyckeln** |hello delade åtkomstnyckeln används tooauthorize åtkomst toohello IoT-hubb. |
| **Konsumentgrupp** (valfritt) |hello konsumenten grupp toouse tooingest data från hello IoT-hubb. Om ingen konsumentgrupp har angetts använder Stream Analytics-jobbet hello förinställd konsumentgrupp. Vi rekommenderar att du använder en annan konsumentgrupp för varje Stream Analytics-jobbet. |
| **Händelsen serialiseringsformat** |hello serialiseringsformat (JSON, CSV eller Avro) för hello inkommande dataström. |
| **Kodning** |UTF-8 är för närvarande hello stöds endast kodningsformat. |

När data kommer från en IoT-hubb, har du åtkomst toohello följande fält med metadata i Stream Analytics-fråga:

| Egenskap | Beskrivning |
| --- | --- |
| **EventProcessedUtcTime** |hello händelsen bearbetades hello datum och tid. |
| **EventEnqueuedUtcTime** |hello datum och tid togs hello händelsen emot av hello IoT-hubb. |
| **Partitions-ID** |hello nollbaserade partitions-ID för inkommande hello-kort. |
| **IoTHub.MessageId** | Ett ID som har använt toocorrelate dubbelriktad kommunikation i IoT-hubb. |
| **IoTHub.CorrelationId** |Ett ID som används i meddelandesvar och feedback i IoT-hubb. |
| **IoTHub.ConnectionDeviceId** |hello-ID för autentisering används toosend för det här meddelandet. Det här värdet är stämplad på servicebound meddelanden i hello IoT hub. |
| **IoTHub.ConnectionDeviceGenerationId** |hello generations-ID för hello autentiserade enhet som tidigare har använt toosend det här meddelandet. Det här värdet är stämplad på servicebound meddelanden i hello IoT hub. |
| **IoTHub.EnqueuedTime** |hello tid när hello-meddelande togs emot av hello IoT-hubb. |
| **IoTHub.StreamId** |En anpassad händelse-egenskap som lagts till av hello avsändaren enhet. |


## <a name="create-data-stream-input-from-blob-storage"></a>Skapa en inkommande dataström från Blob storage
För scenarier med stora mängder Ostrukturerade data toostore i molnet hello erbjuder Azure Blob storage en kostnadseffektiv och skalbar lösning. Data i Blob storage anses vanligtvis data i vila. Det kan dock bearbetas som en dataström med Stream Analytics. Ett typiskt scenario för Blob storage indata med Stream Analytics är bearbetar. I det här scenariot har spelats in telemetridata från ett system och behov toobe parsas och bearbetas tooextract meningsfull information.

hello standard tidsstämpeln för Blob storage-händelser i Stream Analytics är hello tidsstämpel som hello blob ändrades senast, vilket är `BlobLastModifiedUtcTime`. tooprocess hello data som en dataström med en tidsstämpel i hello händelsenyttolast måste du använda hello [TIMESTAMP BY](https://msdn.microsoft.com/library/azure/dn834998.aspx) nyckelord.

CSV-formaterad indata *kräver* ett sidhuvud raden toodefine fält för hello datamängden. Alla rubrikfält raden måste dessutom vara unikt.

> [!NOTE]
> Stream Analytics stöder inte att lägga till innehåll tooan befintlig blob. Stream Analytics ska visa en blob bara en gång och eventuella ändringar som uppstår i hello blob när hello jobbet har läst hello data bearbetas inte. Bästa praxis är tooupload alla hello-data en gång och sedan att lägga till inte händelser toothat blobstore.
> 

### <a name="configure-blob-storage-as-a-data-stream-input"></a>Konfigurera Blob storage som en dataström som indata

hello i tabellen nedan beskrivs varje egenskap i hello **nya indata** bladet i hello Azure-portalen när du konfigurerar Blob storage som indata.

| Egenskap | Beskrivning |
| --- | --- |
| **Ett inmatat alias** | Ett eget namn som du använder i hello jobbet frågan tooreference detta indata. |
| **Lagringskonto** | hello namnet på hello storage-konto där hello blob-filerna lagras. |
| **Lagringskontonyckel** | hello hemlig nyckel kopplad till hello storage-konto. |
| **Behållaren** | hello behållare för hello blob-indata. Behållare innehåller en logisk gruppering för blobbar som lagras i hello Microsoft Azure Blob-tjänsten. När du överför en blob-toohello Azure Blob storage-tjänst måste du ange en behållare för blobben. |
| **Sökvägar** (valfritt) | hello sökväg används toolocate hello blobbar i hello angivna behållaren. I hello sökvägen du anger en eller flera instanser av följande tre variabler hello: `{date}`, `{time}`, eller`{partition}`<br/><br/>Exempel 1:`cluster1/logs/{date}/{time}/{partition}`<br/><br/>Exempel 2:`cluster1/logs/{date}`<br/><br/>Hej `*` tecken är inte ett tillåtet värde för hello sökväg prefix. Endast giltiga <a HREF="https://msdn.microsoft.com/library/azure/dd135715.aspx">Azure blob-tecken</a> tillåts. |
| **Datumformatet** (valfritt) | Om du använder variabeln i hello datum i hello sökväg hello datumformat i vilka hello filerna ordnas. Exempel:`YYYY/MM/DD` |
| **Tidsformat** (valfritt) |  Om du använder hello tid variabeln i hello sökväg hello tidsformat i vilka hello filerna ordnas. Hello stöds endast värdet är för närvarande `HH`. |
| **Händelsen serialiseringsformat** | hello serialiseringsformat (JSON, CSV eller Avro) för inkommande dataströmmar. |
| **Kodning** | UTF-8 är för närvarande kodningsformat hello stöds bara för CSV och JSON. |

När data kommer från en källa för Blob storage, har du åtkomst toohello följande fält med metadata i Stream Analytics-fråga:

| Egenskap | Beskrivning |
| --- | --- |
| **BlobName** |hello namnet på hello inkommande blob som hello händelsen kom från. |
| **EventProcessedUtcTime** |hello datum och tid har att hello-händelsen bearbetats av Stream Analytics. |
| **BlobLastModifiedUtcTime** |hello datum och tid ändrades som hello blob senast. |
| **Partitions-ID** |hello nollbaserade partitions-ID för inkommande hello-kort. |

Till exempel kan med hjälp av dessa fält, du skriva en fråga som hello följande exempel:

````
SELECT
    BlobName,
    EventProcessedUtcTime,
    BlobLastModifiedUtcTime
FROM Input
````

## <a name="get-help"></a>Få hjälp
För ytterligare hjälp försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nästa steg
Du har lärt dig om anslutningsalternativ för data i Azure för Stream Analytics-jobb. toolearn mer om Stream Analytics, se:

* [Komma igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Referens för Azure Stream Analytics-frågespråket](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referens för Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
