---
title: "aaaDeciding när toouse Azure BLOB-objekt, Azure-filer eller Azure Datadiskar"
description: "Läs mer om hello olika sätt toostore och komma åt data i Azure toohelp du bestämma vilka toouse teknik."
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: robinsh
ms.openlocfilehash: 6109affe41e98ed459616a4f91064ded0c74428d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deciding-when-toouse-azure-blobs-azure-files-or-azure-data-disks"></a>Bestämmer när toouse Azure BLOB-objekt, Azure-filer eller Azure Datadiskar

Microsoft Azure tillhandahåller flera funktioner i Azure Storage för att lagra och komma åt dina data i hello molnet. Den här artikeln täcker Azure-filer, Blobbar och Datadiskar och är utformad toohelp som du väljer mellan dessa funktioner.

## <a name="scenarios"></a>Scenarier

hello följande tabell jämförs filer, Blobbar och Datadiskar och visar exempelscenarier som är lämpliga för varje.

| Funktion | Beskrivning | När toouse |
|--------------|-------------|-------------|
| **Azure Files** | Tillhandahåller ett gränssnitt för SMB, klientbiblioteken, och en [REST-gränssnittet](/rest/api/storageservices/file-service-rest-api) som tillåter åtkomst överallt toostored filer. | Du vill för ”lyfta och flytta” ett program toohello moln som redan använder hello inbyggda system-API: er tooshare fildata mellan den och andra program som körs i Azure.<br/><br/>Vill du toostore utvecklings- och felsökningsverktyg som behöver toobe nås från många virtuella datorer. |
| **Azure BLOB** | Ger klientbibliotek och en [REST-gränssnittet](/rest/api/storageservices/blob-service-rest-api) som gör det möjligt för Ostrukturerade data för lagras och komma åt i massiv skala i blockblobbar. | Du vill att programmet toosupport strömning och direktåtkomst scenarier.<br/><br/>Vill du toobe kan tooaccess programdata från var som helst. |
| **Azure Datadiskar** | Ger klientbibliotek och en [REST-gränssnittet](/rest/api/compute/virtualmachines/virtualmachines-create-or-update) som gör att data toobe beständigt lagras och nås från en ansluten virtuell hårddisk. | Du vill toolift och flytta program som använder interna filen system-API: er tooread och skriva toopersistent datadiskar.<br/><br/>Vill du toostore data som inte är nödvändiga toobe nås från utanför hello virtuella toowhich hello disken är ansluten. |

## <a name="comparison-files-and-blobs"></a>Jämförelse: Filer och Blobbar

hello följande tabell jämförs Azure-filer med Azure BLOB.  
  
||||  
|-|-|-|  
|**Attributet**|**Azure BLOB**|**Azure Files**|  
|Alternativ för hållbarhet|LRS-, ZRS-, GRS (och RA-GRS för högre tillgänglighet)|LRS GRS|  
|Hjälpmedel|REST API:er|REST API:er<br /><br /> SMB 2.1 och SMB 3.0 (standard filsystem API: er)|  
|Anslutning|REST-API: er – globalt|REST API: er – globalt<br /><br /> SMB 2.1 – inom region<br /><br /> SMB 3.0 – globalt|  
|Slutpunkter|`http://myaccount.blob.core.windows.net/mycontainer/myblob`|`\\myaccount.file.core.windows.net\myshare\myfile.txt`<br /><br /> `http://myaccount.file.core.windows.net/myshare/myfile.txt`|  
|Kataloger|Flat namnområde|True katalogobjekt|  
|Skiftlägeskänslighet namn|Versaler och gemener|Skilftlägeskänsliga, men case bevarar|  
|Kapacitet|Konfigurera too500 TB behållare|5 TB filresurser|  
|Dataflöde|Konfigurera too60 MB/s per blockblob|Konfigurera too60 MB/s per resurs|  
|Objektstorlek|Konfigurera too200 GB/blockblob|Konfigurera too1TB-fil|  
|Fakturerade kapacitet|Baserat på skrivna byte|Baserat på filstorlek|  
|Klientbibliotek|Flera språk|Flera språk|  
  
## <a name="comparison-files-and-data-disks"></a>Jämförelse: Filer och Datadiskar

Azure Files kompletterar Azure Datadiskar. En datadisk endast kan bifogade tooone Azure-dator i taget. Datadiskar är fast format virtuella hårddiskar lagrade som sidblobbar i Azure Storage och används av hello virtuella toostore beständiga data. Filresurser i Azure-filer kan nås hello samma sätt som hello lokal disk används (med hjälp av filsystem API: er) för i och kan delas mellan flera virtuella datorer.  
 
hello följande tabell jämförs Azure-filer med Azure Datadiskar.  
 
||||  
|-|-|-|  
|**Attributet**|**Azure Datadiskar**|**Azure Files**|  
|Omfång|Exklusiv tooa enskild virtuell dator|Delad åtkomst över flera virtuella datorer|  
|Ögonblicksbilder och kopiera|Ja|Nej|  
|Konfiguration|Ansluten vid start av hello virtuell dator|Ansluten efter hello virtuella datorn har startats|  
|Autentisering|Inbyggd|Ställ in med net use|  
|Rensa|Automatisk|Manuell|  
|Åtkomst med hjälp av REST|Filer i hello VHD kan inte nås|Filer som lagras på en resurs kan nås|  
|Max storlek|Disk 1 TB|5 TB-filresurs och 1 TB fil i resursen|  
|Maximalt antal 8KB IOps|500 IOps|1000 IOps|  
|Dataflöde|Konfigurera too60 MB/s per Disk|Konfigurera too60 MB/s per resurs|  

## <a name="next-steps"></a>Nästa steg

När de fattar beslut om hur data lagras och komma åt bör det också övervägas hello kostnader ingår. Mer information finns i [priser för Azure Storage](https://azure.microsoft.com/pricing/details/storage/).
  
Vissa SMB-funktioner är inte tillämplig toohello moln. Mer information finns i [funktioner som inte stöds av hello Azure File service](/rest/api/storageservices/features-not-supported-by-the-azure-file-service).
  
Läs mer om datadiskar [hantera diskar och bilder](storage-about-disks-and-vhds-linux.md) och [hur tooAttach en datadisk tooa Windows-dator](../virtual-machines/windows/classic/attach-disk.md).
