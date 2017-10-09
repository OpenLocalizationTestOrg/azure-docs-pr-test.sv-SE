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
# <a name="encrypting-your-content-with-storage-encryption"></a>Kryptera din innehåll med lagringskryptering

Det rekommenderas starkt tooencrypt innehållet lokalt med hjälp av AES 256-bitars kryptering och sedan ladda upp den tooAzure lagring där den lagras krypterat i vila.

Den här artikeln ger en översikt över AMS lagringskryptering och visar hur tooupload hello lagring krypterat innehåll:

* Skapa en innehållsnyckel.
* Skapa en tillgång. Ange hello AssetCreationOption tooStorageEncryption när du skapar hello tillgången.
  
     Krypterade tillgångar ha toobe som är associerade med nycklar för multiinnehåll.
* Länken hello innehåll viktiga toohello tillgången.  
* Ange hello kryptering relaterade parametrar på hello AssetFile entiteter.

## <a name="considerations"></a>Överväganden 

Om du vill toodeliver en krypterad tillgång för lagring, måste du konfigurera hello tillgångsleveransprincip. Innan din tillgång kan strömmas angetts hello streaming server tar bort hello lagringskryptering och dataströmmar innehåll med hello principen för tillgångsleverans. Mer information finns i [konfigurerar principerna för Tillgångsleverans](media-services-rest-configure-asset-delivery-policy.md).

Vid åtkomst till entiteter i Media Services måste du ange specifika namn på huvudfält och värden i HTTP-begäranden. Mer information finns i [installationsprogrammet för Media Services REST API-utveckling](media-services-rest-how-to-use.md). 

## <a name="connect-toomedia-services"></a>Ansluta tooMedia tjänster

Mer information om hur tooconnect toohello AMS API, se [åtkomst hello Azure Media Services API med Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md). 

>[!NOTE]
>När du har anslutit toohttps://media.windows.net, får du en 301 omdirigering att ange en annan Media Services-URI. Du måste göra följande anrop toohello ny URI.

## <a name="storage-encryption-overview"></a>Översikt över kryptering
hello AMS lagringskryptering gäller **AES CTR** läge kryptering toohello hela filen.  AES-CTR läge är ett blockchiffer som kan kryptera godtycklig längddata utan behov av utfyllnad. Det fungerar genom att ett räknaren block med hello AES-algoritmen och XOR ing hello utdata av AES med hello data tooencrypt kryptera eller dekryptera.  hello räknaren block som används har skapats genom att kopiera hello värdet för hello InitializationVector toobytes 0 too7 av hello räknarvärdet och byte 8 too15 av hello räknarvärdet ställs toozero. Hello 16 byte räknaren blockets byte 8 too15 (d.v.s. hello minst viktiga byte) som används som en enkel 64-bitars osignerat heltal som ökar med ett för varje efterföljande datablock bearbetas och lagras i nätverksbyteordning. Observera att om den här heltal når hello maxvärdet (0xFFFFFFFF) öka det återställer hello block räknaren toozero (byte 8 too15) utan att påverka andra hello 64-bitars på räknaren av hello (d.v.s. byte 0 too7).   Hej InitializationVector värde för en given nyckelidentifierare för varje innehållsnyckeln vara unikt för varje fil i ordning toomaintain hello säkerheten för hello AES CTR läge kryptering, och filer bör vara mindre än 2 ^ 64 block i längd.  Detta är tooensure som ett värde för prestandaräknaren är aldrig återanvänds med en viss nyckel. Mer information om hello CTR läge finns [wiki-sida](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CTR) (hello wiki-artikeln använder hello termen ”temporärt ID” i stället för ”InitializationVector”).

Använd **Lagringskryptering** tooencrypt innehållet lokalt med hjälp av AES 256-bitars kryptering och sedan ladda upp den tooAzure lagring där den lagras krypterat i vila. Tillgångar som skyddas med lagringskryptering okrypterad och placeras i en krypterad fil system tidigare tooencoding och eventuellt omkrypterade tidigare toouploading tillbaka som en ny utdatatillgång automatiskt. hello primära användningsfall för lagringskryptering är om du vill toosecure dina indata mediefiler hög kvalitet med stark kryptering i vila på disk.

I ordning toodeliver en krypterad tillgång för lagring, måste du konfigurera hello tillgångsleveransprincip så Media Services vet du hur toodeliver ditt innehåll. Innan din tillgång kan strömmas angetts hello streaming server tar bort hello lagringskryptering och dataströmmar innehåll med hello principen för tillgångsleverans (till exempel AES, vanliga kryptering eller Ingen kryptering).

## <a name="create-contentkeys-used-for-encryption"></a>Skapa ContentKeys som används för kryptering
Krypterade tillgångar ha toobe som är kopplade till krypteringsnyckeln för lagring. Du måste skapa hello innehåll viktiga toobe används för kryptering innan du skapar hello tillgångsfiler. Det här avsnittet beskrivs hur toocreate en innehållsnyckel.

hello är följande allmänna steg för att generera innehåll nycklar som ska associeras med resurser som du vill toobe krypteras. 

1. Generera slumpmässigt en 32-byte AES-nyckel för kryptering. 
   
    Detta blir hello innehållsnyckeln för tillgången, vilket innebär att alla filer som är associerade med tillgången kommer måste toouse hello samma innehållsnyckeln under dekrypteringen. 
