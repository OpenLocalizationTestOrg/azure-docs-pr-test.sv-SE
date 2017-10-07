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
# <a name="add-fault-tolerance-in-copy-activity-by-skipping-incompatible-rows"></a><span data-ttu-id="f767c-103">Lägg till feltolerans i en Kopieringsaktivitet genom att hoppa över inkompatibla rader</span><span class="sxs-lookup"><span data-stu-id="f767c-103">Add fault tolerance in Copy Activity by skipping incompatible rows</span></span>

<span data-ttu-id="f767c-104">Azure Data Factory [Kopieringsaktiviteten](data-factory-data-movement-activities.md) ger dig två sätt toohandle inkompatibla rader vid kopiering av data mellan käll- och mottagarnoderna datalager:</span><span class="sxs-lookup"><span data-stu-id="f767c-104">Azure Data Factory [Copy Activity](data-factory-data-movement-activities.md) offers you two ways toohandle incompatible rows when copying data between source and sink data stores:</span></span>

- <span data-ttu-id="f767c-105">Du kan avbryta och misslyckas hello kopiera aktivitet när inkompatibla data påträffades (standardinställning).</span><span class="sxs-lookup"><span data-stu-id="f767c-105">You can abort and fail hello copy activity when incompatible data is encountered (default behavior).</span></span>
- <span data-ttu-id="f767c-106">Du kan fortsätta toocopy alla hello data genom att lägga till feltolerans och hoppar över inkompatibla datarader.</span><span class="sxs-lookup"><span data-stu-id="f767c-106">You can continue toocopy all of hello data by adding fault tolerance and skipping incompatible data rows.</span></span> <span data-ttu-id="f767c-107">Dessutom kan du logga hello inkompatibla rader i Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="f767c-107">In addition, you can log hello incompatible rows in Azure Blob storage.</span></span> <span data-ttu-id="f767c-108">Du kan sedan granska hello loggen toolearn hello orsaken till felet för hello, åtgärda hello data på hello-datakälla och försök hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="f767c-108">You can then examine hello log toolearn hello cause for hello failure, fix hello data on hello data source, and retry hello copy activity.</span></span>

## <a name="supported-scenarios"></a><span data-ttu-id="f767c-109">Scenarier som stöds</span><span class="sxs-lookup"><span data-stu-id="f767c-109">Supported scenarios</span></span>
<span data-ttu-id="f767c-110">Kopieringsaktiviteten stöder tre scenarier för identifiering, hoppar över och loggning inkompatibla data:</span><span class="sxs-lookup"><span data-stu-id="f767c-110">Copy Activity supports three scenarios for detecting, skipping, and logging incompatible data:</span></span>

- <span data-ttu-id="f767c-111">**Inkompatibilitet mellan hello källdatatyp och interna hello Mottagartypen**</span><span class="sxs-lookup"><span data-stu-id="f767c-111">**Incompatibility between hello source data type and hello sink native type**</span></span>

    <span data-ttu-id="f767c-112">Till exempel: kopieringsdata från en CSV-fil i Blob storage tooa SQL-databas med en schemadefinition som innehåller tre **INT** kolumner av typen.</span><span class="sxs-lookup"><span data-stu-id="f767c-112">For example: Copy data from a CSV file in Blob storage tooa SQL database with a schema definition that contains three **INT** type columns.</span></span> <span data-ttu-id="f767c-113">Hej CSV-filen rader som innehåller numeriska data, till exempel `123,456,789` kopieras har toohello sink store.</span><span class="sxs-lookup"><span data-stu-id="f767c-113">hello CSV file rows that contain numeric data, such as `123,456,789` are copied successfully toohello sink store.</span></span> <span data-ttu-id="f767c-114">Dock hello rader som innehåller icke-numeriska värden, till exempel `123,456,abc` identifieras som inkompatibel och hoppas över.</span><span class="sxs-lookup"><span data-stu-id="f767c-114">However, hello rows that contain non-numeric values, such as `123,456,abc` are detected as incompatible and are skipped.</span></span>

- <span data-ttu-id="f767c-115">**Matchningsfel i hello antal kolumner mellan hello källa och mottagare för hello**</span><span class="sxs-lookup"><span data-stu-id="f767c-115">**Mismatch in hello number of columns between hello source and hello sink**</span></span>

    <span data-ttu-id="f767c-116">Till exempel: kopieringsdata från en CSV-fil i Blob storage tooa SQL-databas med en schemadefinition som innehåller sex kolumner.</span><span class="sxs-lookup"><span data-stu-id="f767c-116">For example: Copy data from a CSV file in Blob storage tooa SQL database with a schema definition that contains six columns.</span></span> <span data-ttu-id="f767c-117">hello kopierats CSV-fil som rader som innehåller sex kolumner har toohello sink store.</span><span class="sxs-lookup"><span data-stu-id="f767c-117">hello CSV file rows that contain six columns are copied successfully toohello sink store.</span></span> <span data-ttu-id="f767c-118">hello CSV-filen rader som innehåller fler eller färre än sex kolumner identifieras som inkompatibel och hoppas över.</span><span class="sxs-lookup"><span data-stu-id="f767c-118">hello CSV file rows that contain more or fewer than six columns are detected as incompatible and are skipped.</span></span>

