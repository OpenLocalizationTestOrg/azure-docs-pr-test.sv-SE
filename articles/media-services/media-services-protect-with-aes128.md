---
title: "aaaUsing AES-128 dynamisk kryptering och nyckeln leverans av tjänsten | Microsoft Docs"
description: "Microsoft Azure Media Services kan du toodeliver ditt innehåll som krypterats med AES 128-bitars krypteringsnycklar. Media Services tillhandahåller också hello nyckeln Delivery service som ger dig kryptering nycklar tooauthorized användare. Det här avsnittet visar hur toodynamically kryptera med AES-128 och använda hello viktiga delivery service."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 4d2c10af-9ee0-408f-899b-33fa4c1d89b9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: juliako
ms.openlocfilehash: cb1b413ec2ba79f7437464099cf72236ab93f312
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-aes-128-dynamic-encryption-and-key-delivery-service"></a>Med hjälp av AES-128 dynamisk kryptering och viktiga leveranser
> [!div class="op_single_selector"]
> * [.NET](media-services-protect-with-aes128.md)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a>Översikt
> [!NOTE]
> Se [detta](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-Protecting-your-Media-Content-with-AES-Encryption) video för en översikt över hur tooprotect Media innehåll med AES-kryptering.
> 
> 

Microsoft Azure Media Services kan du toodeliver Http-Live-Streaming (HLS) och Smooth strömmar som krypterats med Standard AES (Advanced Encryption) (med 128-bitars krypteringsnycklar). Media Services tillhandahåller också hello nyckeln Delivery service som ger dig kryptering nycklar tooauthorized användare. Om du vill använda för Media Services tooencrypt en tillgång, behöver tooassociate en krypteringsnyckel med hello tillgången och även konfigurera auktoriseringsprinciper för hello nyckeln. När en dataströmmen har begärts av en spelare, använder Media Services hello angetts viktiga toodynamically kryptera ditt innehåll med hjälp av AES-kryptering. toodecrypt hello dataströmmen hello player begär hello nyckeln från hello viktiga leverans av tjänsten. toodecide om huruvida användaren hello är godkänd tooget hello nyckel, hello tjänsten utvärderar hello auktoriseringsprinciper som du angav för hello nyckeln.

