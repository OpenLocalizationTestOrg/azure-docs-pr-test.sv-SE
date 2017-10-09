---
title: aaaAzure Media Services encoding felkoder | Microsoft Docs
description: "Det här avsnittet beskrivs felkoder som kan returneras om ett fel uppstod under hello kodning för körning av aktiviteten..."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: ce4e939f-5aee-41f9-859d-e4429815e9f2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: b69b6abee797c40c9b8b8f23bf2398273c170e7f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="encoding-error-codes"></a>Kodning felkoder

hello visas följande tabell felkoder som kan returneras om ett fel uppstod under hello kodning för körning av aktiviteten.  tooget felinformation i .NET-kod använder hello [felinformation](http://msdn.microsoft.com/library/microsoft.windowsazure.mediaservices.client.errordetail.aspx) klass. tooget felinformation i REST-koden använder hello [ErrorDetail](https://msdn.microsoft.com/library/jj853026.aspx) REST API.

| ErrorDetail.Code | Möjliga orsaker till felet |
| --- | --- |
| Okänd |Okänt fel vid körning av hello-aktivitet |
| ErrorDownloadingInputAssetMalformedContent |Kategori för fel som täcker fel i hämtar inkommande tillgång till exempel felaktiga filnamn, noll längd filer, felaktig formaterar och så vidare. |
| ErrorDownloadingInputAssetServiceFailure |Kategori för fel som beskriver problem på tjänstsidan hello - exempel nätverks- eller fel vid hämtning av. |
| ErrorParsingConfiguration |Kategori av fel där uppgift <see cref="MediaTask.PrivateData"/> (konfiguration) är inte giltig, till exempel hello-konfigurationen är inte ett giltigt system förinställningen eller innehåller ogiltig XML. |
| ErrorExecutingTaskMalformedContent |Fel under hello körning av hello uppgift där problem i hello indata mediefiler orsaka fel kategori. |
| ErrorExecutingTaskUnsupportedFormat |Kategori av fel där hello media kunde inte bearbeta filer hello - medieformat inte stöds eller matchar inte hello konfiguration. Till exempel försök tooproduce ljuddata utdata från en tillgång som har endast video |
| ErrorProcessingTask |Kategori för andra fel som hello medieprocessor uppstår under hello bearbetning av hello-aktivitet som är inte relaterat toocontent. |
| ErrorUploadingOutputAsset |Fel vid överföring av hello utdatatillgången kategori |
| ErrorCancelingTask |Kategori fel toocover fel vid försök toocancel hello aktivitet |
| TransientError |Kategori av fel toocover tillfälliga problem (t.ex. tillfällig nätverksproblem med Azure Storage) |

tooget hjälp från hello **Media Services** team, öppna en [stöder biljett](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>Relaterade artiklar
* [Utföra avancerade kodning uppgifter genom att anpassa Media Encoder Standard förinställningar](media-services-custom-mes-presets-with-dotnet.md)
* [Kvoter och begränsningar](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
