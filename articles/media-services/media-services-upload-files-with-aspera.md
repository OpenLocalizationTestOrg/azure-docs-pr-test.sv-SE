---
title: aaaUpload filer till ett Azure Media Services-konto med Aspera | Microsoft Docs
description: "Den här självstudiekursen vägleder dig genom hello stegen för att överföra filer till ett lagringskonto som är kopplat till ett Media Services-konto med hello ** Aspera Server på begäran **-tjänsten på Azure."
services: media-services
documentationcenter: 
author: johndeu
manager: cfowler
editor: 
ms.assetid: 8812623a-b425-4a0f-9e05-0ee6c839b6f9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/17/2017
ms.author: juliako
ms.openlocfilehash: 7213f016cc1b7f262b14db7b39b478a03970d1c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-into-a-media-services-account-using-hello-aspera-server-on-demand-service-on-azure"></a>Överföra filer till ett Media Services-konto med hello Aspera Server på begäran-tjänsten på Azure

## <a name="overview"></a>Översikt

**Aspera** är en programvara för filöverföring i hög hastighet. **Aspera Server On Demand** för Azure möjliggör snabba överföringar och hämtning av stora filer direkt i Azure-blobobjektlagringen. Information om **Aspera på begäran**, se hello [Aspera moln](http://cloud.asperasoft.com/) plats. 
  
**Aspera Server på begäran** för Azure kan köpas från hello [Azure marketplace](https://azure.microsoft.com/en-us/marketplace/). Ordna toocomplete köp av **Aspera Server på begäran** på Azure, loggar du in Azure Marketplace med ditt Windows Live ID.

Den här självstudiekursen vägleder dig genom hello stegen för att överföra filer till ett lagringskonto som är kopplat till ett Media Services-konto med hello **Aspera Server på begäran** på Azure. 

Du hittar ett exempel som visar hur toouse Azure fungerar med Aspera och Media Services [här](https://github.com/Azure-Samples/media-services-dotnet-functions-integration/tree/master/103-aspera-ingest).

>[!NOTE]
>Det finns en gräns toohello maximal filstorlek som stöds för bearbetning med Azure Media Services-medieprocessorer (MP). Se [detta](media-services-quotas-and-limitations.md) avsnittet för information om hello filstorleksbegränsningar.
>

## <a name="prerequisites"></a>Krav 

toocomplete den här kursen behöver du:

* Ett Windows Live ID
* Ett [Azure-konto](https://azure.microsoft.com). Mer information finns i [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/). 
* Ett [Azure Media Services-konto](media-services-portal-create-account.md).

## <a name="purchase-aspera-on-demand-for-azure"></a>Köpa Aspera On Demand för Azure

När du har loggat in på Azure Marketplace, följer du dessa grundläggande steg toocomplete köpet Aspera på begäran för Azure.

1. Sök efter Aspera och välj Server On Demand.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera001.png)

2. Granska hello-prenumerationsavtal och klicka på Registrera dig

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera002.png)

3. Fyll i hello programvarukrav för din Server på begäran-prenumeration.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera003.png)

4. Klicka på hello **prisnivån** och välj önskad månatliga volymen hello sub Kontrollpanelen. I hello **planera** fönstret väljer **OK**. Sedan hello **väljer din prisnivån** klickar du på **Välj**.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera004.png)

5. Klicka på **juridiska villkor** tooview och godkänner hello juridiska villkoren hello sub Kontrollpanelen. När du har granskat hello juridiska villkor, klickar du på **inköp**.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera005.png)

6. Fullständigt hello köp genom att klicka på **skapa**.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera006.png)

7. hello Azure instrumentpanelen kommer att meddela att den etablerar hello-tjänsten.  När den är klar med att etablera hittar hello prenumeration genom att söka i dina resurser för hello namnet på hello-tjänst. När du har hittat hello-tjänsten, dubbel-Klicka på det toolaunch hello service management portal.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera007.png)

8. Starta hello Aspera-hanteringsportalen. När du har hittat nya Aspera tjänsten får du åtkomst toohello management portal genom att klicka på hello-tjänsten.  En ny panel öppnas. Från inom den nya panelen måste tooclick på hello **resursnamnet** av den nya tjänsten.  I följande skärmbild hello, är hello resursnamnet 'AsperaTransferDemo'. När du klickar på hello resursnamnet startas en annan panel. Länken Manage (Hantera) visas på den nyöppnade panelen. Klicka på hello ”hantera” länken toolaunch hello Aspera-hanteringsportalen.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera008.png)

9. Hantera länken genom att klicka på hello, du får toohello registreringssidan, vilket krävs tooaccess hello service.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera009.png)

10. Du bör nu ha åtkomst toohello Aspera service management portal där du kan skapa snabbtangenter, hämta Aspera klienter och licenser, visa syntax och lär dig mer om hello API: er.

    hello visar följande skärmbild hello åtkomst skapas. 

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera010.png)

    hello visar följande skärmbild hello användning reporting gränssnitt i hello-portalen. 

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera011.png)

## <a name="upload-files-with-aspera"></a>Överföra filer med Aspera

1. Hämta och installera klientprogramvaran för hello Aspera:
    
    * [Plugin-program för webbläsare](http://downloads.asperasoft.com/connect2/)
    * [Rich-klient](http://downloads.asperasoft.com/en/downloads/2)

2. Gör din första överföring. I ordning toouse hello Aspera klienten tootransfer med hello tjänst för överföring av Aspera behöver du toocomplete hello följande: 

    1. Skapa en snabbtangent hello Aspera portalen.  
    2. Hämta och installera licens hello Aspera klienten (programvara finns i hello Aspera portal).  

    >[!NOTE]
    >Läs hello Aspera klienten guide för konfigurationsinformation.
    
    3. Hämta viss information för ditt lagringskonto som är kopplat till ditt Azure Media-konto med hjälp av hello [Azure-portalen](https://portal.azure.com/). Mer specifikt namn och nyckel, och hello storage blob behållare i toowhich du tooplace ditt innehåll. 

        * tooget hello lagring information från hello portal: hitta ditt lagringskonto, klickar du på hello snabbtangenter och kopiera hello namn och hello nyckeln för ditt konto.
        * tooget hello behållarnamn: hitta ditt lagringskonto, Välj **Blobbar**väljer hello namnet på hello-behållare som du vill tooupload hello innehåll i. 

    Nedan visas hello Skärmbild av hello Aspera klienten **Connection Manager** där du måste ange hello ”Azure” lagringstyp och autentiseringsuppgifter som hello blob-behållare.

    ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera012.png)

## <a name="resources"></a>Resurser

hello följande resurser har nämns i den här artikeln. 

* [Ansluta plugin-program för webbläsare](http://downloads.asperasoft.com/connect2/)
* [Anslutningsguide](http://downloads.asperasoft.com/en/documentation/8)
* [Aspera-klient](http://downloads.asperasoft.com/en/downloads/2)
* [Klientguide](http://downloads.asperasoft.com/en/documentation/2)

## <a name="next-steps"></a>Nästa steg

Du kan nu [kopiera blobar från ett lagringskonto till ett AMS-konto](media-services-copying-existing-blob.md#copy-blobs-from-a-storage-account-into-an-ams-account).

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

