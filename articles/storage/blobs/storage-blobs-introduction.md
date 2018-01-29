---
title: Introduktion till Azure Blob storage | Microsoft Docs
description: Introduktion till Azure Blob storage
services: storage
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/28/2017
ms.author: tamram
ms.openlocfilehash: 0097f1c02b88343a135b6489130a6e0d35cf6fba
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/23/2018
---
# <a name="introduction-to-blob-storage"></a>Introduktion till Blob storage

Azure Blob Storage är en tjänst för att lagra stora mängder ostrukturerad objektdata, exempelvis text eller binär data som kan nås från var som helst i världen via HTTP eller HTTPS. Du kan använda Blob Storage för att exponera data offentligt eller lagra programdata privat.

Vanliga användningsområden för Blob Storage är:

* Leverera bilder eller dokument direkt till en webbläsare
* Lagra filer för distribuerad åtkomst
* Direktuppspelning av video och ljud
* Lagra data för säkerhetskopiering och återställning, haveriberedskap och arkivering
* Lagra data för analys av en tjänst som kan vara lokal eller Azure-baserad

## <a name="blob-service-concepts"></a>Blob Service-koncept

Blob Service innehåller följande komponenter:

![Blobb-arkitektur](./media/storage-blobs-introduction/blob1.png)

* **Lagringskonto:** All åtkomst till Azure Storage görs genom ett lagringskonto. Det här lagringskontot kan vara en **Allmänt lagringskonto (v1 eller v2)** eller **Blob storage-konton**. Mer information finns i [Om Azure Storage-konton](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

* **Behållare:** En behållare grupperar en uppsättning blobbar. Alla blobbar måste vara i en behållare. Ett konto kan innehålla ett obegränsat antal behållare. En behållare kan lagra ett obegränsat antal blobbar. Observera att behållarens namn får innehålla endast gemener.

* **Blob:** en fil av valfri typ och storlek. Azure Storage erbjuder tre typer av blobbar: blockblobbar, [sidblobbar](storage-blob-pageblob-overview.md), och tilläggsblobar.
  
    *Blockblobbar* lämpar sig för lagring av text eller binära filer, exempelvis dokument och mediafiler. *Tilläggsblobbar* liknar blockblobbar i det avseendet att de består av block, men de är optimerade för tilläggsåtgärder, så de lämpar sig väl för loggningsscenarier. En enda blockblobb kan innehålla upp till 50 000 block med upp till 100 MB vardera, vilket ger en total storlek på lite över 4,75 TB (100 MB X 50 000). En enda tilläggsblobb kan innehålla upp till 50 000 block med upp till 4 MB vardera, vilket ger en total storlek på lite över 195 GB (4 MB X 50 000).
  
    *Sidblobbar* kan vara upp till 8 TB i storlek och är bäst för frekventa läs-/ skrivåtgärder. Azure Virtual Machines använder sig av sidblobbar som operativsystem- och datadiskar.
  
    Mer information om namngivning av behållare och blobbar finns i [Namngivning och referens av behållare, blobbar och metadata](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata).

## <a name="next-steps"></a>Nästa steg

* [Skapa ett lagringskonto](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Komma igång med Blob storage med hjälp av .NET](storage-dotnet-how-to-use-blobs.md)
* [Azure Storage-exempel med hjälp av .NET](../common/storage-samples-dotnet.md)
* [Azure Storage-exempel som använder Java](../common/storage-samples-java.md)
