---
title: "aaaList Azure Storage-resurser med hello Storage-klientbibliotek för C++ | Microsoft Docs"
description: "Lär dig hur toouse hello visar en lista över API: er i Microsoft Azure Storage-klientbibliotek för C++ tooenumerate behållare, blobbar, köer, tabeller och enheter."
documentationcenter: .net
services: storage
author: dineshmurthy
manager: jahogg
editor: tysonn
ms.assetid: 33563639-2945-4567-9254-bc4a7e80698f
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: dineshm
ms.openlocfilehash: d20a86688938704c24339aa9a1145786783fea5e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="list-azure-storage-resources-in-c"></a>Visa en lista med Azure Storage-resurser i C++
Lista över åtgärder är viktiga toomany utvecklingsscenarier med Azure Storage. Den här artikeln beskriver hur toomost effektivt räkna upp objekt i Azure Storage med hjälp av hello visar en lista över API: er som anges i hello Microsoft Azure Storage-klientbibliotek för C++.

> [!NOTE]
> Den här handboken riktar sig till hello Azure Storage-klientbibliotek för C++ version 2.x som är tillgängliga via [NuGet](http://www.nuget.org/packages/wastorage) eller [GitHub](https://github.com/Azure/azure-storage-cpp).
> 
> 

hello Storage-klientbiblioteket tillhandahåller flera olika metoder toolist eller fråga objekt i Azure Storage. Den här artikeln tar hello följande scenarier:

* Lista behållare i ett konto
* Lista över blobbar i en behållare eller virtuella blob-katalog
* Lista över köer på ett konto
* Lista över tabeller i ett konto
* Fråga entiteter i en tabell

Var och en av dessa metoder visas med olika överlagringar för olika scenarier.

## <a name="asynchronous-versus-synchronous"></a>Asynkron och synkron
Eftersom hello Storage-klientbibliotek för C++ bygger på hello [C++ REST biblioteket](https://github.com/Microsoft/cpprestsdk), vi stöds asynkrona åtgärder med hjälp av [pplx::task](http://microsoft.github.io/cpprestsdk/classpplx_1_1task.html). Exempel:

```cpp
pplx::task<list_blob_item_segment> list_blobs_segmented_async(continuation_token& token) const;
```

Synkrona åtgärder omsluter hello motsvarande asynkrona åtgärder:

```cpp
list_blob_item_segment list_blobs_segmented(const continuation_token& token) const
{
    return list_blobs_segmented_async(token).get();
}
```

Om du arbetar med flera trådade program eller tjänster rekommenderar vi att du använder hello asynkrona API: er direkt i stället för att skapa en tråd toocall hello synkronisering API: er, vilket kan påverka prestandan avsevärt.

## <a name="segmented-listing"></a>Segmenterade lista
hello skala molnlagring kräver segmenterade lista. Exempelvis kan du ha över en miljon blobbar i en Azure blob-behållaren eller över en miljard entiteter i en tabell i Azure. Detta är inte teoretisk siffror, men verkliga kundens användning fall.

Det är därför opraktiska toolist alla objekt i ett enda svar. I stället kan du visa objekt med hjälp av sidindelning. Varje hello visar en lista över API: er har en *segmenterat* överlagra.

hello-svar för en segmenterade åtgärd innehåller:

* <i>_segment</i>, som innehåller hello uppsättning resultat returneras för en enda anrop toohello visar en lista över API.
* *continuation_token*, som har överförts toohello nästa anrop i ordning tooget hello nästa sida i resultatet. När det finns inga fler resultat tooreturn, är hello fortsättningstoken null.

Till exempel en typisk anropet toolist alla blobbar i en behållare kan se ut som hello följande kodavsnitt. hello koden finns i vår [exempel](https://github.com/Azure/azure-storage-cpp/blob/master/Microsoft.WindowsAzure.Storage/samples/BlobsGettingStarted/Application.cpp):

```cpp
// List blobs in hello blob container
azure::storage::continuation_token token;
do
{
    azure::storage::list_blob_item_segment segment = container.list_blobs_segmented(token);
    for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
{
    if (it->is_blob())
    {
        process_blob(it->as_blob());
    }
    else
    {
        process_diretory(it->as_directory());
    }
}

    token = segment.continuation_token();
}
while (!token.empty());
```

Observera att hello antalet resultat som returneras i en sida kan styras efter hello parametern *max_results* i hello överlagring för varje API, till exempel:

```cpp
list_blob_item_segment list_blobs_segmented(const utility::string_t& prefix, bool use_flat_blob_listing,
    blob_listing_details::values includes, int max_results, const continuation_token& token,
    const blob_request_options& options, operation_context context)
```

Om du inte anger hello *max_results* parametern, hello standard maximivärdet för too5000 resultat returneras i en enda sida.

Observera också att en fråga mot Azure Table storage kan returnera några poster eller färre poster än hello värdet hello *max_results* parameter som du angett, även om hello fortsättningstoken inte är tom. En orsak kan vara att hello frågan inte kunde slutföras i fem sekunder. Så länge hello fortsättningstoken inte är tom hello frågan ska fortsätta och din kod anta inte hello storlek på segment resultat.

hello bör kodning mönster för de flesta fall är segmenterat lista, som innehåller explicita fortskrider lista eller en fråga och hur hello-tjänsten svarar tooeach begäran. Särskilt för C++-program eller tjänster kan på lägre nivå kontroll över hello lista pågår hjälpa kontrollen minne och prestanda.

## <a name="greedy-listing"></a>Girig lista
Tidigare versioner av hello Storage-klientbibliotek för C++ (versioner 0.5.0 Förhandsgranska och tidigare) med icke-segmenterade listan API: er för tabeller och köer, som i följande exempel hello:

```cpp
std::vector<cloud_table> list_tables(const utility::string_t& prefix) const;
std::vector<table_entity> execute_query(const table_query& query) const;
std::vector<cloud_queue> list_queues() const;
```

Dessa metoder har implementerats som omslutningar av segmenterade API: er. För varje svar för segmenterade lista hello kod läggs hello resultat tooa vector och returneras alla resultat när hello fullständig behållare har genomsökts.

Den här metoden kan fungera när hello storage-konto eller tabell som innehåller ett litet antal objekt. Men med en ökning i hello antalet objekt kan hello-minne som krävs öka utan begränsning, eftersom alla resultat kvar i minnet. En åtgärd kan ta mycket lång tid under vilken hello anroparen har ingen information om förloppet.

Dessa girig visar en lista över API: er i hello SDK finns i C#, Java, eller inte hello JavaScript Node.js-miljö. tooavoid hello problem med att använda API: erna girig, vi bort dem i version 0.6.0 förhandsgranskning.

Om koden anropar dessa girig API: er:

```cpp
std::vector<azure::storage::table_entity> entities = table.execute_query(query);
for (auto it = entities.cbegin(); it != entities.cend(); ++it)
{
    process_entity(*it);
}
```

Du bör ändra koden toouse hello segmenterat visar en lista över API: er:

```cpp
azure::storage::continuation_token token;
do
{
    azure::storage::table_query_segment segment = table.execute_query_segmented(query, token);
    for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
    {
        process_entity(*it);
    }

    token = segment.continuation_token();
} while (!token.empty());
```

Genom att ange hello *max_results* parametern för hello segment du balanserar mellan hello antal begäranden och minne användning toomeet prestandaöverväganden för ditt program.

Dessutom om du använder med segmenterade lista API: er, men hello data lagras i en lokal samling i en ”girig” style, även rekommenderar vi starkt att du refactor din kod toohandle som lagrar data i en lokal insamling noggrant i större skala.

## <a name="lazy-listing"></a>Enkel lista
Girig lista aktiveras potentiella problem, är men praktiskt om hello behållaren finns för många objekt.

Om du använder också C# eller Oracle Java SDK: er, bör du känna till hello Enumerable-programmodellen, vilket ger en lazy--format som lista, där hello vid en viss förskjutning är bara hämta data om det behövs. Hej iterator-baserade mallen innehåller också ett liknande sätt i C++.

En typisk lazy lista API, med hjälp av **list_blobs** exempel ser ut så här:

```cpp
list_blob_item_iterator list_blobs() const;
```

En typisk kodstycke som använder hello lazy lista mönster kan se ut så här:

```cpp
// List blobs in hello blob container
azure::storage::list_blob_item_iterator end_of_results;
for (auto it = container.list_blobs(); it != end_of_results; ++it)
{
    if (it->is_blob())
    {
        process_blob(it->as_blob());
    }
    else
    {
        process_directory(it->as_directory());
    }
}
```

Observera att lazy lista endast är tillgängliga i synkront läge.

Jämfört med girig listan hämtar lazy lista data bara när det behövs. Under hello omfattar hämtar den data från Azure Storage endast när hello nästa iterator flyttar till nästa segment. Därför minnesanvändning kontrolleras med en begränsad storlek och hello åtgärden går snabbt.

Lazy lista API: er finns i hello Storage-klientbibliotek för C++ i version 2.2.0.

## <a name="conclusion"></a>Slutsats
I den här artikeln beskrivs vi olika överlagringar för att lista API: er för olika objekt i hello Storage-klientbibliotek för C++. toosummarize:

* Async APIs rekommenderas under scenarier med flera trådmodell.
* Segmenterade lista rekommenderas för de flesta scenarier.
* Lazy lista anges i hello-biblioteket som en lämplig omslutning i synkron scenarier.
* Girig lista rekommenderas inte och har tagits bort från hello-biblioteket.

## <a name="next-steps"></a>Nästa steg
Mer information om Azure Storage och klientbibliotek för C++ finns hello följande resurser.

* [Hur toouse Blob Storage från C++](storage-c-plus-plus-how-to-use-blobs.md)
* [Hur toouse Table Storage från C++](storage-c-plus-plus-how-to-use-tables.md)
* [Hur toouse Queue Storage från C++](storage-c-plus-plus-how-to-use-queues.md)
* [Azure Storage-klientbibliotek för C++-API-dokumentationen.](http://azure.github.io/azure-storage-cpp/)
* [Azure Storage Teamblogg](http://blogs.msdn.com/b/windowsazurestorage/)
* [Azure Storage-dokumentation](https://azure.microsoft.com/documentation/services/storage/)

