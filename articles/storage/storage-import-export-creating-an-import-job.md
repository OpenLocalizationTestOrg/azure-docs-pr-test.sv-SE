---
title: "aaaCreate ett importjobb för Azure Import/Export | Microsoft Docs"
description: "Lär dig hur toocreate en import för hello Microsoft Azure Import/Export service."
author: muralikk
manager: syadav
editor: syadav
services: storage
documentationcenter: 
ms.assetid: 8b886e83-6148-4149-9d0f-5d48ec822475
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: da974c33a3688bb5e2412c8bfcbeca704096c2fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="creating-an-import-job-for-hello-azure-importexport-service"></a>Skapa ett importjobb för hello Azure Import/Export service

Skapa ett importjobb för hello Microsoft Azure Import/Export-tjänsten använder hello REST API omfattar hello följande steg:

-   Förbereda enheter med hello Azure Import/Export-verktyget.

-   Hämta hello plats toowhich tooship hello enhet.

-   Skapa hello importjobbet.

-   Leverera hello enheter tooMicrosoft via en stöds operatör-tjänst.

-   Uppdaterar hello importjobbet med hello leveransinformation.

 Se [med hello Microsoft Azure Import/Export service tooTransfer Data tooBlob lagring](storage-import-export-service.md) för en översikt över hello Import/Export service och en genomgång som visar hur toouse hello [Azure-portalen](https://portal.azure.com/) toocreate och hantera importera och exportera jobben.

## <a name="preparing-drives-with-hello-azure-importexport-tool"></a>Förbereda enheter med hello Azure Import/Export-verktyget

hello steg tooprepare enheter för importen är hello samma om du skapar hello jobvia hello portalen eller via hello REST API.

Nedan finns en kort översikt över förberedelse av enheten. Se toohello [Azure Import ExportTool referens](storage-import-export-tool-how-to-v1.md) fullständiga instruktioner. Du kan hämta hello Azure Import/Export verktyget [här](http://go.microsoft.com/fwlink/?LinkID=301900).

Förbereda enheten omfattar:

-   Identifiera hello data toobe importeras.

-   Identifiera hello mål blobbar i Windows Azure Storage.

-   Använder hello Azure Import/Export verktyget toocopy data-tooone eller hårddiskar.

 hello Azure Import/Export verktyget skapar också en manifestfil för varje hello enheter som den är redo. En manifestfil innehåller:

-   En uppräkning av alla hello-filer som är avsedd för överföringen och hello mappningar från dessa filer tooblobs.

-   Kontrollsummor för hello segment för varje fil.

-   Information om hello metadata och egenskaper tooassociate med varje blob.

-   En lista över hello åtgärd tootake om en blob som överförs har hello samma namn som en befintlig blob i hello behållaren. Möjliga alternativ är: a) över hello blob med hello-fil, b) skydda hello befintlig blob och hoppa över överföring hello-fil, c) lägga till ett suffix toohello namn så att den inte står i konflikt med andra filer.

## <a name="obtaining-your-shipping-location"></a>Hämta din leverans plats

Innan du skapar ett importjobb du behöver tooobtain en leverans namn och adress genom att anropa hello [listan platser](/rest/api/storageimportexport/listlocations) igen. `List Locations`Returnerar en lista över platser och deras e-postadresser. Du kan välja en plats från hello returnerade lista och leverera din hårddiskar toothat adress. Du kan också använda hello `Get Location` åtgärden tooobtain hello leveransadress för en specifik plats direkt.

 Så hello under tooobtain hello leverans platsen:

-   Identifiera hello namn hello platsen för ditt lagringskonto. Det här värdet kan hittas under hello **plats** på hello lagringskontots **instrumentpanelen** i hello Azure portal eller efterfrågade för med hello service management API-åtgärd [hämta lagring Kontot egenskaper](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties).

-   Hämta hello-plats som är tillgängliga tooprocess det här lagringskontot genom att anropa hello `Get Location` igen.

-   Om hello `AlternateLocations` egenskapen hello plats innehåller själva hello platsen så är det bra toouse den här platsen. Annars kan anropa hello `Get Location` igen med en av hello alternativa platser. hello ursprungsplatsen kan tillfälligt stängt för underhåll.

## <a name="creating-hello-import-job"></a>Skapa hello-importjobb
toocreate hello importjobbet, anrop hello [placera jobbet](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) igen. Du behöver tooprovide hello följande information:

-   Ett namn för hello jobbet.

-   Hej lagringskontonamn.

-   hello leverans platsnamn, hämtas från hello föregående steg.

-   En jobbtyp (importera).

-   hello avsändaradress där hello enheter som ska skickas när hello importjobbet har slutförts.

-   hello lista över enheter i hello-jobbet. För varje enhet, måste du inkludera hello följande information som erhölls vid förberedelsen för hello enhet:

    -   hello enhet Id

    -   hello BitLocker-nyckel

    -   hello manifestfilen relativ sökväg på hello hårddisken

    -   Hej Base16 kodad manifestfilen MD5-hash

## <a name="shipping-your-drives"></a>Leverera dina enheter
Du måste leverera din enheter toohello adress som du fick från hello föregående steg och du måste ange hello Import/Export service med hello spåra antalet hello-paketet.

> [!NOTE]
>  Du måste leverera enheter via en stöds operatör-tjänst som tillhandahåller ett spårningsnummer för paketet.

## <a name="updating-hello-import-job-with-your-shipping-information"></a>Uppdaterar hello importjobbet med leveransinformation
När du har Spårningsnumret till din kan anropa hello [uppdatering jobbegenskaper](/api/storageimportexport/jobs#Jobs_Update) åtgärden tooupdate hello leverans operatör namn, hello Spårningsnumret för hello jobbet och hello operatör kontonummer för returnerade leverans. Du kan alternativt ange hello antalet enheter och hello leverans samt datum.

## <a name="next-steps"></a>Nästa steg

* [Med hjälp av hello Import/Export service REST API](storage-import-export-using-the-rest-api.md)
