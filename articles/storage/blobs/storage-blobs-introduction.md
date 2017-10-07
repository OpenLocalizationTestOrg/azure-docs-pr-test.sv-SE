---
title: aaaIntroduction tooAzure Blob storage | Microsoft Docs
description: Introduktion tooAzure Blob storage
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
ms.date: 08/17/2017
ms.author: robinsh
ms.openlocfilehash: 3431f826ae51d42dbced084ee60f9ff70a8168d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooblob-storage"></a>Introduktion tooBlob lagring

Azure Blob storage är en tjänst för att lagra stora mängder Ostrukturerade objektdata, exempelvis text eller binära data som kan nås från var som helst i hello world via HTTP eller HTTPS. Du kan använda Blob storage tooexpose data offentligt toohello world eller toostore programdata privat.

Vanliga användningsområden för Blob Storage är:

* Betjänar bilder eller dokument direkt tooa webbläsare
* Lagra filer för distribuerad åtkomst
* Direktuppspelning av video och ljud
* Lagra data för säkerhetskopiering och återställning, haveriberedskap och arkivering
* Lagra data för analys av en tjänst som kan vara lokal eller Azure-baserad

## <a name="blob-service-concepts"></a>Blob Service-koncept

hello Blob-tjänsten innehåller hello följande komponenter:

![Blobb-arkitektur](./media/storage-blobs-introduction/blob1.png)

* **Storage-konto:** komma åt tooAzure Storage görs genom ett lagringskonto. Det här lagringskontot kan vara en **Allmänt lagringskonto** eller en **Blob storage-konto** som är specialiserat för lagring av objekt/blobbar. Mer information finns i [Om Azure Storage-konton](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

* **Behållare:** En behållare grupperar en uppsättning blobbar. Alla blobbar måste vara i en behållare. Ett konto kan innehålla ett obegränsat antal behållare. En behållare kan lagra ett obegränsat antal blobbar. Observera att hello behållarens namn måste vara gemener.

* **Blob:** en fil av valfri typ och storlek. Azure Storage erbjuder tre typer av blobbar: blockblobbar, sidblobbar och tilläggsblobbar.
  
    *Blockblobbar* lämpar sig för lagring av text eller binära filer, exempelvis dokument och mediafiler. *Tilläggsblobbar* är liknande tooblock blobbar i att de består av block, men de är optimerade för tilläggsåtgärder, så att de är väl för loggningsscenarier. En enda blockblobb kan innehålla upp too50 000 block med upp too100 MB vardera, ger en total storlek på lite över 4,75 TB (100 MB X 50 000). En enda tilläggsblobb kan innehålla upp too50 000 block med upp too4 MB vardera, för en total storlek på lite över 195 GB (4 MB X 50 000).
  
    *Sidblobbar* kan vara upp too1 TB i storlek och är bäst för frekventa läs-/ skrivåtgärder. Virtuella Azure-datorer använder sidblobbar som Operativsystemet och datadiskarna.
  
    Mer information om namngivning av behållare och blobbar finns i [Namngivning och referens av behållare, blobbar och metadata](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata).

## <a name="next-steps"></a>Nästa steg

* [Skapa ett lagringskonto](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Komma igång med Blob storage med hjälp av .NET](storage-dotnet-how-to-use-blobs.md)