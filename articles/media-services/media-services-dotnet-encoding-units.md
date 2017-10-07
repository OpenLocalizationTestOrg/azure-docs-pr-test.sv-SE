---
title: "aaaScale media bearbetning genom att lägga till kodning enheter – Azure |  Microsoft Docs"
description: "Lär dig hur toohow tooadd kodning enheter med .NET"
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 33f7625a-966a-4f06-bc09-bccd6e2a42b5
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako;milangada;
ms.openlocfilehash: b9f71a6487c5d136319a38a1598d60edfaa81b9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooscale-encoding-with-net-sdk"></a>Hur tooscale encoding med .NET SDK
> [!div class="op_single_selector"]
> * [Portal](media-services-portal-scale-media-processing.md)
> * [.NET](media-services-dotnet-encoding-units.md)
> * [REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a>Översikt
> [!IMPORTANT]
> Se till att tooreview hello [översikt](media-services-scale-media-processing-overview.md) avsnittet tooget mer information om att skala media bearbetning av avsnittet.
> 
> 

toochange hello reserverad enhet typ och hello antalet kodningsreserverade enheter med hjälp av .NET SDK hello följande:

    IEncodingReservedUnit encodingS1ReservedUnit = _context.EncodingReservedUnits.FirstOrDefault();
    encodingS1ReservedUnit.ReservedUnitType = ReservedUnitType.Basic; // Corresponds tooS1
    encodingS1ReservedUnit.Update();
    Console.WriteLine("Reserved Unit Type: {0}", encodingS1ReservedUnit.ReservedUnitType);

    encodingS1ReservedUnit.CurrentReservedUnits = 2;
    encodingS1ReservedUnit.Update();

    Console.WriteLine("Number of reserved units: {0}", encodingS1ReservedUnit.CurrentReservedUnits);

## <a name="opening-a-support-ticket"></a>Öppna ett supportärende
Som standard var Media Services-konto kan skala tooup too25 kodning och 5 på begäran reserverade enheter för strömning. Du kan begära en högre gräns genom att öppna ett supportärende.

### <a name="open-a-support-ticket"></a>Öppna ett supportärende
tooopen ett supportärende hello följande:

1. Klicka på [få Support](https://manage.windowsazure.com/?getsupport=true). Om du inte är inloggad som du kommer att tillfrågas tooenter dina autentiseringsuppgifter.
2. Välj din prenumeration.
3. Välj ”Technical” under typ av stöd.
4. Klicka på ”Skapa biljett”.
5. Välj ”Azure Media Services” i hello produktlista visas på hello nästa sida.
6. Välj ”typ” som passar ditt problem.
7. Klicka på Fortsätt.
8. Följ instruktionerna på nästa sida och anger sedan information om problemet.
9. Klicka på Skicka tooopen hello biljett.

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

