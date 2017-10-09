---
title: "aaaEncrypting ditt innehåll med lagringskryptering med AMS REST API"
description: "Lär dig hur tooencrypt ditt innehåll med lagringskryptering med AMS REST API: er."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: a0a79f3d-76a1-4994-9202-59b91a2230e0
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: d5f8cb8dd1dcded76c9fededccc772d8102ccbad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="encrypting-your-content-with-storage-encryption"></a><span data-ttu-id="5ab0d-103">Kryptera din innehåll med lagringskryptering</span><span class="sxs-lookup"><span data-stu-id="5ab0d-103">Encrypting your content with storage encryption</span></span>

<span data-ttu-id="5ab0d-104">Det rekommenderas starkt tooencrypt innehållet lokalt med hjälp av AES 256-bitars kryptering och sedan ladda upp den tooAzure lagring där den lagras krypterat i vila.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-104">It is highly recommended tooencrypt your content locally using AES-256 bit encryption and then upload it tooAzure Storage where it will be stored encrypted at rest.</span></span>

<span data-ttu-id="5ab0d-105">Den här artikeln ger en översikt över AMS lagringskryptering och visar hur tooupload hello lagring krypterat innehåll:</span><span class="sxs-lookup"><span data-stu-id="5ab0d-105">This article gives an overview of AMS storage encryption and shows you how tooupload hello storage encrypted content:</span></span>

* <span data-ttu-id="5ab0d-106">Skapa en innehållsnyckel.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-106">Create a content key.</span></span>
* <span data-ttu-id="5ab0d-107">Skapa en tillgång.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-107">Create an Asset.</span></span> <span data-ttu-id="5ab0d-108">Ange hello AssetCreationOption tooStorageEncryption när du skapar hello tillgången.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-108">Set hello AssetCreationOption tooStorageEncryption when creating hello Asset.</span></span>
  
     <span data-ttu-id="5ab0d-109">Krypterade tillgångar ha toobe som är associerade med nycklar för multiinnehåll.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-109">Encrypted assets have toobe associated with content keys.</span></span>
* <span data-ttu-id="5ab0d-110">Länken hello innehåll viktiga toohello tillgången.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-110">Link hello content key toohello asset.</span></span>  
* <span data-ttu-id="5ab0d-111">Ange hello kryptering relaterade parametrar på hello AssetFile entiteter.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-111">Set hello encryption related parameters on hello AssetFile entities.</span></span>

## <a name="considerations"></a><span data-ttu-id="5ab0d-112">Överväganden</span><span class="sxs-lookup"><span data-stu-id="5ab0d-112">Considerations</span></span> 

