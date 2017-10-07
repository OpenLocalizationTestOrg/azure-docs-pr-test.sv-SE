---
title: aaaCreate ContentKeys med .NET
description: "Lär dig hur toocreate innehåll nycklar som ger säker åtkomst till tooAssets."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 225b05e5-7d30-409c-b5b7-3ef0634310c7
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: 35909c64e8393e228be75c464a034ffc40122952
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-contentkeys-with-net"></a>Skapa ContentKeys med .NET
> [!div class="op_single_selector"]
> * [REST](media-services-rest-create-contentkey.md)
> * [.NET](media-services-dotnet-create-contentkey.md)
> 
> 

Media Services kan du toocreate och leverera krypterade tillgångar. En **ContentKey** ger säker åtkomst tooyour **tillgången**s. 

När du skapar en ny tillgång (till exempel innan du [ladda upp filer](media-services-dotnet-upload-files.md)), kan du ange följande krypteringsalternativ för hello: **StorageEncrypted**, **CommonEncryptionProtected**, eller **EnvelopeEncryptionProtected**. 

När du levererar tillgångar tooyour klienter kan du [konfigurera för tillgångar toobe dynamiskt krypterad](media-services-dotnet-configure-asset-delivery-policy.md) med något av följande två krypteringar hello: **DynamicEnvelopeEncryption** eller  **DynamicCommonEncryption**.

Krypterade tillgångar ha toobe som är associerade med **ContentKey**s. Den här artikeln beskriver hur toocreate en innehållsnyckel.

> [!NOTE]
> När du skapar en ny **StorageEncrypted** tillgångsinformation med hjälp av hello Media Services .NET SDK hello **ContentKey** skapas automatiskt och länkade med hello tillgången.
> 
> 

## <a name="contentkeytype"></a>ContentKeyType
Ett av hello-värden som du måste ange när skapar en innehåll nyckeln är hello viktiga innehållstyp. Välj något av följande värden hello. 

    public enum ContentKeyType
    {
        /// <summary>
        /// Specifies a content key for common encryption.
        /// </summary>
        /// <remarks>This is hello default value.</remarks>
        CommonEncryption = 0,

        /// <summary>
        /// Specifies a content key for storage encryption.
        /// </summary>
        StorageEncryption = 1,

        /// <summary>
        /// Specifies a content key for configuration encryption.
        /// </summary>
        ConfigurationEncryption = 2,

        /// <summary>
        /// Specifies a content key for Envelope encryption.  Only used internally.
        /// </summary>
        EnvelopeEncryption = 4
    }

## <a id="envelope_contentkey"></a>Skapa kuvert typen ContentKey
hello skapar nedanstående kodutdrag en innehållsnyckel av hello kuvert krypteringstyp. Sedan associerar den hello nyckeln med hello angivna tillgången.

    static public IContentKey CreateEnvelopeTypeContentKey(IAsset asset)
    {
        // Create envelope encryption content key
        Guid keyId = Guid.NewGuid();
        byte[] contentKey = GetRandomBuffer(16);

        IContentKey key = _context.ContentKeys.Create(
                                keyId,
                                contentKey,
                                "ContentKey",
                                ContentKeyType.EnvelopeEncryption);

        asset.ContentKeys.Add(key);

        return key;
    }

    static private byte[] GetRandomBuffer(int size)
    {
        byte[] randomBytes = new byte[size];
        using (RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider())
        {
            rng.GetBytes(randomBytes);
        }

        return randomBytes;
    }

Anropa

    IContentKey key = CreateEnvelopeTypeContentKey(encryptedsset);



## <a id="common_contentkey"></a>Skapa gemensam typ. ContentKey
hello skapar nedanstående kodutdrag en innehållsnyckel av hello vanliga krypteringstyp. Sedan associerar den hello nyckeln med hello angivna tillgången.

    static public IContentKey CreateCommonTypeContentKey(IAsset asset)
    {
        // Create common encryption content key
        Guid keyId = Guid.NewGuid();
        byte[] contentKey = GetRandomBuffer(16);

        IContentKey key = _context.ContentKeys.Create(
                                keyId,
                                contentKey,
                                "ContentKey",
                                ContentKeyType.CommonEncryption);

        // Associate hello key with hello asset.
        asset.ContentKeys.Add(key);

        return key;
    }

    static private byte[] GetRandomBuffer(int length)
    {
        var returnValue = new byte[length];

        using (var rng =
            new System.Security.Cryptography.RNGCryptoServiceProvider())
        {
            rng.GetBytes(returnValue);
        }

        return returnValue;
    }
Anropa

    IContentKey key = CreateCommonTypeContentKey(encryptedsset); 


## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

