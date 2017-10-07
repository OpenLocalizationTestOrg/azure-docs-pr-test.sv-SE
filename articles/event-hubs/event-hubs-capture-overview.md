---
title: aaaOverview av Azure Event Hubs avbilda | Microsoft Docs
description: Samla in telemetridata med Event Hubs avbilda
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: e53cdeea-8a6a-474e-9f96-59d43c0e8562
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: sethm;darosa
ms.openlocfilehash: 0238cae712a0ed7bdf3e87ee93a069a553cb65df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-event-hubs-capture"></a>Azure Event Hubs avbildning

Azure Event Hubs avbilda kan du tooautomatically leverera hello strömmande data i Händelsehubbar tooan [Azure Blob storage](https://azure.microsoft.com/services/storage/blobs/) eller [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) konto önskat med hello extra flexibilitet Ange ett intervall för tid, eller storlek. Ställa in avbildningen är snabb, det finns inga administrativa kostnader toorun den och skalar automatiskt med Händelsehubbar [genomflödesenheter](event-hubs-features.md#capacity). Event Hubs avbilda är hello enklaste sättet tooload strömmande data till Azure och aktiverar toofocus databehandling i stället för datainsamling.

Event Hubs avbilda kan du tooprocess i realtid och batch-baserade pipelines på hello samma dataström. Det innebär att du kan skapa lösningar som växa med dina behov över tid. Om du utvecklar batch-baserade system idag med ett öga mot framtida realtidsbearbetning eller om du vill tooadd en effektiv cold sökvägen tooan befintlig realtid lösning, gör Event Hubs avbilda arbeta med strömmande data enklare.

## <a name="how-event-hubs-capture-works"></a>Så här fungerar Event Hubs avbilda

Händelsehubbar är en tid kvarhållning varaktiga buffert för telemetriingång, liknande tooa distribuerade loggen. hello viktiga tooscaling i Händelsehubbar är hello [konsumentmönster indelat modellen](event-hubs-features.md#partitions). Varje partition är en oberoende segment av data och används oberoende av varandra. Över tiden data åldras ut, baserat på hello konfigurerbara kvarhållningsperioden. Därför det finns en given händelsehubb för aldrig hämtar ”full”.

Event Hubs avbilda kan du toospecify din egen Azure Blob storage-konto och en behållare eller ett Azure Data Lake Store-konto, vilket används toostore hello infångade data. Dessa konton kan ha hello samma region som din händelsehubb eller i en annan region, lägger till toohello flexibilitet för hello Event Hubs avbilda funktion.

Insamlade data skrivs i [Apache Avro] [ Apache Avro] format: ett compact, snabb, binära format som ger omfattande datastrukturer inlineschema. Det här formatet används ofta i hello Hadoop-ekosystemet, Stream Analytics och Azure Data Factory. Mer information om hur du arbetar med Avro finns senare i den här artikeln.

### <a name="capture-windowing"></a>Avbilda fönsterhantering

Event Hubs avbilda gör tooset upp en avbildning av fönstret toocontrol. Det här fönstret är en minsta storlek och konfiguration med en ”första WINS-princip”, vilket innebär en capture-åtgärd som hello första utlösaren påträffade orsaker. Om du har en femton minuter fönstret capture 100 MB och skicka 1 MB per sekund, hello storlek fönstret utlösare innan hello tidsperioden. Varje partition in separat och skriver slutförda blockblob när hello avbildning för hello tid vid vilken hello avbilda intervall påträffades. hello namngivningskonvention för lagring är följande:

```
[namespace]/[event hub]/[partition]/[YYYY]/[MM]/[DD]/[HH]/[mm]/[ss]
```

### <a name="scaling-toothroughput-units"></a>Skalning toothroughput enheter

Event Hubs trafik styrs av [genomflödesenheter](event-hubs-features.md#capacity). En genomflödesenhet kan 1 MB per sekund eller 1 000 händelser per sekund på inkommande trafik och två gånger att mängden utgång. Standard Händelsehubbar kan konfigureras med 1-20 genomflödesenheter och du kan köpa fler med en kvot öka [supportbegäran][support request]. Användning utöver din inköpta genomflödesenheter begränsas. Event Hubs avbilda kopierar data direkt från hello intern Händelsehubbar lagring, kringgå genomströmning enhet utgång kvoter och spara din utgång för andra bearbetning, till exempel Stream Analytics eller Spark.

När du konfigurerat Event Hubs avbilda körs automatiskt när du skickar din första händelsen och fortsätter att köras. toomake underlättar för dina bearbetningen nedströms tooknow som hello processen fungerar Händelsehubbar skriver tomma filer när det finns inga data. Den här processen ger en förutsägbar takt och markör som kan mata batch-processorer.

## <a name="setting-up-event-hubs-capture"></a>Konfigurera Event Hubs avbilda

Du kan konfigurera avbilda vid hello event hub skapandet med hello [Azure-portalen](https://portal.azure.com), eller med hjälp av Azure Resource Manager-mallar. Mer information finns i följande artiklar hello:

- [Aktivera Event Hubs hämtar med hello Azure-portalen](event-hubs-capture-enable-through-portal.md)
- [Skapa ett namnområde för Händelsehubbar med en händelsehubb och aktivera avbildning med en Azure Resource Manager-mall](event-hubs-resource-manager-namespace-event-hub-enable-capture.md)

## <a name="exploring-hello-captured-files-and-working-with-avro"></a>Utforska hello avbildas filer och arbeta med Avro

Event Hubs avbilda skapar filer i Avro-format, som anges på hello konfigurerade tidsperioden. Du kan visa dessa filer i ett verktyg som [Azure Lagringsutforskaren][Azure Storage Explorer]. Du kan hämta hello filer lokalt toowork på dem.

hello-filer som skapas av Event Hubs avbilda har hello följande Avro-schemat:

![][3]

Ett enkelt sätt tooexplore Avro-filernas är med hjälp av hello [Avro verktyg] [ Avro Tools] jar från Apache. Du kan se hello schemat för en viss Avro-fil genom att köra följande kommando hello när du har hämtat den här jar:

```
java -jar avro-tools-1.8.2.jar getschema <name of capture file>
```

Det här kommandot returnerar

```
{

    "type":"record",
    "name":"EventData",
    "namespace":"Microsoft.ServiceBus.Messaging",
    "fields":[
                 {"name":"SequenceNumber","type":"long"},
                 {"name":"Offset","type":"string"},
                 {"name":"EnqueuedTimeUtc","type":"string"},
                 {"name":"SystemProperties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Properties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Body","type":["null","bytes"]}
             ]
}
```

Du kan också använder Avro verktyg tooconvert hello loggfilsformat tooJSON och utföra andra bearbetning.

tooperform mer avancerade bearbetning, hämta och installera Avro för valet av plattform. När hello detta skrivs, det finns implementeringar för C, C++, C\#, Java, NodeJS, Perl, PHP, Python eller Ruby.

Apache Avro har slutförts komma igång guider för [Java] [ Java] och [Python][Python]. Du kan också läsa hello [komma igång med Event Hubs avbilda](event-hubs-capture-python.md) artikel.

## <a name="how-event-hubs-capture-is-charged"></a>Hur Event Hubs avbilda debiteras

Event Hubs avbilda mäts på samma sätt toothroughput enheter: som timkostnaden. hello är proportionell toohello antalet genomflödesenheter för hello namnområde. Genomflödesenheter ökas och minskas, Event Hubs avbilda mätare öka och minska tooprovide matchar prestanda. hello mätare uppstå tillsammans. Information om priser, se [priser för Händelsehubbar](https://azure.microsoft.com/pricing/details/event-hubs/). 

## <a name="next-steps"></a>Nästa steg

Event Hubs avbilda är hello enklaste sättet tooget data till Azure. Med Azure Data Lake och Azure Data Factory Azure HDInsight kan du utföra batchbearbetning och andra analytics med hjälp av välbekanta verktyg och plattformar du väljer, skaländras du behöver.

Mer information om Händelsehubbar genom att besöka hello följande länkar:

* [Börja skicka och ta emot händelser](event-hubs-dotnet-framework-getstarted-send.md)
* Ett komplett [exempelprogram som använder händelsehubbar][sample application that uses Event Hubs]
* [Översikt av händelsehubbar][Event Hubs overview]

[Apache Avro]: http://avro.apache.org/
[support request]: https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[Azure Storage Explorer]: http://azurestorageexplorer.codeplex.com/
[3]: ./media/event-hubs-capture-overview/event-hubs-capture3.png
[Avro Tools]: http://www-us.apache.org/dist/avro/avro-1.8.2/java/avro-tools-1.8.2.jar
[Java]: http://avro.apache.org/docs/current/gettingstartedjava.html
[Python]: http://avro.apache.org/docs/current/gettingstartedpython.html
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
[sample application that uses Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Scale out Event Processing with Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