<span data-ttu-id="5ab0d-113">Om du vill toodeliver en krypterad tillgång för lagring, måste du konfigurera hello tillgångsleveransprincip.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-113">If you want toodeliver a storage encrypted asset, you must configure hello asset’s delivery policy.</span></span> <span data-ttu-id="5ab0d-114">Innan din tillgång kan strömmas angetts hello streaming server tar bort hello lagringskryptering och dataströmmar innehåll med hello principen för tillgångsleverans.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-114">Before your asset can be streamed, hello streaming server removes hello storage encryption and streams your content using hello specified delivery policy.</span></span> <span data-ttu-id="5ab0d-115">Mer information finns i [konfigurerar principerna för Tillgångsleverans](media-services-rest-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="5ab0d-115">For more information, see [Configuring Asset Delivery Policies](media-services-rest-configure-asset-delivery-policy.md).</span></span>

<span data-ttu-id="5ab0d-116">Vid åtkomst till entiteter i Media Services måste du ange specifika namn på huvudfält och värden i HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-116">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="5ab0d-117">Mer information finns i [installationsprogrammet för Media Services REST API-utveckling](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="5ab0d-117">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span> 

## <a name="connect-toomedia-services"></a><span data-ttu-id="5ab0d-118">Ansluta tooMedia tjänster</span><span class="sxs-lookup"><span data-stu-id="5ab0d-118">Connect tooMedia Services</span></span>

<span data-ttu-id="5ab0d-119">Mer information om hur tooconnect toohello AMS API, se [åtkomst hello Azure Media Services API med Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="5ab0d-119">For information on how tooconnect toohello AMS API, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="5ab0d-120">När du har anslutit toohttps://media.windows.net, får du en 301 omdirigering att ange en annan Media Services-URI.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-120">After successfully connecting toohttps://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="5ab0d-121">Du måste göra följande anrop toohello ny URI.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-121">You must make subsequent calls toohello new URI.</span></span>

## <a name="storage-encryption-overview"></a><span data-ttu-id="5ab0d-122">Översikt över kryptering</span><span class="sxs-lookup"><span data-stu-id="5ab0d-122">Storage encryption overview</span></span>
<span data-ttu-id="5ab0d-123">hello AMS lagringskryptering gäller **AES CTR** läge kryptering toohello hela filen.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-123">hello AMS storage encryption applies **AES-CTR** mode encryption toohello entire file.</span></span>  <span data-ttu-id="5ab0d-124">AES-CTR läge är ett blockchiffer som kan kryptera godtycklig längddata utan behov av utfyllnad.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-124">AES-CTR mode is a block cipher that can encrypt arbitrary length data without need for padding.</span></span> <span data-ttu-id="5ab0d-125">Det fungerar genom att ett räknaren block med hello AES-algoritmen och XOR ing hello utdata av AES med hello data tooencrypt kryptera eller dekryptera.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-125">It operates by encrypting a counter block with hello AES algorithm and then XOR-ing hello output of AES with hello data tooencrypt or decrypt.</span></span>  <span data-ttu-id="5ab0d-126">hello räknaren block som används har skapats genom att kopiera hello värdet för hello InitializationVector toobytes 0 too7 av hello räknarvärdet och byte 8 too15 av hello räknarvärdet ställs toozero.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-126">hello counter block used is constructed by copying hello value of hello InitializationVector toobytes 0 too7 of hello counter value and bytes 8 too15 of hello counter value are set toozero.</span></span> <span data-ttu-id="5ab0d-127">Hello 16 byte räknaren blockets byte 8 too15 (d.v.s. hello minst viktiga byte) som används som en enkel 64-bitars osignerat heltal som ökar med ett för varje efterföljande datablock bearbetas och lagras i nätverksbyteordning.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-127">Of hello 16 byte counter block, bytes 8 too15 (i.e. hello least significant bytes) are used as a simple 64 bit unsigned integer that is incremented by one for each subsequent block of data processed and is kept in network byte order.</span></span> <span data-ttu-id="5ab0d-128">Observera att om den här heltal når hello maxvärdet (0xFFFFFFFF) öka det återställer hello block räknaren toozero (byte 8 too15) utan att påverka andra hello 64-bitars på räknaren av hello (d.v.s. byte 0 too7).</span><span class="sxs-lookup"><span data-stu-id="5ab0d-128">Note that if this integer reaches hello maximum value (0xFFFFFFFFFFFFFFFF) then incrementing it resets hello block counter toozero (bytes 8 too15) without affecting hello other 64 bits of hello counter (i.e. bytes 0 too7).</span></span>   <span data-ttu-id="5ab0d-129">Hej InitializationVector värde för en given nyckelidentifierare för varje innehållsnyckeln vara unikt för varje fil i ordning toomaintain hello säkerheten för hello AES CTR läge kryptering, och filer bör vara mindre än 2 ^ 64 block i längd.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-129">In order toomaintain hello security of hello AES-CTR mode encryption, hello InitializationVector value for a given Key Identifier for each content key shall be unique for each file and files shall be less than 2^64 blocks in length.</span></span>  <span data-ttu-id="5ab0d-130">Detta är tooensure som ett värde för prestandaräknaren är aldrig återanvänds med en viss nyckel.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-130">This is tooensure that a counter value is never reused with a given key.</span></span> <span data-ttu-id="5ab0d-131">Mer information om hello CTR läge finns [wiki-sida](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CTR) (hello wiki-artikeln använder hello termen ”temporärt ID” i stället för ”InitializationVector”).</span><span class="sxs-lookup"><span data-stu-id="5ab0d-131">For more information about hello CTR mode, see [this wiki page](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CTR) (hello wiki article uses hello term "Nonce" instead of "InitializationVector").</span></span>

<span data-ttu-id="5ab0d-132">Använd **Lagringskryptering** tooencrypt innehållet lokalt med hjälp av AES 256-bitars kryptering och sedan ladda upp den tooAzure lagring där den lagras krypterat i vila.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-132">Use **Storage Encryption** tooencrypt your clear content locally using AES-256 bit encryption and then upload it tooAzure Storage where it is stored encrypted at rest.</span></span> <span data-ttu-id="5ab0d-133">Tillgångar som skyddas med lagringskryptering okrypterad och placeras i en krypterad fil system tidigare tooencoding och eventuellt omkrypterade tidigare toouploading tillbaka som en ny utdatatillgång automatiskt.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-133">Assets protected with storage encryption are automatically unencrypted and placed in an encrypted file system prior tooencoding, and optionally re-encrypted prior toouploading back as a new output asset.</span></span> <span data-ttu-id="5ab0d-134">hello primära användningsfall för lagringskryptering är om du vill toosecure dina indata mediefiler hög kvalitet med stark kryptering i vila på disk.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-134">hello primary use case for storage encryption is when you want toosecure your high quality input media files with strong encryption at rest on disk.</span></span>

<span data-ttu-id="5ab0d-135">I ordning toodeliver en krypterad tillgång för lagring, måste du konfigurera hello tillgångsleveransprincip så Media Services vet du hur toodeliver ditt innehåll.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-135">In order toodeliver a storage encrypted asset, you must configure hello asset’s delivery policy so Media Services knows how you want toodeliver your content.</span></span> <span data-ttu-id="5ab0d-136">Innan din tillgång kan strömmas angetts hello streaming server tar bort hello lagringskryptering och dataströmmar innehåll med hello principen för tillgångsleverans (till exempel AES, vanliga kryptering eller Ingen kryptering).</span><span class="sxs-lookup"><span data-stu-id="5ab0d-136">Before your asset can be streamed, hello streaming server removes hello storage encryption and streams your content using hello specified delivery policy (for example, AES, common encryption, or no encryption).</span></span>

## <a name="create-contentkeys-used-for-encryption"></a><span data-ttu-id="5ab0d-137">Skapa ContentKeys som används för kryptering</span><span class="sxs-lookup"><span data-stu-id="5ab0d-137">Create ContentKeys used for encryption</span></span>
<span data-ttu-id="5ab0d-138">Krypterade tillgångar ha toobe som är kopplade till krypteringsnyckeln för lagring.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-138">Encrypted assets have toobe associated with Storage Encryption key.</span></span> <span data-ttu-id="5ab0d-139">Du måste skapa hello innehåll viktiga toobe används för kryptering innan du skapar hello tillgångsfiler.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-139">You must create hello content key toobe used for encryption before creating hello asset files.</span></span> <span data-ttu-id="5ab0d-140">Det här avsnittet beskrivs hur toocreate en innehållsnyckel.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-140">This section describes how toocreate a content key.</span></span>

<span data-ttu-id="5ab0d-141">hello är följande allmänna steg för att generera innehåll nycklar som ska associeras med resurser som du vill toobe krypteras.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-141">hello following are general steps for generating content keys that you will associate with assets that you want toobe encrypted.</span></span> 

1. <span data-ttu-id="5ab0d-142">Generera slumpmässigt en 32-byte AES-nyckel för kryptering.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-142">For storage encryption, randomly generate a 32-byte AES key.</span></span> 
   
    <span data-ttu-id="5ab0d-143">Detta blir hello innehållsnyckeln för tillgången, vilket innebär att alla filer som är associerade med tillgången kommer måste toouse hello samma innehållsnyckeln under dekrypteringen.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-143">This will be hello content key for your asset, which means all files associated with that asset will need toouse hello same content key during decryption.</span></span> 
2. <span data-ttu-id="5ab0d-144">Anropa hello [GetProtectionKeyId](https://docs.microsoft.com/rest/api/media/operations/rest-api-functions#getprotectionkeyid) och [GetProtectionKey](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkey) metoder tooget hello rätt X.509-certifikat som måste vara används tooencrypt din innehållsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-144">Call hello [GetProtectionKeyId](https://docs.microsoft.com/rest/api/media/operations/rest-api-functions#getprotectionkeyid) and [GetProtectionKey](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkey) methods tooget hello correct X.509 Certificate that must be used tooencrypt your content key.</span></span>
3. <span data-ttu-id="5ab0d-145">Kryptera din innehållsnyckeln med hello offentlig nyckel för hello X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-145">Encrypt your content key with hello public key of hello X.509 Certificate.</span></span> 
   
   <span data-ttu-id="5ab0d-146">Media Services .NET SDK använder RSA med OAEP när du gör hello kryptering.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-146">Media Services .NET SDK uses RSA with OAEP when doing hello encryption.</span></span>  <span data-ttu-id="5ab0d-147">Du kan se ett exempel på .NET i hello [EncryptSymmetricKeyData funktionen](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).</span><span class="sxs-lookup"><span data-stu-id="5ab0d-147">You can see a .NET example in hello [EncryptSymmetricKeyData function](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).</span></span>
4. <span data-ttu-id="5ab0d-148">Skapa ett kontrollsummevärde beräknas med hjälp av hello nyckelidentifierare och innehållsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-148">Create a checksum value calculated using hello key identifier and content key.</span></span> <span data-ttu-id="5ab0d-149">hello efter .NET-exempel beräknas hello kontrollsumma använder hello GUID del av hello nyckelidentifierare och hello Rensa innehållsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-149">hello following .NET example calculates hello checksum using hello GUID part of hello key identifier and hello clear content key.</span></span>

        public static string CalculateChecksum(byte[] contentKey, Guid keyId)
        {
            const int ChecksumLength = 8;
            const int KeyIdLength = 16;

            byte[] encryptedKeyId = null;

            // Checksum is computed by AES-ECB encrypting hello KID
            // with hello content key.
            using (AesCryptoServiceProvider rijndael = new AesCryptoServiceProvider())
            {
                rijndael.Mode = CipherMode.ECB;
                rijndael.Key = contentKey;
                rijndael.Padding = PaddingMode.None;

                ICryptoTransform encryptor = rijndael.CreateEncryptor();
                encryptedKeyId = new byte[KeyIdLength];
                encryptor.TransformBlock(keyId.ToByteArray(), 0, KeyIdLength, encryptedKeyId, 0);
            }

            byte[] retVal = new byte[ChecksumLength];
            Array.Copy(encryptedKeyId, retVal, ChecksumLength);

            return Convert.ToBase64String(retVal);
        }

1. <span data-ttu-id="5ab0d-150">Skapa hello innehållsnyckeln med hello **EncryptedContentKey** (konverterat toobase64-kodad sträng) **ProtectionKeyId**, **ProtectionKeyType**,  **ContentKeyType**, och **kontrollsumma** värden som du har fått i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-150">Create hello Content key with hello **EncryptedContentKey** (converted toobase64-encoded string), **ProtectionKeyId**, **ProtectionKeyType**, **ContentKeyType**, and **Checksum** values you have received in previous steps.</span></span>

    <span data-ttu-id="5ab0d-151">För lagringskryptering, ska hello följande egenskaper tas med i hello frågans brödtext.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-151">For storage encryption, hello following properties should be included in hello request body.</span></span>

    <span data-ttu-id="5ab0d-152">Egenskapen för brödtext i begäran</span><span class="sxs-lookup"><span data-stu-id="5ab0d-152">Request body property</span></span>    | <span data-ttu-id="5ab0d-153">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5ab0d-153">Description</span></span>
    ---|---
    <span data-ttu-id="5ab0d-154">Id</span><span class="sxs-lookup"><span data-stu-id="5ab0d-154">Id</span></span> | <span data-ttu-id="5ab0d-155">hello ContentKey-Id som vi generera oss själva med hjälp av hello följande format ”nb:kid:UUID:<NEW GUID>”.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-155">hello ContentKey Id which we generate ourselves using hello following format, “nb:kid:UUID:<NEW GUID>”.</span></span>
    <span data-ttu-id="5ab0d-156">ContentKeyType</span><span class="sxs-lookup"><span data-stu-id="5ab0d-156">ContentKeyType</span></span> | <span data-ttu-id="5ab0d-157">Detta är hello viktiga innehållstyp som ett heltal för den här nyckeln.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-157">This is hello content key type as an integer for this content key.</span></span> <span data-ttu-id="5ab0d-158">Vi skickar hello värdet 1 för kryptering.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-158">We pass hello value 1 for storage encryption.</span></span>
    <span data-ttu-id="5ab0d-159">EncryptedContentKey</span><span class="sxs-lookup"><span data-stu-id="5ab0d-159">EncryptedContentKey</span></span> | <span data-ttu-id="5ab0d-160">Vi skapa ett nytt innehåll nyckelvärde som är en 256-bitars (32 byte)-värde.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-160">We create a new content key value which is a 256-bit (32 byte) value.</span></span> <span data-ttu-id="5ab0d-161">hello nyckeln är krypterad med hello storage kryptering X.509-certifikat som vi hämta från Microsoft Azure Media Services genom att köra en HTTP GET-begäran för hello GetProtectionKeyId och GetProtectionKey metoder.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-161">hello key is encrypted using hello storage encryption X.509 certificate which we retrieve from Microsoft Azure Media Services by executing a HTTP GET request for hello GetProtectionKeyId and GetProtectionKey Methods.</span></span> <span data-ttu-id="5ab0d-162">Se exempelvis hello efter .NET-kod: hello **EncryptSymmetricKeyData** metod som definieras [här](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).</span><span class="sxs-lookup"><span data-stu-id="5ab0d-162">As an example, see hello following .NET code: hello  **EncryptSymmetricKeyData** method defined [here](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).</span></span>
    <span data-ttu-id="5ab0d-163">ProtectionKeyId</span><span class="sxs-lookup"><span data-stu-id="5ab0d-163">ProtectionKeyId</span></span> | <span data-ttu-id="5ab0d-164">Detta är hello skydd nyckel-id för hello storage kryptering X.509-certifikat som har använt tooencrypt våra innehållsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-164">This is hello protection key id for hello storage encryption X.509 certificate that was used tooencrypt our content key.</span></span>
    <span data-ttu-id="5ab0d-165">ProtectionKeyType</span><span class="sxs-lookup"><span data-stu-id="5ab0d-165">ProtectionKeyType</span></span> | <span data-ttu-id="5ab0d-166">Detta är hello krypteringstyp för hello Skyddsnyckel som har använt tooencrypt hello innehållsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-166">This is hello encryption type for hello protection key that was used tooencrypt hello content key.</span></span> <span data-ttu-id="5ab0d-167">Det här värdet är StorageEncryption(1) i vårt exempel.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-167">This value is StorageEncryption(1) for our example.</span></span>
    <span data-ttu-id="5ab0d-168">Kontrollsumma</span><span class="sxs-lookup"><span data-stu-id="5ab0d-168">Checksum</span></span> |<span data-ttu-id="5ab0d-169">hello MD5 beräknade kontrollsumman för hello innehållsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-169">hello MD5 calculated checksum for hello content key.</span></span> <span data-ttu-id="5ab0d-170">Det beräknas genom att kryptera hello innehåll Id med hello innehållsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-170">It is computed by encrypting hello content Id with hello content key.</span></span> <span data-ttu-id="5ab0d-171">hello exempelkod visar hur toocalculate hello kontrollsumma.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-171">hello example code demonstrates how toocalculate hello checksum.</span></span>


### <a name="retrieve-hello-protectionkeyid"></a><span data-ttu-id="5ab0d-172">Hämta hello ProtectionKeyId</span><span class="sxs-lookup"><span data-stu-id="5ab0d-172">Retrieve hello ProtectionKeyId</span></span>
<span data-ttu-id="5ab0d-173">hello som följande exempel visar hur tooretrieve hello ProtectionKeyId, en certifikat-tumavtrycket för hello certifikat måste du använda när du krypterar din innehållsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-173">hello following example shows how tooretrieve hello ProtectionKeyId, a certificate thumbprint, for hello certificate you must use when encrypting your content key.</span></span> <span data-ttu-id="5ab0d-174">Göra det här steget toomake till att du redan har hello lämpligt certifikat på din dator.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-174">Do this step toomake sure that you already have hello appropriate certificate on your machine.</span></span>

<span data-ttu-id="5ab0d-175">Begäran:</span><span class="sxs-lookup"><span data-stu-id="5ab0d-175">Request:</span></span>

    GET https://media.windows.net/api/GetProtectionKeyId?contentKeyType=0 HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net

<span data-ttu-id="5ab0d-176">Svar:</span><span class="sxs-lookup"><span data-stu-id="5ab0d-176">Response:</span></span>

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 139
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    x-ms-request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:42:52 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String","value":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C"}

### <a name="retrieve-hello-protectionkey-for-hello-protectionkeyid"></a><span data-ttu-id="5ab0d-177">Hämta hello ProtectionKey för hello ProtectionKeyId</span><span class="sxs-lookup"><span data-stu-id="5ab0d-177">Retrieve hello ProtectionKey for hello ProtectionKeyId</span></span>
<span data-ttu-id="5ab0d-178">hello följande exempel visas hur tooretrieve hello X.509-certifikatet med hello ProtectionKeyId du fick i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-178">hello following example shows how tooretrieve hello X.509 certificate using hello ProtectionKeyId you received in hello previous step.</span></span>

<span data-ttu-id="5ab0d-179">Begäran:</span><span class="sxs-lookup"><span data-stu-id="5ab0d-179">Request:</span></span>

    GET https://media.windows.net/api/GetProtectionKey?ProtectionKeyId='7D9BB04D9D0A4A24800CADBFEF232689E048F69C' HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    Host: media.windows.net

<span data-ttu-id="5ab0d-180">Svar:</span><span class="sxs-lookup"><span data-stu-id="5ab0d-180">Response:</span></span>

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 1227
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    x-ms-request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Thu, 05 Feb 2015 07:52:30 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String",
    "value":"MIIDSTCCAjGgAwIBAgIQqf92wku/HLJGCbMAU8GEnDANBgkqhkiG9w0BAQQFADAuMSwwKgYDVQQDEyN3YW1zYmx1cmVnMDAxZW5jcnlwdGFsbHNlY3JldHMtY2VydDAeFw0xMjA1MjkwNzAwMDBaFw0zMjA1MjkwNzAwMDBaMC4xLDAqBgNVBAMTI3dhbXNibHVyZWcwMDFlbmNyeXB0YWxsc2VjcmV0cy1jZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAzR0SEbXefvUjb9wCUfkEiKtGQ5Gc328qFPrhMjSo+YHe0AVviZ9YaxPPb0m1AaaRV4dqWpST2+JtDhLOmGpWmmA60tbATJDdmRzKi2eYAyhhE76MgJgL3myCQLP42jDusWXWSMabui3/tMDQs+zfi1sJ4Ch/lm5EvksYsu6o8sCv29VRwxfDLJPBy2NlbV4GbWz5Qxp2tAmHoROnfaRhwp6WIbquk69tEtu2U50CpPN2goLAqx2PpXAqA+prxCZYGTHqfmFJEKtZHhizVBTFPGS3ncfnQC9QIEwFbPw6E5PO5yNaB68radWsp5uvDg33G1i8IT39GstMW6zaaG7cNQIDAQABo2MwYTBfBgNVHQEEWDBWgBCOGT2hPhsvQioZimw8M+jOoTAwLjEsMCoGA1UEAxMjd2Ftc2JsdXJlZzAwMWVuY3J5cHRhbGxzZWNyZXRzLWNlcnSCEKn/dsJLvxyyRgmzAFPBhJwwDQYJKoZIhvcNAQEEBQADggEBABcrQPma2ekNS3Wc5wGXL/aHyQaQRwFGymnUJ+VR8jVUZaC/U/f6lR98eTlwycjVwRL7D15BfClGEHw66QdHejaViJCjbEIJJ3p2c9fzBKhjLhzB3VVNiLIaH6RSI1bMPd2eddSCqhDIn3VBN605GcYXMzhYp+YA6g9+YMNeS1b+LxX3fqixMQIxSHOLFZ1G/H2xfNawv0VikH3djNui3EKT1w/8aRkUv/AAV0b3rYkP/jA1I0CPn0XFk7STYoiJ3gJoKq9EMXhit+Iwfz0sMkfhWG12/XO+TAWqsK1ZxEjuC9OzrY7pFnNxs4Mu4S8iinehduSpY+9mDd3dHynNwT4="}

### <a name="create-hello-content-key"></a><span data-ttu-id="5ab0d-181">Skapa hello innehållsnyckel</span><span class="sxs-lookup"><span data-stu-id="5ab0d-181">Create hello content key</span></span>
<span data-ttu-id="5ab0d-182">När du har hämtats hello X.509-certifikat och använda dess offentliga nyckel tooencrypt din innehållsnyckeln, skapa en **ContentKey** entiteten och ange egenskapen värden i enlighet med detta.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-182">After you have retrieved hello X.509 certificate and used its public key tooencrypt your content key, create a **ContentKey** entity and set its property values accordingly.</span></span>

<span data-ttu-id="5ab0d-183">Ett av hello-värden som du måste ange när skapa hello innehåll nyckeln är hello-typen.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-183">One of hello values that you must set when create hello content key is hello type.</span></span> <span data-ttu-id="5ab0d-184">Vid hello lagringskryptering är hello-värdet '1'.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-184">In case of hello storage encryption, hello value is '1'.</span></span> 

<span data-ttu-id="5ab0d-185">följande exempel visar hur hello toocreate en **ContentKey** med en **ContentKeyType** för lagringskryptering (”1”) och hello **ProtectionKeyType** ange för ”0” tooindicate som hello skydd nyckeln Id är hello tumavtrycket för X.509-certifikatet.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-185">hello following example shows how toocreate a **ContentKey** with a **ContentKeyType** set for storage encryption ("1") and hello **ProtectionKeyType** set too"0" tooindicate that hello protection key Id is hello X.509 certificate thumbprint.</span></span>  

<span data-ttu-id="5ab0d-186">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="5ab0d-186">Request</span></span>

    POST https://media.windows.net/api/ContentKeys HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net
    {
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C", 
    "ContentKeyType":"1", 
    "ProtectionKeyType":"0",
    "EncryptedContentKey":"your encrypted content key",
    "Checksum":"calculated checksum"
    }

<span data-ttu-id="5ab0d-187">Svar:</span><span class="sxs-lookup"><span data-stu-id="5ab0d-187">Response:</span></span>

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 777
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/ContentKeys('nb%3Akid%3AUUID%3A9c8ea9c6-52bd-4232-8a43-8e43d8564a99')
    Server: Microsoft-IIS/8.5
    request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    x-ms-request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:37:46 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeys/@Element",
    "Id":"nb:kid:UUID:9c8ea9c6-52bd-4232-8a43-8e43d8564a99","Created":"2015-02-04T02:37:46.9684379Z",
    "LastModified":"2015-02-04T02:37:46.9684379Z",
    "ContentKeyType":1,
    "EncryptedContentKey":"your encrypted content key",
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C",
    "ProtectionKeyType":0,
    "Checksum":"calculated checksum"}

