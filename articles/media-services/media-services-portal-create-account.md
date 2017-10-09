---
title: aaaCreate ett Azure Media Services-konto med hello Azure-portalen | Microsoft Docs
description: "Den här självstudiekursen vägleder dig genom hello stegen för att skapa ett Azure Media Services-konto med hello Azure-portalen."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: c551e158-aad6-47b4-931e-b46260b3ee4c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: juliako
ms.openlocfilehash: fdad93d5d470fc08380670ec0f6c2d33468b1492
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-media-services-account-using-hello-azure-portal"></a>Skapa ett Azure Media Services-konto med hello Azure-portalen
> [!div class="op_single_selector"]
> * [Portal](media-services-portal-create-account.md)
> * [PowerShell](media-services-manage-with-powershell.md)
> * [REST](https://docs.microsoft.com/rest/api/media/mediaservice)
> 
> [!NOTE]
> toocomplete den här självstudiekursen kommer du behöver ett Azure-konto. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/). 
> 
> 

hello Azure-portalen kan tooquickly skapa ett Azure Media Services (AMS)-konto. Du kan använda ditt konto tooaccess Media Services som aktiverar du toostore, kryptera, koda, hantera och strömma medieinnehåll i Azure. Samtidigt hello du skapar ett Media Services-konto du också skapa ett associerat lagringskonto (eller använda ett befintligt) i hello samma geografiska region som hello Media Services-konto.

Den här artikeln beskriver några vanliga begrepp och visar hur toocreate ett Media Services-konto med hello Azure-portalen.

## <a name="concepts"></a>Koncept
Åtkomst till Media Services kräver två associerade konton:

* Ett Media Services-konto. De ger dig åtkomst tooa uppsättning molnbaserade Media Services som är tillgängliga i Azure. På ett Media Services-konto lagras inget faktiskt medieinnehåll. I stället lagras metadata om hello medieinnehåll och mediebearbetningsjobb på ditt konto. För närvarande hello du skapar hello konto, Välj en tillgänglig Media Services-region. hello-region som du väljer är ett datacenter som lagrar hello metadataposterna för ditt konto.
  
* Ett Azure-lagringskonto. Storage-konton måste finnas i hello samma geografiska region som hello Media Services-konto. När du skapar ett Media Services-konto kan du antingen välja ett befintligt lagringskonto i hello samma region eller skapa ett nytt lagringskonto i hello samma region. Om du tar bort ett Media Services-konto raderas inte hello blobbar på ditt relaterade lagringskonto.

> [!NOTE]
> Information om tillgänglighet för Azure Media Services-funktioner i olika regioner finns i [tillgänglighet för AMS-funktioner i datacenter](scenarios-and-availability.md#availability).

## <a name="create-an-ams-account"></a>Skapa ett AMS-konto
hello stegen i det här avsnittet visar hur toocreate en AMS-kontot.

1. Logga in på hello [Azure-portalen](https://portal.azure.com/).
2. Klicka på **+Ny** > **Webb + mobilt** > **Media Services**.
   
    ![Skapa Media Services](./media/media-services-create-account/media-services-new1.png)
3. Ange de erfordrade värdena i **SKAPA MEDIA SERVICES-KONTO**.
   
    ![Skapa Media Services](./media/media-services-create-account/media-services-new3.png)
   
   1. I **kontonamn**, ange hello namnet på hello nya AMS-kontot. Namnet på ett Media Services är alla gemena bokstäver eller siffror utan blanksteg och 3 too24 tecken.
   2. Vid prenumeration väljer du bland hello olika Azure-prenumerationer som du har åtkomst till.
   3. I **resursgruppen**, Välj hello ny eller befintlig resurs.  En resursgrupp är en samling resurser som delar livscykel, behörigheter och principer. Lär dig mer [här](../azure-resource-manager/resource-group-overview.md#resource-groups).
   4. I **plats**, Välj hello geografiska region som kommer att använda toostore hello media och metadataposter poster för Media Services-kontot. Den här regionen kommer att använda tooprocess och strömma dina media. Endast hello tillgängliga Media Services-regionerna visas i listrutan hello. 
   5. I **Lagringskonto**, Välj en storage-konto tooprovide blob storage för hello medieinnehållet från ditt Media Services-konto. Du kan välja ett befintligt lagringskonto i hello samma geografiska region som ditt Media Services-konto, eller om du kan skapa ett lagringskonto. Ett nytt lagringskonto skapas i hello samma region. hello regler för lagringskontot namn är hello samma som för Media Services-konton.
      
       Mer information om lagring finns [här](../storage/common/storage-introduction.md).
   6. Välj **PIN-kod toodashboard** toosee hello förloppet för kontodistributionen hello.
4. Klicka på **skapa** längst hello hello formuläret.
   
    När hello kontot har skapats, översikt översiktssidan läses in. I hello strömmande slutpunkten tabell hello konto har ett standardvärde strömmande slutpunkten i hello **stoppad** tillstånd. 

    >[!NOTE]
    >När AMS-kontot skapas en **standard** strömningsslutpunkt har lagts till tooyour konto i hello **stoppad** tillstånd. toostart strömning ditt innehåll och dra nytta av dynamisk paketering och dynamisk kryptering hello strömningsslutpunkt som du vill toostream innehåll har toobe i hello **kör** tillstånd. 
   
## <a name="toomanage-your-ams-account"></a>toomanage AMS-kontot

toomanage AMS-kontot (till exempel ansluta toohello AMS API programmässigt, överföra videor, koda tillgångar, konfigurera innehållsskydd, övervaka jobbförlopp) Välj **inställningar** på vänster sida av hello portal hello. Från hello **inställningar**, navigera tooone hello tillgängliga blad (till exempel: **API-åtkomst**, **tillgångar**, **jobb**, **Content protection**).


## <a name="next-steps"></a>Nästa steg

Du kan nu överföra filer till AMS-kontot. Mer information finns i [Överföra filer](media-services-portal-upload-files.md).

Om du planerar tooaccess AMS API programmässigt Se [åtkomst hello Azure Media Services API med Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

