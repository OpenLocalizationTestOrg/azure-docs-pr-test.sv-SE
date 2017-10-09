---
title: "aaaUpdate Media Services efter rullande lagring åtkomstnycklar | Microsoft Docs"
description: "Den här artikeln får du vägledning om hur tooupdate Media Services efter rullande lagring åtkomstnycklar."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: a892ebb0-0ea0-4fc8-b715-60347cc5c95b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: milanga;cenkdin;juliako
ms.openlocfilehash: 26fa7a75a73397842aaebda59516a00f68ab97f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="update-media-services-after-rolling-storage-access-keys"></a>Uppdatera Media Services när du återställer åtkomstnycklar för lagring

När du skapar ett nytt konto i Azure Media Services (AMS) kan uppmanas du också tooselect ett Azure Storage-konto som används för toostore media-innehåll. Du kan lägga till fler än en storage-konton tooyour Media Services-konto. Det här avsnittet visar hur toorotate lagringsnycklar. Den visar även hur tooadd lagringskonton tooa media-konto. 

tooperform hello åtgärder som beskrivs i det här avsnittet, bör du använda [ARM-API: er](https://docs.microsoft.com/rest/api/media/mediaservice) och [Powershell](https://docs.microsoft.com/powershell/resourcemanager/azurerm.media/v0.3.2/azurerm.media).  Mer information finns i [hur toomanage Azure resurser med PowerShell och Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md).

## <a name="overview"></a>Översikt

När ett nytt lagringskonto har skapats, genererar Azure två 512-bitars åtkomstnycklar för lagring, som används tooauthenticate åtkomst tooyour lagringskontot. tookeep lagring anslutningarna säkrare bör tooperiodically återskapa och rotera din lagringsåtkomstnyckel. Två åtkomstnycklar (primär eller sekundär) tillhandahålls i ordning tooenable du toomaintain anslutningar toohello storage-konto med en snabbtangent medan du återskapar hello andra snabbtangent. Den här proceduren är en förkortning av ”rullande snabbtangenter”.

Media Services är beroende av en nyckel för säkerhetslagring tillhandahålls tooit. Mer specifikt beroende hello-positionerare som används toostream eller hämta dina tillgångar hello angivna lagringsåtkomstnyckel. När en AMS-kontot skapas det tar ett beroende på hello primära lagringsåtkomstnyckel som standard men som en användare kan du uppdatera hello lagringsnyckel AMS. Du måste kontrollera toolet Media Services veta vilka viktiga toouse genom att följa stegen som beskrivs i det här avsnittet.  

>[!NOTE]
> Om du har flera lagringskonton, utför den här proceduren med varje lagringskonto: hello ordning i vilken du roterar lagringsnycklar är inte fast. Du kan rotera hello sekundärnyckeln först och hello primär nyckel eller tvärtom.
>
> Innan du kör stegen beskrivs i det här avsnittet på ett Produktionskonto för, se till att tootest dem på ett konto för Förproduktion.
>

## <a name="steps-toorotate-storage-keys"></a>Steg toorotate lagringsnycklar 
 
 1. Ändra hello primära lagringskontonyckel via hello powershell-cmdleten eller [Azure](https://portal.azure.com/) portal.
 2. Anropa synkronisering AzureRmMediaServiceStorageKeys cmdleten med lämpliga parametrar tooforce media konto toopick in lagringskontonycklar
 
    hello som följande exempel visar hur toosync nycklar toostorage konton.
  
         Sync-AzureRmMediaServiceStorageKeys -ResourceGroupName $resourceGroupName -AccountName $mediaAccountName -StorageAccountId $storageAccountId
  
 3. Vänta en timme. Kontrollera hello strömmande scenarier fungerar.
 4. Ändra lagringskontots sekundära åtkomstnyckel via hello powershell-cmdlet eller Azure-portalen.
 5. Anropa synkronisering AzureRmMediaServiceStorageKeys powershell med lämpliga parametrar tooforce media konto toopick in nya lagringskontonycklarna genereras. 
 6. Vänta en timme. Kontrollera hello strömmande scenarier fungerar.
 
### <a name="a-powershell-cmdlet-example"></a>Ett powershell-cmdlet-exempel 

hello visar exemplet nedan hur tooget hello storage-konto och synkroniseras med hello AMS-kontot.

    $regionName = "West US"
    $resourceGroupName = "SkyMedia-USWest-App"
    $mediaAccountName = "sky"
    $storageAccountName = "skystorage"
    $storageAccountId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Storage/storageAccounts/$storageAccountName"

    Sync-AzureRmMediaServiceStorageKeys -ResourceGroupName $resourceGroupName -AccountName $mediaAccountName -StorageAccountId $storageAccountId

 
## <a name="steps-tooadd-storage-accounts-tooyour-ams-account"></a>Steg tooadd lagringskonton tooyour AMS-konto

hello följande avsnitt visar hur tooadd lagringskonton tooyour AMS-konto: [bifoga flera storage-konton tooa Media Services-konto](meda-services-managing-multiple-storage-accounts.md).

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

### <a name="acknowledgments"></a>Bekräftelser
Vi vill gärna tooacknowledge hello följande personer som har bidragit till att skapa det här dokumentet: Cenk Dingiloglu, Milano Gada Seva Titov.
