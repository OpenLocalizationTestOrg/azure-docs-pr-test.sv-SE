---
title: "aaaCreate export jobb för Azure Import/Export | Microsoft Docs"
description: "Lär dig hur toocreate export jobbet för hello Microsoft Azure Import/Export service."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 613d480b-a8ef-4b28-8f54-54174d59b3f4
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 4a10b42cc86dbf3bcea3a515bc065e2259228ef9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="creating-an-export-job-for-hello-azure-importexport-service"></a>Skapa ett exportjobb för hello Azure Import/Export service
Skapa ett exportjobb för hello Microsoft Azure Import/Export-tjänsten använder hello REST API omfattar hello följande steg:

-   Att välja hello-blobbar tooexport.

-   Hämta en leveransplats.

-   Skapa hello exportjobb.

-   Leverera din tomma enheter tooMicrosoft via en stöds operatör-tjänst.

-   Uppdaterar hello exportjobb med hello Paketinformation.

-   Ta emot hello enheter tillbaka från Microsoft.

 Se [med hello Windows Azure Import/Export service tooTransfer Data tooBlob lagring](storage-import-export-service.md) för en översikt över hello Import/Export service och en genomgång som visar hur toouse hello [Azure-portalen](https://portal.azure.com/) toocreate och hantera importera och exportera jobben.

## <a name="selecting-blobs-tooexport"></a>Att välja blobbar tooexport
 toocreate ett exportjobb måste tooprovide en lista över blobbar som du vill tooexport från ditt lagringskonto. Det finns ett par sätt tooselect blobbar toobe exporteras:

-   Du kan använda en relativ blob sökvägen tooselect en enda blob och alla dess ögonblicksbilder.

-   Du kan använda en relativ blob sökvägen tooselect en enda blob undantag av dess ögonblicksbilder.

-   Du kan använda en relativ blob-sökväg och en ögonblicksbild tid tooselect en ögonblicksbild.

-   Du kan använda en blob-prefixet tooselect alla blobbar och ögonblicksbilder med hello angivna prefix.

-   Du kan exportera alla blobbar och ögonblicksbilder i hello storage-konto.

 Mer information om att ange-blobbar tooexport, finns hello [placera jobbet](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) igen.

## <a name="obtaining-your-shipping-location"></a>Hämta din leverans plats
Innan du skapar ett exportjobb du behöver tooobtain en leverans namn och adress genom att anropa hello [få plats](https://portal.azure.com) eller [listan platser](/rest/api/storageimportexport/listlocations) igen. `List Locations`Returnerar en lista över platser och deras e-postadresser. Du kan välja en plats från hello returnerade lista och leverera din hårddiskar toothat adress. Du kan också använda hello `Get Location` åtgärden tooobtain hello leveransadress för en specifik plats direkt.

Så hello under tooobtain hello leverans platsen:

-   Identifiera hello namn hello platsen för ditt lagringskonto. Det här värdet kan hittas under hello **plats** på hello lagringskontots **instrumentpanelen** i hello klassisk portal eller efterfrågade för med hello service management API-åtgärd [hämta Lagringskontoegenskaperna](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties).

-   Hämta hello platsen som är tillgängliga tooprocess det här lagringskontot genom att anropa hello `Get Location` igen.

-   Om hello `AlternateLocations` egenskapen hello plats innehåller själva hello platsen så är det bra toouse den här platsen. Annars kan anropa hello `Get Location` igen med en av hello alternativa platser. hello ursprungsplatsen kan tillfälligt stängt för underhåll.

## <a name="creating-hello-export-job"></a>Skapa hello exportjobb
 toocreate hello exportjobb, anrop hello [placera jobbet](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) igen. Du behöver tooprovide hello följande information:

-   Ett namn för hello jobbet.

-   Hej lagringskontonamn.

-   hello leverans platsnamn, fick i hello föregående steg.

-   En jobbtyp (exportera).

-   hello avsändaradress där hello enheter som ska skickas när hello exportjobb har slutförts.

-   hello lista över BLOB (eller blob-prefix) toobe exporteras.

## <a name="shipping-your-drives"></a>Leverera dina enheter
 Sedan använder hello Azure Import/Export verktyget toodetermine hello antalet enheter som du behöver toosend, baserat på hello blob som du har valt toobe exporteras och hello enhetsstorlek. Se hello [referens för Azure Import/Export verktyg](storage-import-export-tool-how-to-v1.md) mer information.

 Paketet hello enheter i en enda paketera och leverera dem toohello adress som du fått i hello tidigare steg. Observera hello spåra antalet paketet för hello nästa steg.

> [!NOTE]
>  Du måste leverera enheter via en stöds operatör-tjänst som tillhandahåller ett spårningsnummer för paketet.

## <a name="updating-hello-export-job-with-your-package-information"></a>Uppdaterar hello exportjobb med Paketinformation
 När du har Spårningsnumret till din kan anropa hello [uppdatering jobbegenskaper](/rest/api/storageimportexport/jobs#Jobs_Update) operatör åtgärdsnamn tooupdated hello och spårnings-ID för hello jobbet. Alternativt kan du ange hello antalet enheter, hello avsändaradress och hello leverans samt datum.

## <a name="receiving-hello-package"></a>Mottagande hello-paket
 När din exportjobb har bearbetats returnerade enheter tooyou med krypterade data. Du kan hämta hello BitLocker-nyckel för varje hello enheter genom att anropa hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) igen. Sedan kan du låsa upp hello-enhet med hjälp av hello nyckel. hello enhet manifestfilen på varje enhet innehåller hello lista över filer på hello enhet samt hello ursprungliga blob-adressen för varje fil.

## <a name="next-steps"></a>Nästa steg

* [Med hjälp av hello Import/Export service REST API](storage-import-export-using-the-rest-api.md)