- <span data-ttu-id="f767c-119">**Primärnyckelfel när du skriver tooa relationsdatabas**</span><span class="sxs-lookup"><span data-stu-id="f767c-119">**Primary key violation when writing tooa relational database**</span></span>

    <span data-ttu-id="f767c-120">Till exempel: kopiera data från en SQL server tooa SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="f767c-120">For example: Copy data from a SQL server tooa SQL database.</span></span> <span data-ttu-id="f767c-121">En primär nyckel har definierats i hello sink SQL-databas, men ingen primär nyckel har definierats i hello källa SQLServer.</span><span class="sxs-lookup"><span data-stu-id="f767c-121">A primary key is defined in hello sink SQL database, but no such primary key is defined in hello source SQL server.</span></span> <span data-ttu-id="f767c-122">hello dupliceras rader som finns i hello källan får inte vara kopierade toohello mottagare.</span><span class="sxs-lookup"><span data-stu-id="f767c-122">hello duplicated rows that exist in hello source cannot be copied toohello sink.</span></span> <span data-ttu-id="f767c-123">Kopieringsaktiviteten kopieras bara hello första raden i hello källdata till hello mottagare.</span><span class="sxs-lookup"><span data-stu-id="f767c-123">Copy Activity copies only hello first row of hello source data into hello sink.</span></span> <span data-ttu-id="f767c-124">hello efterföljande källraderna som innehåller hello dupliceras primärnyckelvärde identifieras som inkompatibel och hoppas över.</span><span class="sxs-lookup"><span data-stu-id="f767c-124">hello subsequent source rows that contain hello duplicated primary key value are detected as incompatible and are skipped.</span></span>

## <a name="configuration"></a><span data-ttu-id="f767c-125">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="f767c-125">Configuration</span></span>
<span data-ttu-id="f767c-126">hello innehåller följande exempel en JSON-definitionen tooconfigure hoppar över hello inkompatibla rader i en Kopieringsaktivitet:</span><span class="sxs-lookup"><span data-stu-id="f767c-126">hello following example provides a JSON definition tooconfigure skipping hello incompatible rows in Copy Activity:</span></span>

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

| <span data-ttu-id="f767c-127">Egenskap</span><span class="sxs-lookup"><span data-stu-id="f767c-127">Property</span></span> | <span data-ttu-id="f767c-128">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f767c-128">Description</span></span> | <span data-ttu-id="f767c-129">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="f767c-129">Allowed values</span></span> | <span data-ttu-id="f767c-130">Krävs</span><span class="sxs-lookup"><span data-stu-id="f767c-130">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="f767c-131">**enableSkipIncompatibleRow**</span><span class="sxs-lookup"><span data-stu-id="f767c-131">**enableSkipIncompatibleRow**</span></span> | <span data-ttu-id="f767c-132">Aktivera hoppar inkompatibla rader under kopia eller inte.</span><span class="sxs-lookup"><span data-stu-id="f767c-132">Enable skipping incompatible rows during copy or not.</span></span> | <span data-ttu-id="f767c-133">True</span><span class="sxs-lookup"><span data-stu-id="f767c-133">True</span></span><br/><span data-ttu-id="f767c-134">FALSKT (standard)</span><span class="sxs-lookup"><span data-stu-id="f767c-134">False (default)</span></span> | <span data-ttu-id="f767c-135">Nej</span><span class="sxs-lookup"><span data-stu-id="f767c-135">No</span></span> |
| <span data-ttu-id="f767c-136">**redirectIncompatibleRowSettings**</span><span class="sxs-lookup"><span data-stu-id="f767c-136">**redirectIncompatibleRowSettings**</span></span> | <span data-ttu-id="f767c-137">En grupp egenskaper som kan anges när du vill toolog hello inkompatibla rader.</span><span class="sxs-lookup"><span data-stu-id="f767c-137">A group of properties that can be specified when you want toolog hello incompatible rows.</span></span> | &nbsp; | <span data-ttu-id="f767c-138">Nej</span><span class="sxs-lookup"><span data-stu-id="f767c-138">No</span></span> |
| <span data-ttu-id="f767c-139">**linkedServiceName**</span><span class="sxs-lookup"><span data-stu-id="f767c-139">**linkedServiceName**</span></span> | <span data-ttu-id="f767c-140">hello länkade tjänsten för Azure Storage toostore hello logg som innehåller hello hoppas över rader.</span><span class="sxs-lookup"><span data-stu-id="f767c-140">hello linked service of Azure Storage toostore hello log that contains hello skipped rows.</span></span> | <span data-ttu-id="f767c-141">hello namnet på en [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) eller [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) länkade tjänst som refererar toohello lagring-instans som du vill toouse toostore hello-loggfilen.</span><span class="sxs-lookup"><span data-stu-id="f767c-141">hello name of an [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) or [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) linked service, which refers toohello storage instance that you want toouse toostore hello log file.</span></span> | <span data-ttu-id="f767c-142">Nej</span><span class="sxs-lookup"><span data-stu-id="f767c-142">No</span></span> |
| <span data-ttu-id="f767c-143">**sökväg**</span><span class="sxs-lookup"><span data-stu-id="f767c-143">**path**</span></span> | <span data-ttu-id="f767c-144">hello sökväg på hello-loggfil som innehåller hello hoppas över rader.</span><span class="sxs-lookup"><span data-stu-id="f767c-144">hello path of hello log file that contains hello skipped rows.</span></span> | <span data-ttu-id="f767c-145">Ange hello Blob storage sökväg som du vill toouse toolog hello inkompatibla data.</span><span class="sxs-lookup"><span data-stu-id="f767c-145">Specify hello Blob storage path that you want toouse toolog hello incompatible data.</span></span> <span data-ttu-id="f767c-146">Om du inte anger en sökväg skapas hello tjänst en behållare.</span><span class="sxs-lookup"><span data-stu-id="f767c-146">If you do not provide a path, hello service creates a container for you.</span></span> | <span data-ttu-id="f767c-147">Nej</span><span class="sxs-lookup"><span data-stu-id="f767c-147">No</span></span> |