## <a name="create-an-asset"></a><span data-ttu-id="5ab0d-188">Skapa en tillgång</span><span class="sxs-lookup"><span data-stu-id="5ab0d-188">Create an asset</span></span>
<span data-ttu-id="5ab0d-189">följande exempel visar hur hello toocreate en tillgång.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-189">hello following example shows how toocreate an asset.</span></span>

<span data-ttu-id="5ab0d-190">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="5ab0d-190">**HTTP Request**</span></span>

    POST https://media.windows.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net

    {"Name":"BigBuckBunny" "Options":1}

<span data-ttu-id="5ab0d-191">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="5ab0d-191">**HTTP Response**</span></span>

<span data-ttu-id="5ab0d-192">Om det lyckas, returneras hello följande:</span><span class="sxs-lookup"><span data-stu-id="5ab0d-192">If successful, hello following is returned:</span></span>

    HTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 452
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    request-id: e98be122-ae09-473a-8072-0ccd234a0657
    x-ms-request-id: e98be122-ae09-473a-8072-0ccd234a0657
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny.mp4",
       "Options":1,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }

## <a name="associate-hello-contentkey-with-an-asset"></a><span data-ttu-id="5ab0d-193">Associera hello ContentKey med en tillgång</span><span class="sxs-lookup"><span data-stu-id="5ab0d-193">Associate hello ContentKey with an Asset</span></span>
<span data-ttu-id="5ab0d-194">När du har skapat hello ContentKey associera den med din tillgång med hjälp av hello $links åtgärd, som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="5ab0d-194">After creating hello ContentKey, associate it with your Asset using hello $links operation, as shown in hello following example:</span></span>

