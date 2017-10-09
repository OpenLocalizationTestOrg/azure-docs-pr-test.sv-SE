---
title: "aaaUpload filer till ett Azure Media Services-konto från Azure StorSimple | Microsoft Docs"
description: "I den här artikeln finns en kort översikt över Azure StorSimple Data Manager. hello artikeln innehåller också länkar tootutorials som visar hur tooextract data från StorSimple och ladda upp det som tillgångar tooan Azure Media Services-konto."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 1dd09328-262b-43ef-8099-73241b49a925
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/27/2017
ms.author: juliako
ms.openlocfilehash: 7e9712aa480106bbd5fcc63eaecf0418b24a8bef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-into-an-azure-media-services-account-from-azure-storsimple"></a>Ladda upp filer till ett Azure Media Services-konto från Azure StorSimple

I den här artikeln finns en kort översikt över Azure StorSimple Data Manager. hello artikeln innehåller också länkar tootutorials som visar hur tooextract data från StorSimple och ladda upp data som tillgångar tooan konto i Azure Media Services (AMS).

> 
> [!NOTE]
> Azure StorSimple Data Manager finns för närvarande som privat förhandsutgåva. 
> 

## <a name="overview"></a>Översikt

I Media Services överför du dina digitala filer till en tillgång. hello tillgång kan innehålla video, ljud, bilder, miniatyrsamlingar, text spår och textning filer (och hello metadata om dessa filer.) När hello filerna har överförts lagras innehållet på ett säkert sätt i hello molnet för ytterligare bearbetning och strömning.

[Azure StorSimple](https://docs.microsoft.com/azure/storsimple/) använder molnlagring som ett tillägg av hello lokalt lösning och automatiskt nivåer data över hello lokal lagring och lagringsutrymmet i molnet. Hej StorSimple-enhet dedupes och komprimerar data innan du skickar den toohello moln, vilket gör det mycket effektivt för att skicka stora filer toohello moln. Hej [StorSimple Data Manager](../storsimple/storsimple-data-manager-overview.md) service ger API: er som aktiverar du tooextract data från StorSimple och visa AMS tillgångar.

## <a name="get-started"></a>Kom igång

1. [Skapa ett Media Services-konto](media-services-portal-create-account.md) som du vill tootransfer hello tillgångar.
2. Registrera dig för förhandsversionen av Data Manager, enligt beskrivningen i hello [StorSimple Data Manager](../storsimple/storsimple-data-manager-overview.md) artikel.
3. Skapa ett StorSimple Data Manager-konto.
4. Skapa ett omvandlingsjobb som när det körs hämtar data från en StorSimple-enhet och överför dem till ett AMS-konto som tillgångar. 

    När hello jobb börjar köras, skapas en storage-kö. Den här kön fylls med meddelanden om transformerade blobbar i takt med att de är klara. hello namnet på den här kön är hello samma som hello namnet på hello jobbdefinitionen. Du kan använda den här kön toodetermine när tillgången är klar och anropa din önskade toorun för Media Services-åtgärden på den. Du kan till exempel använda den här kön tootrigger en Azure-funktion som innehåller hello nödvändig Media Services-kod.

## <a name="see-also"></a>Se även

[Använd hello .net SDK tootrigger jobb i hello Data Manager](../storsimple/storsimple-data-manager-dotnet-jobs.md)

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Nästa steg

Du kan nu koda överförda tillgångar. Mer information finns i [Koda tillgångar](media-services-portal-encode.md).