2. Anropa hello [GetProtectionKeyId](https://docs.microsoft.com/rest/api/media/operations/rest-api-functions#getprotectionkeyid) och [GetProtectionKey](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkey) metoder tooget hello rätt X.509-certifikat som måste vara används tooencrypt din innehållsnyckeln.
3. Kryptera din innehållsnyckeln med hello offentlig nyckel för hello X.509-certifikat. 
   
   Media Services .NET SDK använder RSA med OAEP när du gör hello kryptering.  Du kan se ett exempel på .NET i hello [EncryptSymmetricKeyData funktionen](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
4. Skapa ett kontrollsummevärde beräknas med hjälp av hello nyckelidentifierare och innehållsnyckeln. hello efter .NET-exempel beräknas hello kontrollsumma använder hello GUID del av hello nyckelidentifierare och hello Rensa innehållsnyckeln.

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

1. Skapa hello innehållsnyckeln med hello **EncryptedContentKey** (konverterat toobase64-kodad sträng) **ProtectionKeyId**, **ProtectionKeyType**,  **ContentKeyType**, och **kontrollsumma** värden som du har fått i föregående steg.

    För lagringskryptering, ska hello följande egenskaper tas med i hello frågans brödtext.

    Egenskapen för brödtext i begäran    | Beskrivning
    ---|---
    Id | hello ContentKey-Id som vi generera oss själva med hjälp av hello följande format ”nb:kid:UUID:<NEW GUID>”.
    ContentKeyType | Detta är hello viktiga innehållstyp som ett heltal för den här nyckeln. Vi skickar hello värdet 1 för kryptering.
    EncryptedContentKey | Vi skapa ett nytt innehåll nyckelvärde som är en 256-bitars (32 byte)-värde. hello nyckeln är krypterad med hello storage kryptering X.509-certifikat som vi hämta från Microsoft Azure Media Services genom att köra en HTTP GET-begäran för hello GetProtectionKeyId och GetProtectionKey metoder. Se exempelvis hello efter .NET-kod: hello **EncryptSymmetricKeyData** metod som definieras [här](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
    ProtectionKeyId | Detta är hello skydd nyckel-id för hello storage kryptering X.509-certifikat som har använt tooencrypt våra innehållsnyckeln.
    ProtectionKeyType | Detta är hello krypteringstyp för hello Skyddsnyckel som har använt tooencrypt hello innehållsnyckeln. Det här värdet är StorageEncryption(1) i vårt exempel.
    Kontrollsumma |hello MD5 beräknade kontrollsumman för hello innehållsnyckeln. Det beräknas genom att kryptera hello innehåll Id med hello innehållsnyckeln. hello exempelkod visar hur toocalculate hello kontrollsumma.


### <a name="retrieve-hello-protectionkeyid"></a>Hämta hello ProtectionKeyId
hello som följande exempel visar hur tooretrieve hello ProtectionKeyId, en certifikat-tumavtrycket för hello certifikat måste du använda när du krypterar din innehållsnyckeln. Göra det här steget toomake till att du redan har hello lämpligt certifikat på din dator.

Begäran:

    GET https://media.windows.net/api/GetProtectionKeyId?contentKeyType=0 HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net

Svar:

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

### <a name="retrieve-hello-protectionkey-for-hello-protectionkeyid"></a>Hämta hello ProtectionKey för hello ProtectionKeyId
hello följande exempel visas hur tooretrieve hello X.509-certifikatet med hello ProtectionKeyId du fick i hello föregående steg.

Begäran:

    GET https://media.windows.net/api/GetProtectionKey?ProtectionKeyId='7D9BB04D9D0A4A24800CADBFEF232689E048F69C' HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    Host: media.windows.net

Svar:

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

### <a name="create-hello-content-key"></a>Skapa hello innehållsnyckel
När du har hämtats hello X.509-certifikat och använda dess offentliga nyckel tooencrypt din innehållsnyckeln, skapa en **ContentKey** entiteten och ange egenskapen värden i enlighet med detta.

Ett av hello-värden som du måste ange när skapa hello innehåll nyckeln är hello-typen. Vid hello lagringskryptering är hello-värdet '1'. 

följande exempel visar hur hello toocreate en **ContentKey** med en **ContentKeyType** för lagringskryptering (”1”) och hello **ProtectionKeyType** ange för ”0” tooindicate som hello skydd nyckeln Id är hello tumavtrycket för X.509-certifikatet.  

Förfrågan

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

Svar:

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

## <a name="create-an-asset"></a>Skapa en tillgång
följande exempel visar hur hello toocreate en tillgång.

**HTTP-begäran**

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

**HTTP-svar**

Om det lyckas, returneras hello följande:

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

## <a name="associate-hello-contentkey-with-an-asset"></a>Associera hello ContentKey med en tillgång
När du har skapat hello ContentKey associera den med din tillgång med hjälp av hello $links åtgärd, som visas i följande exempel hello:

Begäran:

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

Svar:

    HTTP/1.1 204 No Content 

## <a name="create-an-assetfile"></a>Skapa en AssetFile
Hej [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entiteten representerar en video eller ljud-fil som lagras i en blob-behållare. En resursfil är alltid associerat med en tillgång och en tillgång kan innehålla en eller många tillgångsfiler. hello misslyckas Media Services-kodaren om objekttypen tillgången filen inte är associerad med en digital fil i en blob-behållaren.

Observera att hello **AssetFile** instansen och hello faktiska mediefilen är två distinkta objekt. Hej AssetFile instans innehåller metadata om hello mediefilen när hello mediefilen innehåller hello faktiskt medieinnehåll.

När du har överfört din digitala media-fil till en blobbbehållare använder hello **sammanfoga** HTTP-begäran tooupdate hello AssetFile med information om din mediefilen (visas inte i det här avsnittet). 

**HTTP-begäran**

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

**HTTP-svar**

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
