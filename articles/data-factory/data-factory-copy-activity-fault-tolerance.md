---
title: "aaaAdd feltolerans i Azure Data Factory-Kopieringsaktiviteten genom att hoppa över inkompatibla rader | Microsoft Docs"
description: "Lär dig hur tooadd feltolerans i Azure Data Factory-Kopieringsaktiviteten genom att hoppa över inkompatibla rader vid kopiering"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jingwang
ms.openlocfilehash: e7cf6117655910844b292d340674d8d631450a81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-fault-tolerance-in-copy-activity-by-skipping-incompatible-rows"></a>Lägg till feltolerans i en Kopieringsaktivitet genom att hoppa över inkompatibla rader

Azure Data Factory [Kopieringsaktiviteten](data-factory-data-movement-activities.md) ger dig två sätt toohandle inkompatibla rader vid kopiering av data mellan käll- och mottagarnoderna datalager:

- Du kan avbryta och misslyckas hello kopiera aktivitet när inkompatibla data påträffades (standardinställning).
- Du kan fortsätta toocopy alla hello data genom att lägga till feltolerans och hoppar över inkompatibla datarader. Dessutom kan du logga hello inkompatibla rader i Azure Blob storage. Du kan sedan granska hello loggen toolearn hello orsaken till felet för hello, åtgärda hello data på hello-datakälla och försök hello kopieringsaktiviteten.

## <a name="supported-scenarios"></a>Scenarier som stöds
Kopieringsaktiviteten stöder tre scenarier för identifiering, hoppar över och loggning inkompatibla data:

- **Inkompatibilitet mellan hello källdatatyp och interna hello Mottagartypen**

    Till exempel: kopieringsdata från en CSV-fil i Blob storage tooa SQL-databas med en schemadefinition som innehåller tre **INT** kolumner av typen. Hej CSV-filen rader som innehåller numeriska data, till exempel `123,456,789` kopieras har toohello sink store. Dock hello rader som innehåller icke-numeriska värden, till exempel `123,456,abc` identifieras som inkompatibel och hoppas över.

- **Matchningsfel i hello antal kolumner mellan hello källa och mottagare för hello**

    Till exempel: kopieringsdata från en CSV-fil i Blob storage tooa SQL-databas med en schemadefinition som innehåller sex kolumner. hello kopierats CSV-fil som rader som innehåller sex kolumner har toohello sink store. hello CSV-filen rader som innehåller fler eller färre än sex kolumner identifieras som inkompatibel och hoppas över.

- **Primärnyckelfel när du skriver tooa relationsdatabas**

    Till exempel: kopiera data från en SQL server tooa SQL-databas. En primär nyckel har definierats i hello sink SQL-databas, men ingen primär nyckel har definierats i hello källa SQLServer. hello dupliceras rader som finns i hello källan får inte vara kopierade toohello mottagare. Kopieringsaktiviteten kopieras bara hello första raden i hello källdata till hello mottagare. hello efterföljande källraderna som innehåller hello dupliceras primärnyckelvärde identifieras som inkompatibel och hoppas över.

## <a name="configuration"></a>Konfiguration
hello innehåller följande exempel en JSON-definitionen tooconfigure hoppar över hello inkompatibla rader i en Kopieringsaktivitet:

```json
"typeProperties": {
    "source": {
        "type": "BlobSource"
    },
    "sink": {
        "type": "SqlSink",
    },         
    "enableSkipIncompatibleRow": true,           
    "redirectIncompatibleRowSettings": {
        "linkedServiceName": "BlobStorage",
        "path": "redirectcontainer/erroroutput"
    }
}
```

| Egenskap | Beskrivning | Tillåtna värden | Krävs |
| --- | --- | --- | --- |
| **enableSkipIncompatibleRow** | Aktivera hoppar inkompatibla rader under kopia eller inte. | True<br/>FALSKT (standard) | Nej |
| **redirectIncompatibleRowSettings** | En grupp egenskaper som kan anges när du vill toolog hello inkompatibla rader. | &nbsp; | Nej |
| **linkedServiceName** | hello länkade tjänsten för Azure Storage toostore hello logg som innehåller hello hoppas över rader. | hello namnet på en [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) eller [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) länkade tjänst som refererar toohello lagring-instans som du vill toouse toostore hello-loggfilen. | Nej |
| **sökväg** | hello sökväg på hello-loggfil som innehåller hello hoppas över rader. | Ange hello Blob storage sökväg som du vill toouse toolog hello inkompatibla data. Om du inte anger en sökväg skapas hello tjänst en behållare. | Nej |

## <a name="monitoring"></a>Övervakning
När hello kopieringsaktiviteten kör har slutförts kan se du hello Antal överhoppade rader i avsnittet övervakning hello:

![Övervakaren hoppas över inkompatibla rader](./media/data-factory-copy-activity-fault-tolerance/skip-incompatible-rows-monitoring.png)

Om du konfigurerar toolog hello inkompatibla rader hittar hello fil på sökvägen: `https://[your-blob-account].blob.core.windows.net/[path-if-configured]/[copy-activity-run-id]/[auto-generated-GUID].csv` i hello loggfilen du kan se hello rader som hoppas över och hello grundorsaken till hello inkompatibilitet.

Både hello ursprungliga data och hello motsvarande fel loggas i hello-filen. Ett exempel på hello loggen innehåll är följande:
```
data1, data2, data3, UserErrorInvalidDataValue,Column 'Prop_2' contains an invalid value 'data3'. Cannot convert 'data3' tootype 'DateTime'.,
data4, data5, data6, Violation of PRIMARY KEY constraint 'PK_tblintstrdatetimewithpk'. Cannot insert duplicate key in object 'dbo.tblintstrdatetimewithpk'. hello duplicate key value is (data4).
```

## <a name="next-steps"></a>Nästa steg
toolearn mer om Azure Data Factory Kopieringsaktivitet kan se [flytta data med hjälp av Kopieringsaktiviteten](data-factory-data-movement-activities.md).
