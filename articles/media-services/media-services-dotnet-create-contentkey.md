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
# <a name="create-contentkeys-with-net"></a><span data-ttu-id="40ffd-103">Skapa ContentKeys med .NET</span><span class="sxs-lookup"><span data-stu-id="40ffd-103">Create ContentKeys with .NET</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="40ffd-104">REST</span><span class="sxs-lookup"><span data-stu-id="40ffd-104">REST</span></span>](media-services-rest-create-contentkey.md)
> * [<span data-ttu-id="40ffd-105">.NET</span><span class="sxs-lookup"><span data-stu-id="40ffd-105">.NET</span></span>](media-services-dotnet-create-contentkey.md)
> 
> 

<span data-ttu-id="40ffd-106">Media Services kan du toocreate och leverera krypterade tillgångar.</span><span class="sxs-lookup"><span data-stu-id="40ffd-106">Media Services enables you toocreate and deliver encrypted assets.</span></span> <span data-ttu-id="40ffd-107">En **ContentKey** ger säker åtkomst tooyour **tillgången**s.</span><span class="sxs-lookup"><span data-stu-id="40ffd-107">A **ContentKey** provides secure access tooyour **Asset**s.</span></span> 

<span data-ttu-id="40ffd-108">När du skapar en ny tillgång (till exempel innan du [ladda upp filer](media-services-dotnet-upload-files.md)), kan du ange följande krypteringsalternativ för hello: **StorageEncrypted**, **CommonEncryptionProtected**, eller **EnvelopeEncryptionProtected**.</span><span class="sxs-lookup"><span data-stu-id="40ffd-108">When you create a new asset (for example, before you [upload files](media-services-dotnet-upload-files.md)), you can specify hello following encryption options: **StorageEncrypted**, **CommonEncryptionProtected**, or **EnvelopeEncryptionProtected**.</span></span> 

<span data-ttu-id="40ffd-109">När du levererar tillgångar tooyour klienter kan du [konfigurera för tillgångar toobe dynamiskt krypterad](media-services-dotnet-configure-asset-delivery-policy.md) med något av följande två krypteringar hello: **DynamicEnvelopeEncryption** eller  **DynamicCommonEncryption**.</span><span class="sxs-lookup"><span data-stu-id="40ffd-109">When you deliver assets tooyour clients, you can [configure for assets toobe dynamically encrypted](media-services-dotnet-configure-asset-delivery-policy.md) with one of hello following two encryptions: **DynamicEnvelopeEncryption** or **DynamicCommonEncryption**.</span></span>

<span data-ttu-id="40ffd-110">Krypterade tillgångar ha toobe som är associerade med **ContentKey**s.</span><span class="sxs-lookup"><span data-stu-id="40ffd-110">Encrypted assets have toobe associated with **ContentKey**s.</span></span> <span data-ttu-id="40ffd-111">Den här artikeln beskriver hur toocreate en innehållsnyckel.</span><span class="sxs-lookup"><span data-stu-id="40ffd-111">This article describes how toocreate a content key.</span></span>

> [!NOTE]
> <span data-ttu-id="40ffd-112">När du skapar en ny **StorageEncrypted** tillgångsinformation med hjälp av hello Media Services .NET SDK hello **ContentKey** skapas automatiskt och länkade med hello tillgången.</span><span class="sxs-lookup"><span data-stu-id="40ffd-112">When creating a new **StorageEncrypted** asset using hello Media Services .NET SDK , hello **ContentKey** is automatically created and linked with hello asset.</span></span>
> 
> 

## <a name="contentkeytype"></a><span data-ttu-id="40ffd-113">ContentKeyType</span><span class="sxs-lookup"><span data-stu-id="40ffd-113">ContentKeyType</span></span>
<span data-ttu-id="40ffd-114">Ett av hello-värden som du måste ange när skapar en innehåll nyckeln är hello viktiga innehållstyp.</span><span class="sxs-lookup"><span data-stu-id="40ffd-114">One of hello values that you must set when create a content key is hello content key type.</span></span> <span data-ttu-id="40ffd-115">Välj något av följande värden hello.</span><span class="sxs-lookup"><span data-stu-id="40ffd-115">Choose from one of hello following values.</span></span> 

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

## <span data-ttu-id="40ffd-116"><a id="envelope_contentkey"></a>Skapa kuvert typen ContentKey</span><span class="sxs-lookup"><span data-stu-id="40ffd-116"><a id="envelope_contentkey"></a>Create envelope type ContentKey</span></span>
<span data-ttu-id="40ffd-117">hello skapar nedanstående kodutdrag en innehållsnyckel av hello kuvert krypteringstyp.</span><span class="sxs-lookup"><span data-stu-id="40ffd-117">hello following code snippet creates a content key of hello envelope encryption type.</span></span> <span data-ttu-id="40ffd-118">Sedan associerar den hello nyckeln med hello angivna tillgången.</span><span class="sxs-lookup"><span data-stu-id="40ffd-118">It then associates hello key with hello specified asset.</span></span>

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

<span data-ttu-id="40ffd-119">Anropa</span><span class="sxs-lookup"><span data-stu-id="40ffd-119">call</span></span>

    IContentKey key = CreateEnvelopeTypeContentKey(encryptedsset);



## <span data-ttu-id="40ffd-120"><a id="common_contentkey"></a>Skapa gemensam typ. ContentKey</span><span class="sxs-lookup"><span data-stu-id="40ffd-120"><a id="common_contentkey"></a>Create common type ContentKey</span></span>
<span data-ttu-id="40ffd-121">hello skapar nedanstående kodutdrag en innehållsnyckel av hello vanliga krypteringstyp.</span><span class="sxs-lookup"><span data-stu-id="40ffd-121">hello following code snippet creates a content key of hello common encryption type.</span></span> <span data-ttu-id="40ffd-122">Sedan associerar den hello nyckeln med hello angivna tillgången.</span><span class="sxs-lookup"><span data-stu-id="40ffd-122">It then associates hello key with hello specified asset.</span></span>

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
<span data-ttu-id="40ffd-123">Anropa</span><span class="sxs-lookup"><span data-stu-id="40ffd-123">call</span></span>

    IContentKey key = CreateCommonTypeContentKey(encryptedsset); 


## <a name="media-services-learning-paths"></a><span data-ttu-id="40ffd-124">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="40ffd-124">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="40ffd-125">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="40ffd-125">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