## <a name="monitoring"></a><span data-ttu-id="f767c-148">Övervakning</span><span class="sxs-lookup"><span data-stu-id="f767c-148">Monitoring</span></span>
<span data-ttu-id="f767c-149">När hello kopieringsaktiviteten kör har slutförts kan se du hello Antal överhoppade rader i avsnittet övervakning hello:</span><span class="sxs-lookup"><span data-stu-id="f767c-149">After hello copy activity run completes, you can see hello number of skipped rows in hello monitoring section:</span></span>

![Övervakaren hoppas över inkompatibla rader](./media/data-factory-copy-activity-fault-tolerance/skip-incompatible-rows-monitoring.png)

<span data-ttu-id="f767c-151">Om du konfigurerar toolog hello inkompatibla rader hittar hello fil på sökvägen: `https://[your-blob-account].blob.core.windows.net/[path-if-configured]/[copy-activity-run-id]/[auto-generated-GUID].csv` i hello loggfilen du kan se hello rader som hoppas över och hello grundorsaken till hello inkompatibilitet.</span><span class="sxs-lookup"><span data-stu-id="f767c-151">If you configure toolog hello incompatible rows, you can find hello log file at this path: `https://[your-blob-account].blob.core.windows.net/[path-if-configured]/[copy-activity-run-id]/[auto-generated-GUID].csv` In hello log file, you can see hello rows that were skipped and hello root cause of hello incompatibility.</span></span>

<span data-ttu-id="f767c-152">Både hello ursprungliga data och hello motsvarande fel loggas i hello-filen.</span><span class="sxs-lookup"><span data-stu-id="f767c-152">Both hello original data and hello corresponding error are logged in hello file.</span></span> <span data-ttu-id="f767c-153">Ett exempel på hello loggen innehåll är följande:</span><span class="sxs-lookup"><span data-stu-id="f767c-153">An example of hello log file content is as follows:</span></span>
```
data1, data2, data3, UserErrorInvalidDataValue,Column 'Prop_2' contains an invalid value 'data3'. Cannot convert 'data3' tootype 'DateTime'.,
data4, data5, data6, Violation of PRIMARY KEY constraint 'PK_tblintstrdatetimewithpk'. Cannot insert duplicate key in object 'dbo.tblintstrdatetimewithpk'. hello duplicate key value is (data4).
```

## <a name="next-steps"></a><span data-ttu-id="f767c-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f767c-154">Next steps</span></span>
<span data-ttu-id="f767c-155">toolearn mer om Azure Data Factory Kopieringsaktivitet kan se [flytta data med hjälp av Kopieringsaktiviteten](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="f767c-155">toolearn more about Azure Data Factory Copy Activity, see [Move data by using Copy Activity](data-factory-data-movement-activities.md).</span></span>