<span data-ttu-id="5ab0d-195">Begäran:</span><span class="sxs-lookup"><span data-stu-id="5ab0d-195">Request:</span></span>

    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Afbd7ce05-1087-401b-aaae-29f16383c801')/$links/ContentKeys HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    Host: media.windows.net

    {"uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeys('nb%3Akid%3AUUID%3A01e6ea36-2285-4562-91f1-82c45736047c')"}

<span data-ttu-id="5ab0d-196">Svar:</span><span class="sxs-lookup"><span data-stu-id="5ab0d-196">Response:</span></span>

    HTTP/1.1 204 No Content 

## <a name="create-an-assetfile"></a><span data-ttu-id="5ab0d-197">Skapa en AssetFile</span><span class="sxs-lookup"><span data-stu-id="5ab0d-197">Create an AssetFile</span></span>
<span data-ttu-id="5ab0d-198">Hej [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entiteten representerar en video eller ljud-fil som lagras i en blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-198">hello [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entity represents a video or audio file that is stored in a blob container.</span></span> <span data-ttu-id="5ab0d-199">En resursfil är alltid associerat med en tillgång och en tillgång kan innehålla en eller många tillgångsfiler.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-199">An asset file is always associated with an asset, and an asset may contain one or many asset files.</span></span> <span data-ttu-id="5ab0d-200">hello misslyckas Media Services-kodaren om objekttypen tillgången filen inte är associerad med en digital fil i en blob-behållaren.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-200">hello Media Services Encoder task fails if an asset file object is not associated with a digital file in a blob container.</span></span>

<span data-ttu-id="5ab0d-201">Observera att hello **AssetFile** instansen och hello faktiska mediefilen är två distinkta objekt.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-201">Note that hello **AssetFile** instance and hello actual media file are two distinct objects.</span></span> <span data-ttu-id="5ab0d-202">Hej AssetFile instans innehåller metadata om hello mediefilen när hello mediefilen innehåller hello faktiskt medieinnehåll.</span><span class="sxs-lookup"><span data-stu-id="5ab0d-202">hello AssetFile instance contains metadata about hello media file, while hello media file contains hello actual media content.</span></span>

<span data-ttu-id="5ab0d-203">När du har överfört din digitala media-fil till en blobbbehållare använder hello **sammanfoga** HTTP-begäran tooupdate hello AssetFile med information om din mediefilen (visas inte i det här avsnittet).</span><span class="sxs-lookup"><span data-stu-id="5ab0d-203">After you upload your digital media file into a blob container, you will use hello **MERGE** HTTP request tooupdate hello AssetFile with information about your media file (not shown in this topic).</span></span> 

<span data-ttu-id="5ab0d-204">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="5ab0d-204">**HTTP Request**</span></span>

    POST https://media.windows.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    Content-Length: 164

    {  
       "IsEncrypted":"true",
       "EncryptionScheme" : "StorageEncryption", 
       "EncryptionVersion" : "1.0",       
       "EncryptionKeyId" : "nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector" : "397304628502661816</d:InitializationVector",
       "Options":0,
       "IsPrimary":"false",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }

<span data-ttu-id="5ab0d-205">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="5ab0d-205">**HTTP Response**</span></span>

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 535
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5')
    Server: Microsoft-IIS/8.5
    request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    x-ms-request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 00:34:07 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Files/@Element",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "Name":"BigBuckBunny.mp4",
       "ContentFileSize":"0",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "EncryptionVersion": "1.0",
       "EncryptionScheme": "StorageEncryption",
       "IsEncrypted":true,
       "EncryptionKeyId":"nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector":"397304628502661816</d:InitializationVector",
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }
