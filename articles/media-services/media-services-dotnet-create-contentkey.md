---
title: Skapa ContentKeys med .NET
description: "Lär dig hur du skapar innehåll nycklar som ger säker åtkomst till resurser."
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
ms.openlocfilehash: 3280a6fcde59bae360da7cb9fea4bb649f984e43
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-contentkeys-with-net"></a><span data-ttu-id="31fc3-103">Skapa ContentKeys med .NET</span><span class="sxs-lookup"><span data-stu-id="31fc3-103">Create ContentKeys with .NET</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="31fc3-104">REST</span><span class="sxs-lookup"><span data-stu-id="31fc3-104">REST</span></span>](media-services-rest-create-contentkey.md)
> * [<span data-ttu-id="31fc3-105">.NET</span><span class="sxs-lookup"><span data-stu-id="31fc3-105">.NET</span></span>](media-services-dotnet-create-contentkey.md)
> 
> 

<span data-ttu-id="31fc3-106">Media Services kan du skapa och leverera krypterade tillgångar.</span><span class="sxs-lookup"><span data-stu-id="31fc3-106">Media Services enables you to create and deliver encrypted assets.</span></span> <span data-ttu-id="31fc3-107">En **ContentKey** ger säker åtkomst till din **tillgången**s.</span><span class="sxs-lookup"><span data-stu-id="31fc3-107">A **ContentKey** provides secure access to your **Asset**s.</span></span> 

<span data-ttu-id="31fc3-108">När du skapar en ny tillgång (till exempel innan du [ladda upp filer](media-services-dotnet-upload-files.md)), kan du ange följande krypteringsalternativen: **StorageEncrypted**, **CommonEncryptionProtected**, eller **EnvelopeEncryptionProtected**.</span><span class="sxs-lookup"><span data-stu-id="31fc3-108">When you create a new asset (for example, before you [upload files](media-services-dotnet-upload-files.md)), you can specify the following encryption options: **StorageEncrypted**, **CommonEncryptionProtected**, or **EnvelopeEncryptionProtected**.</span></span> 

<span data-ttu-id="31fc3-109">När du levererar tillgångar till dina klienter kan du [konfigurera för tillgångar ska vara dynamiskt krypterad](media-services-dotnet-configure-asset-delivery-policy.md) med något av följande två krypteringar: **DynamicEnvelopeEncryption** eller **DynamicCommonEncryption**.</span><span class="sxs-lookup"><span data-stu-id="31fc3-109">When you deliver assets to your clients, you can [configure for assets to be dynamically encrypted](media-services-dotnet-configure-asset-delivery-policy.md) with one of the following two encryptions: **DynamicEnvelopeEncryption** or **DynamicCommonEncryption**.</span></span>

<span data-ttu-id="31fc3-110">Krypterade tillgångar måste associeras med **ContentKey**s.</span><span class="sxs-lookup"><span data-stu-id="31fc3-110">Encrypted assets have to be associated with **ContentKey**s.</span></span> <span data-ttu-id="31fc3-111">Den här artikeln beskriver hur du skapar en innehållsnyckel.</span><span class="sxs-lookup"><span data-stu-id="31fc3-111">This article describes how to create a content key.</span></span>

> [!NOTE]
> <span data-ttu-id="31fc3-112">När du skapar en ny **StorageEncrypted** tillgång med Media Services .NET SDK i **ContentKey** skapas automatiskt och länkad till tillgången.</span><span class="sxs-lookup"><span data-stu-id="31fc3-112">When creating a new **StorageEncrypted** asset using the Media Services .NET SDK , the **ContentKey** is automatically created and linked with the asset.</span></span>
> 
> 

## <a name="contentkeytype"></a><span data-ttu-id="31fc3-113">ContentKeyType</span><span class="sxs-lookup"><span data-stu-id="31fc3-113">ContentKeyType</span></span>
<span data-ttu-id="31fc3-114">Ett av de värden som du måste ange när du skapar en innehåll nyckeln är viktiga innehållstypen.</span><span class="sxs-lookup"><span data-stu-id="31fc3-114">One of the values that you must set when create a content key is the content key type.</span></span> <span data-ttu-id="31fc3-115">Välj något av följande värden.</span><span class="sxs-lookup"><span data-stu-id="31fc3-115">Choose from one of the following values.</span></span> 

    public enum ContentKeyType
    {
        /// <summary>
        /// Specifies a content key for common encryption.
        /// </summary>
        /// <remarks>This is the default value.</remarks>
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

## <span data-ttu-id="31fc3-116"><a id="envelope_contentkey"></a>Skapa kuvert typen ContentKey</span><span class="sxs-lookup"><span data-stu-id="31fc3-116"><a id="envelope_contentkey"></a>Create envelope type ContentKey</span></span>
<span data-ttu-id="31fc3-117">Följande kodavsnitt skapar en innehållsnyckel av typen kuvert kryptering.</span><span class="sxs-lookup"><span data-stu-id="31fc3-117">The following code snippet creates a content key of the envelope encryption type.</span></span> <span data-ttu-id="31fc3-118">Sedan associerar den nyckeln med den angivna resursen.</span><span class="sxs-lookup"><span data-stu-id="31fc3-118">It then associates the key with the specified asset.</span></span>

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

<span data-ttu-id="31fc3-119">Anropa</span><span class="sxs-lookup"><span data-stu-id="31fc3-119">call</span></span>

    IContentKey key = CreateEnvelopeTypeContentKey(encryptedsset);



## <span data-ttu-id="31fc3-120"><a id="common_contentkey"></a>Skapa gemensam typ. ContentKey</span><span class="sxs-lookup"><span data-stu-id="31fc3-120"><a id="common_contentkey"></a>Create common type ContentKey</span></span>
<span data-ttu-id="31fc3-121">Följande kodavsnitt skapar en innehållsnyckel av den gemensamma krypteringstypen.</span><span class="sxs-lookup"><span data-stu-id="31fc3-121">The following code snippet creates a content key of the common encryption type.</span></span> <span data-ttu-id="31fc3-122">Sedan associerar den nyckeln med den angivna resursen.</span><span class="sxs-lookup"><span data-stu-id="31fc3-122">It then associates the key with the specified asset.</span></span>

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

        // Associate the key with the asset.
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
<span data-ttu-id="31fc3-123">Anropa</span><span class="sxs-lookup"><span data-stu-id="31fc3-123">call</span></span>

    IContentKey key = CreateCommonTypeContentKey(encryptedsset); 


## <a name="media-services-learning-paths"></a><span data-ttu-id="31fc3-124">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="31fc3-124">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="31fc3-125">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="31fc3-125">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