Media Services stöder flera olika sätt att auktorisera användare som begär nycklar. Hej innehållsnyckelns auktoriseringsprincip kan ha en eller flera auktoriseringsbegränsningar: öppen eller tokenbegränsning. Hej tokenbegränsade principen måste åtföljas av en token som utfärdas av en säker säkerhetstokentjänst (STS). Media Services stöder token i hello [Simple Web Tokens](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) format och [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT)-format. Mer information finns i [konfigurera hello innehållsnyckelns auktoriseringsprincip](media-services-protect-with-aes128.md#configure_key_auth_policy).

tootake nytta av dynamisk kryptering behöver du toohave en tillgång som innehåller en uppsättning med flera bithastigheter MP4-filer eller Smooth Streaming källfiler i multibithastighet. Du måste också tooconfigure hello leveransprincipen för hello tillgången (beskrivs senare i det här avsnittet). Sedan, baserat på hello-format som specificerats i hello strömnings-URL, servern för strömning på begäran av hello säkerställer hello dataströmmen levereras i hello-protokollet som du har valt. Därför behöver du bara toostore och betala för hello filer i ett enda lagringsformat och Media Services-tjänsten skapar och ger hello lämplig respons baserat på begäranden från en klient.

Det här avsnittet är användbara toodevelopers som arbetar på appar som levererar media som är skyddade. hello avsnittet visar hur tooconfigure hello viktiga leverans av tjänsten med auktoriseringsprinciper så att endast auktoriserade klienter kan få hello krypteringsnycklar. Den visar även hur toouse dynamisk kryptering.


## <a name="aes-128-dynamic-encryption-and-key-delivery-service-workflow"></a>AES-128 dynamisk kryptering och viktiga Delivery Service arbetsflöde

hello följande är allmänna steg att tooperform för att kryptera dina tillgångar med AES hello Media Services viktiga delivery service, samt även använder dynamisk kryptering.

1. [Skapa en tillgång och överföra filer till tillgången hello](media-services-protect-with-aes128.md#create_asset).
2. [Koda hello tillgång som innehåller hello filen toohello anpassningsbar bithastighet MP4-uppsättningen](media-services-protect-with-aes128.md#encode_asset).
3. [Skapa en innehållsnyckel och associera den med hello kodade tillgången](media-services-protect-with-aes128.md#create_contentkey). I Media Services innehåller innehållsnyckeln hello hello tillgångens krypteringsnyckel.
4. [Konfigurera hello innehållsnyckelns auktoriseringsprincip](media-services-protect-with-aes128.md#configure_key_auth_policy). Hej innehållsnyckelns auktoriseringsprincip måste konfigureras av dig och uppfyllas av hello klienten för hello innehåll viktiga toobe levererat toohello klienten.
5. [Konfigurera hello leveransprincipen för en tillgång](media-services-protect-with-aes128.md#configure_asset_delivery_policy). hello konfigurationen för leveransprincipen omfattar: URL för anskaffning och initiering Vector (IV) (AES 128 kräver hello samma IV toobe angavs när kryptera och dekryptera), leveransprotokoll (till exempel MPEG DASH, HLS, Smooth Streaming eller alla), hello typ av dynamisk kryptering (till exempel kuvert eller ingen dynamisk kryptering).

    Du kan använda olika principer tooeach protokoll på hello samma tillgång. Du kan till exempel tillämpa PlayReady-kryptering tooSmooth/DASH och AES Envelope tooHLS. Alla protokoll som inte har definierats i en leveransprincip (exempelvis du lägga till en enda princip som endast anger HLS som hello protokoll) kommer att blockeras från strömning. hello undantag toothis är om du har någon tillgångsleveransprincip alls definierats. Sedan tillåts alla protokoll i hello Rensa.

6. [Skapa en OnDemand-positionerare](media-services-protect-with-aes128.md#create_locator) ordning tooget en strömnings-URL.

hello avsnittet visar också [hur ett klientprogram kan begära en nyckel från hello viktiga leverans tjänsten](media-services-protect-with-aes128.md#client_request).

Du hittar ett komplett .NET [exempel](media-services-protect-with-aes128.md#example) hello slutet av hello-avsnittet.

hello följande bild visar hello arbetsflödet som beskrivs ovan. Här används hello token för autentisering.

![Skydda med AES-128](./media/media-services-content-protection-overview/media-services-content-protection-with-aes.png)

hello resten av det här avsnittet innehåller detaljerade förklaringar, kodexempel och länkar tootopics som visar hur tooachieve hello uppgifter som beskrivs ovan.

## <a name="current-limitations"></a>Aktuella begränsningar
Om du lägger till eller uppdaterar din tillgångs leveransprincip måste du ta bort en befintlig lokaliserare (om sådan finns) och skapa en ny.

## <a id="create_asset"></a>Skapa en tillgång och överföra filer till hello tillgång
I ordning toomanage, koda och strömma videor, måste du först överföra innehållet till Microsoft Azure Media Services. När du har överfört är ditt innehåll lagras på ett säkert sätt i hello molnet för vidare bearbetning och strömning. 

Utförlig information finns i [Överföra filer till ett Media Services-konto](media-services-dotnet-upload-files.md).

## <a id="encode_asset"></a>Koda hello tillgång som innehåller hello filen toohello anpassningsbar bithastighet MP4-uppsättningen
Med dynamisk kryptering är allt du behöver toocreate en tillgång som innehåller en uppsättning med flera bithastigheter MP4-filer eller Smooth Streaming källfiler i multibithastighet. Sedan, baserat på hello format som anges i manifestet hello eller Fragmentera begäran, hello strömning på begäran kommer servern att säkerställa att du har fått hello dataström i hello-protokollet som du har valt. Därför behöver du bara toostore och betala för hello filer i ett enda lagringsformat och Media Services-tjänsten skapar och ger hello lämplig respons baserat på begäranden från en klient. Mer information finns i hello [översikt över dynamisk paketering](media-services-dynamic-packaging-overview.md) avsnittet.

>[!NOTE]
>När AMS-kontot skapas en **standard** strömningsslutpunkt har lagts till tooyour konto i hello **stoppad** tillstånd. toostart strömning ditt innehåll och dra nytta av dynamisk paketering och dynamisk kryptering hello strömningsslutpunkt som du vill toostream innehåll har toobe i hello **kör** tillstånd. 
>
>Dessutom toobe kan toouse dynamisk paketering och dynamisk kryptering din tillgång måste innehålla en uppsättning MP4s med anpassningsbar bithastighet eller Smooth Streaming-filer.

Mer information om hur tooencode, se [hur tooencode en tillgång med Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md).

## <a id="create_contentkey"></a>Skapa en innehållsnyckel och associera den med hello kodade tillgången
I Media Services innehåller innehållsnyckeln för hello hello-nyckel som du vill tooencrypt en tillgång med.

Utförlig information finns i [Skapa en innehållsnyckel](media-services-dotnet-create-contentkey.md).

## <a id="configure_key_auth_policy"></a>Konfigurera hello innehållsnyckelns auktoriseringsprincip
Media Services stöder flera olika sätt att auktorisera användare som begär nycklar. Hej innehållsnyckelns auktoriseringsprincip måste konfigureras av dig och uppfyllas av hello klienten (spelaren) för hello viktiga toobe levereras toohello klienten. Hej innehållsnyckelns auktoriseringsprincip kan ha en eller flera auktoriseringsbegränsningar: öppen och token begränsning eller en IP-begränsning.

Mer information finns i [Konfigurera  innehållsnyckelns auktoriseringsprincip](media-services-dotnet-configure-content-key-auth-policy.md).

## <a id="configure_asset_delivery_policy"></a>Konfigurera tillgångsleveransprincip
Konfigurera hello leveransprincipen för din tillgång. Vissa saker som hello tillgången konfigurationen för leveransprincipen omfattar:

* URL för anskaffning av hello nyckel. 
* hello initieringen Vector (IV) toouse för hello kuvert kryptering. AES-128 kräver hello samma IV toobe angavs när kryptera och dekryptera. 
* hello protokollet för tillgångsleverans (till exempel MPEG DASH, HLS, Smooth Streaming eller alla).
* hello typen av dynamisk kryptering (till exempel AES envelope) eller ingen dynamisk kryptering. 

Detaljerad information finns i [Konfigurera tillgångsleveransprincip ](media-services-rest-configure-asset-delivery-policy.md).

## <a id="create_locator"></a>Skapa en OnDemand-strömning positionerare ordning tooget en strömnings-URL
Du behöver tooprovide användaren hello strömmande URL för Smooth, DASH eller HLS.

> [!NOTE]
> Om du lägger till eller uppdaterar din tillgångs leveransprincip måste du ta bort en befintlig lokaliserare (om sådan finns) och skapa en ny.
> 
> 

Anvisningar för hur toopublish en tillgång och skapa en strömnings-URL, se [skapa en strömnings-URL](media-services-deliver-streaming-content.md).

## <a name="get-a-test-token"></a>Hämta en testtoken
Hämta en testtoken baserat på hello tokenbegränsningar som användes för hello innehållsnyckelns auktoriseringsprincip.

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate = 
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

    // Generate a test token based on hello data in hello given TokenRestrictionTemplate.
    //hello GenerateTestToken method returns hello token without hello word “Bearer” in front
    //so you have tooadd it in front of hello token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate);
    Console.WriteLine("hello authorization token is:\nBearer {0}", testToken);

Du kan använda hello [AMS Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) tootest strömmen.

## <a id="client_request"></a>Hur kan klienten begär en nyckel från hello viktiga leverans tjänsten?
I föregående steg hello konstrueras hello-URL som pekar tooa manifestfilen. Klienten måste tooextract hello nödvändig information från hello strömning manifestfiler i ordning toomake en begäran toohello viktiga leveranstjänst.

### <a name="manifest-files"></a>Manifest-filer
hello-klienten måste tooextract hello URL (som också innehåller innehållsnyckeln Id (kid)) värdet från hello manifestfilen. hello klienten försök sedan tooget hello krypteringsnyckeln från hello viktiga leverans av tjänsten. hello klienten måste också tooextract hello IV värdet och Använd den dekryptera hello stream.hello följande fragment visar hello <Protection> element hello Smooth Streaming manifestet.

    <Protection>
      <ProtectionHeader SystemID="B47B251A-2409-4B42-958E-08DBAE7B4EE9">
        <ContentProtection xmlns:sea="urn:mpeg:dash:schema:sea:2012" schemeIdUri="urn:mpeg:dash:sea:2012">
          <sea:SegmentEncryption schemeIdUri="urn:mpeg:dash:sea:aes128-cbc:2013"/>
          <sea:KeySystem keySystemUri="urn:mpeg:dash:sea:keysys:http:2013"/>
          <sea:CryptoPeriod IV="0xD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7" 
                            keyUriTemplate="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
                                            kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d"/>
        </ContentProtection>
      </ProtectionHeader>
    </Protection>

Hello gäller HLS, är hello rot manifestet uppdelad i segment filer. 

Hello rot manifestet är till exempel: http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/manifest(format=m3u8-aapl) den innehåller en lista över segment filnamn.

    . . . 
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=630133,RESOLUTION=424x240,CODECS="avc1.4d4015,mp4a.40.2",AUDIO="audio"
    QualityLevels(514369)/Manifest(video,format=m3u8-aapl)
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=965441,RESOLUTION=636x356,CODECS="avc1.4d401e,mp4a.40.2",AUDIO="audio"
    QualityLevels(842459)/Manifest(video,format=m3u8-aapl)
    …

Om du öppnar en hello segment fil i redigerare (till exempel http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/QualityLevels(514369)/Manifest(video,format=m3u8-aapl), it should innehåller #EXT X-nyckel som anger att hello-filen är krypterad.

    #EXTM3U
    #EXT-X-VERSION:4
    #EXT-X-ALLOW-CACHE:NO
    #EXT-X-MEDIA-SEQUENCE:0
    #EXT-X-TARGETDURATION:9
    #EXT-X-KEY:METHOD=AES-128,
    URI="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
         kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d",
            IV=0XD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7
    #EXT-X-PROGRAM-DATE-TIME:1970-01-01T00:00:00.000+00:00
    #EXTINF:8.425708,no-desc
    Fragments(video=0,format=m3u8-aapl)
    #EXT-X-ENDLIST

>[!NOTE] 
>Om du planerar tooplay en AES krypterade HLS i Safari, se [bloggen](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).

### <a name="request-hello-key-from-hello-key-delivery-service"></a>Begära hello nyckeln från hello viktiga delivery service

hello följande kod visar hur toosend begäran-toohello Media Services nyckeln leverans av tjänsten med en nyckel leverans Uri (som har extraherats från hello manifestet) och en token (det här avsnittet inte dig om hur tooget Simple Web Tokens från en Secure Token Service).

    private byte[] GetDeliveryKey(Uri keyDeliveryUri, string token)
    {
        HttpWebRequest request = (HttpWebRequest)WebRequest.Create(keyDeliveryUri);

        request.Method = "POST";
        request.ContentType = "text/xml";
        if (!string.IsNullOrEmpty(token))
        {
            request.Headers[AuthorizationHeader] = token;
        }
        request.ContentLength = 0;

        var response = request.GetResponse();

        var stream = response.GetResponseStream();
        if (stream == null)
        {
            throw new NullReferenceException("Response stream is null");
        }

        var buffer = new byte[256];
        var length = 0;
        while (stream.CanRead && length <= buffer.Length)
        {
            var nexByte = stream.ReadByte();
            if (nexByte == -1)
            {
                break;
            }
            buffer[length] = (byte)nexByte;
            length++;
        }
        response.Close();

        // AES keys must be exactly 16 bytes (128 bits).
        var key = new byte[length];
        Array.Copy(buffer, key, length);
        return key;
    }

## <a name="protect-your-content-with-aes-128-using-net"></a>Skydda ditt innehåll med AES-128 med hjälp av .NET

### <a name="create-and-configure-a-visual-studio-project"></a>Skapa och konfigurera ett Visual Studio-projekt

1. Konfigurera utvecklingsmiljön och fylla hello app.config-fil med anslutningsinformation, enligt beskrivningen i [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md). 
2. Lägg till följande element för hello**appSettings** definieras i filen app.config:

        <add key="Issuer" value="http://testacs.com"/>
        <add key="Audience" value="urn:test"/>

### <a id="example"></a>Exempel

Skriv över hello koden i Program.cs-filen med hello koden som visas i det här avsnittet.
 
>[!NOTE]
>Det finns en gräns på 1 000 000 principer för olika AMS-principer (till exempel för positionerarprincipen eller ContentKeyAuthorizationPolicy). Du bör använda hello samma princip-ID om du alltid använder hello samma dagar / åtkomstbehörigheter, till exempel principer för lokaliserare som är avsedda tooremain på plats för lång tid (icke-överföringen principer). Mer information finns i [detta](media-services-dotnet-manage-entities.md#limit-access-policies) avsnitt.

Se till att tooupdate variabler toopoint toofolders där dina indatafiler finns.

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Security.Cryptography;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;
    using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;

    namespace AESDynamicEncryptionAndKeyDeliverySvc
    {
        class Program
        {
        // Read values from hello App.config file.
        private static readonly string _AADTenantDomain =
        ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
        ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

        // A Uri describing hello issuer of hello token.  
        // Must match hello value in hello token for hello token toobe considered valid.
        private static readonly Uri _sampleIssuer =
            new Uri(ConfigurationManager.AppSettings["Issuer"]);
        // hello Audience or Scope of hello token.  
        // Must match hello value in hello token for hello token toobe considered valid.
        private static readonly Uri _sampleAudience =
            new Uri(ConfigurationManager.AppSettings["Audience"]);

        // Field for service context.
        private static CloudMediaContext _context = null;

        private static readonly string _mediaFiles =
            Path.GetFullPath(@"../..\Media");

        private static readonly string _singleMP4File =
            Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");

        static void Main(string[] args)
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            bool tokenRestriction = false;
            string tokenTemplateString = null;

            IAsset asset = UploadFileAndCreateAsset(_singleMP4File);
            Console.WriteLine("Uploaded asset: {0}", asset.Id);

            IAsset encodedAsset = EncodeToAdaptiveBitrateMP4Set(asset);
            Console.WriteLine("Encoded asset: {0}", encodedAsset.Id);

            IContentKey key = CreateEnvelopeTypeContentKey(encodedAsset);
            Console.WriteLine("Created key {0} for hello asset {1} ", key.Id, encodedAsset.Id);
            Console.WriteLine();

            if (tokenRestriction)
            tokenTemplateString = AddTokenRestrictedAuthorizationPolicy(key);
            else
            AddOpenAuthorizationPolicy(key);

            Console.WriteLine("Added authorization policy: {0}", key.AuthorizationPolicyId);
            Console.WriteLine();

            CreateAssetDeliveryPolicy(encodedAsset, key);
            Console.WriteLine("Created asset delivery policy. \n");
            Console.WriteLine();

            if (tokenRestriction && !String.IsNullOrEmpty(tokenTemplateString))
            {
            // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
            // back into a TokenRestrictionTemplate class instance.
            TokenRestrictionTemplate tokenTemplate =
                TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

            // Generate a test token based on hello data in hello given TokenRestrictionTemplate.
            // Note, you need toopass hello key id Guid because we specified 
            // TokenClaim.ContentKeyIdentifierClaim in during hello creation of TokenRestrictionTemplate.
            Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);

            //hello GenerateTestToken method returns hello token without hello word “Bearer” in front
            //so you have tooadd it in front of hello token string. 
            string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
            Console.WriteLine("hello authorization token is:\nBearer {0}", testToken);
            Console.WriteLine();
            }

            // You can use hello bit.ly/aesplayer Flash player tootest hello URL 
            // (with open authorization policy). 
            // Paste hello URL and click hello Update button tooplay hello video. 
            //
            string URL = GetStreamingOriginLocator(encodedAsset);
            Console.WriteLine("Smooth Streaming Url: {0}/manifest", URL);
            Console.WriteLine();
            Console.WriteLine("HLS Url: {0}/manifest(format=m3u8-aapl)", URL);
            Console.WriteLine();

            Console.ReadLine();
        }

        static public IAsset UploadFileAndCreateAsset(string singleFilePath)
        {
            if (!File.Exists(singleFilePath))
            {
            Console.WriteLine("File does not exist.");
            return null;
            }

            var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
            IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.StorageEncrypted);

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Created assetFile {0}", assetFile.Name);


            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            return inputAsset;
        }

        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("Media Encoder Standard Job");
            // Get a media processor reference, and pass tooit hello name of hello 
            // processor toouse for hello specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

            // Create a task with hello encoding details, using a string preset.
            // In this case "Adaptive Streaming" preset is used.
            ITask task = job.Tasks.AddNew("My encoding task",
            processor,
            "Adaptive Streaming",
            TaskOptions.None);

            // Specify hello input asset toobe encoded.
            task.InputAssets.Add(asset);
            // Add an output asset toocontain hello results of hello job. 
            // This output is specified as AssetCreationOptions.None, which 
            // means hello output asset is not encrypted. 
            task.OutputAssets.AddNew("Output asset",
            AssetCreationOptions.StorageEncrypted);

            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
            job.Submit();
            job.GetExecutionProgressTask(CancellationToken.None).Wait();

            return job.OutputMediaAssets[0];
        }

        private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
        {
            var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
            ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

            if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

            return processor;
        }

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

            // Associate hello key with hello asset.
            asset.ContentKeys.Add(key);

            return key;
        }

        static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
        {
            // Create ContentKeyAuthorizationPolicy with Open restrictions 
            // and create authorization policy             
            IContentKeyAuthorizationPolicy policy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Open Authorization Policy").Result;

            List<ContentKeyAuthorizationPolicyRestriction> restrictions =
            new List<ContentKeyAuthorizationPolicyRestriction>();

            ContentKeyAuthorizationPolicyRestriction restriction =
            new ContentKeyAuthorizationPolicyRestriction
            {
            Name = "HLS Open Authorization Policy",
            KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
            Requirements = null // no requirements needed for HLS
        };

            restrictions.Add(restriction);

            IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
            "policy",
            ContentKeyDeliveryType.BaselineHttp,
            restrictions,
            "");

            policy.Options.Add(policyOption);

            // Add ContentKeyAutorizationPolicy tooContentKey
            contentKey.AuthorizationPolicyId = policy.Id;
            IContentKey updatedKey = contentKey.UpdateAsync().Result;
            Console.WriteLine("Adding Key tooAsset: Key ID is " + updatedKey.Id);
        }

        public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
        {
            string tokenTemplateString = GenerateTokenRequirements();

            IContentKeyAuthorizationPolicy policy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("HLS token restricted authorization policy").Result;

            List<ContentKeyAuthorizationPolicyRestriction> restrictions =
            new List<ContentKeyAuthorizationPolicyRestriction>();

            ContentKeyAuthorizationPolicyRestriction restriction =
            new ContentKeyAuthorizationPolicyRestriction
            {
                Name = "Token Authorization Policy",
                KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                Requirements = tokenTemplateString
            };

            restrictions.Add(restriction);

            //You could have multiple options 
            IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
            "Token option for HLS",
            ContentKeyDeliveryType.BaselineHttp,
            restrictions,
            null  // no key delivery data is needed for HLS
            );

            policy.Options.Add(policyOption);

            // Add ContentKeyAutorizationPolicy tooContentKey
            contentKey.AuthorizationPolicyId = policy.Id;
            IContentKey updatedKey = contentKey.UpdateAsync().Result;
            Console.WriteLine("Adding Key tooAsset: Key ID is " + updatedKey.Id);

            return tokenTemplateString;
        }

        static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
        {
            Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);

            string envelopeEncryptionIV = Convert.ToBase64String(GetRandomBuffer(16));

            // When configuring delivery policy, you can choose tooassociate it
            // with a key acquisition URL that has a KID appended or
            // or a key acquisition URL that does not have a KID appended  
            // in which case a content key can be reused. 

            // EnvelopeKeyAcquisitionUrl:  contains a key ID in hello key URL.
            // EnvelopeBaseKeyAcquisitionUrl:  hello URL does not contains a key ID

            // hello following policy configuration specifies: 
            // key url that will have KID=<Guid> appended toohello envelope and
            // hello Initialization Vector (IV) toouse for hello envelope encryption.

            Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
            {
            {AssetDeliveryPolicyConfigurationKey.EnvelopeKeyAcquisitionUrl, keyAcquisitionUri.ToString()}
            };

            IAssetDeliveryPolicy assetDeliveryPolicy =
            _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
                AssetDeliveryPolicyType.DynamicEnvelopeEncryption,
                AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.Dash,
                assetDeliveryPolicyConfiguration);

            // Add AssetDelivery Policy toohello asset
            asset.DeliveryPolicies.Add(assetDeliveryPolicy);
            Console.WriteLine();
            Console.WriteLine("Adding Asset Delivery Policy: " +
            assetDeliveryPolicy.AssetDeliveryPolicyType);
        }

        static public string GetStreamingOriginLocator(IAsset asset)
        {

            // Get a reference toohello streaming manifest file from hello  
            // collection of files in hello asset. 

            var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                EndsWith(".ism")).
                FirstOrDefault();

            // Create a 30-day readonly access policy. 
            // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.            
            IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);

            // Create a locator toohello streaming content on an origin. 
            ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));

            // Create a URL toohello manifest file. 
            return originLocator.Path + assetFile.Name;
        }

        static private string GenerateTokenRequirements()
        {
            TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);

            template.PrimaryVerificationKey = new SymmetricVerificationKey();
            template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();

            template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);

            return TokenRestrictionTemplateSerializer.Serialize(template);
        }

        static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
            ((IJob)sender).Name,
            e.CurrentState,
            DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
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
        }
    }


## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

