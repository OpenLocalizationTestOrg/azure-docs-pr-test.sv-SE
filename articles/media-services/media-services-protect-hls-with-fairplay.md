---
title: "aaaProtect HLS innehåll med Microsoft PlayReady- eller Apple FairPlay - Azure | Microsoft Docs"
description: "Det här avsnittet ger en översikt och visar hur toouse Azure Media Services toodynamically kryptera din HTTP Live Streaming (HLS) innehåll med Apple FairPlay. Den visar även hur toouse hello Media Services licens leverans av tjänsten toodeliver FairPlay-licenser tooclients."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 7c3b35d9-1269-4c83-8c91-490ae65b0817
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: juliako
ms.openlocfilehash: 91ca451e3e7bf0da1d74dac4c99180f08f39e4ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="protect-your-hls-content-with-apple-fairplay-or-microsoft-playready"></a>Skydda ditt innehåll med Apple FairPlay eller Microsoft PlayReady HLS
Azure Media Services aktiverar du toodynamically kryptera innehållet HTTP Live Streaming (HLS) med hjälp av hello följande format:  

* **AES-128 kuvert klartextnyckeln**

    hello hela segmentet krypteras med hjälp av hello **AES-128 CBC** läge. hello dekryptering av hello dataströmmen stöds internt av iOS och OS X spelare. Mer information finns i [dynamisk med hjälp av AES-128-kryptering och viktiga leverans tjänsten](media-services-protect-with-aes128.md).
* **Apple FairPlay**

    Hej enskilda video och ljud exempel krypteras med hjälp av hello **AES-128 CBC** läge. **FairPlay strömning** (FPS) är integrerat i hello enhetens operativsystem med inbyggt stöd på iOS- och Apple TV. Safari för OS X kan FPS med hello krypterade Media tillägg (EME) Gränssnittssupport.
* **Microsoft PlayReady**

hello följande bild visar hello **HLS + FairPlay eller PlayReady dynamisk kryptering** arbetsflöde.

![Diagram över arbetsflödet för dynamisk kryptering](./media/media-services-content-protection-overview/media-services-content-protection-with-fairplay.png)

Det här avsnittet visar hur toouse Media Services toodynamically kryptera din HLS innehåll med Apple FairPlay. Den visar även hur toouse hello Media Services licens leverans av tjänsten toodeliver FairPlay-licenser tooclients.

> [!NOTE]
> Om du även vill tooencrypt din HLS innehåll med PlayReady, du behöver toocreate en gemensam innehållsnyckel och associera den med din tillgång. Du måste också tooconfigure hello innehållsnyckelns auktoriseringsprincip, enligt beskrivningen i [med PlayReady-kryptering för dynamiska vanliga](media-services-protect-with-drm.md).
>
>

## <a name="requirements-and-considerations"></a>Krav och överväganden

hello följande krävs när du använder Media Services toodeliver HLS som krypterats med FairPlay och toodeliver FairPlay-licenser:

  * Ett Azure-konto. Mer information finns i avsnittet om [den kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
  * Ett Media Services-konto. toocreate, se [skapa ett Azure Media Services-konto med hello Azure-portalen](media-services-portal-create-account.md).
  * Logga med [Apple Development Program](https://developer.apple.com/).
  * Apple kräver hello innehållets ägare tooobtain hello [distributionspaketet](https://developer.apple.com/contact/fps/). Tillstånd redan implementerats nyckeln Security Module (KSM) med Media Services och att du begär hello slutliga FPS paketet. Det finns instruktioner i hello slutliga FPS paketet toogenerate certifiering och få hello programmet hemlig nyckel (be). Du kan använda be tooconfigure FairPlay.
  * Azure Media Services .NET SDK-versionen **3.6.0** eller senare.

följande saker hello måste anges på Media Services viktiga leverans sida:

  * **Appen Cert (nät)**: Detta är en .pfx-fil som innehåller hello privat nyckel. Du skapar den här filen och kryptera den med ett lösenord.

       När du konfigurerar en nyckel leveransprincip måste du ange att lösenordet och hello .pfx-fil i Base64-format.

      hello följande steg beskriver hur toogenerate certifikat PFX-filen för FairPlay:

    1. Installera OpenSSL från https://slproweb.com/products/Win32OpenSSL.html.

        Gå toohello mapp där hello FairPlay certifikat och andra filer som levereras av Apple.
    2. Kör följande kommando från kommandoraden hello hello. Detta omvandlar hello .cer-fil tooa PEM-filen.

        ”C:\OpenSSL-Win32\bin\openssl.exe” x509-informera der-i fairplay.cer-ut fairplay out.pem
    3. Kör följande kommando från kommandoraden hello hello. Detta omvandlar hello PEM-filen tooa .pfx-filen med hello privat nyckel. hello lösenord för hello PFX-filen blir sedan ombedd av OpenSSL.

        ”C:\OpenSSL-Win32\bin\openssl.exe” pkcs12-exportera - ut fairplay out.pfx-inkey privatekey.pem-i fairplay out.pem - passin file:privatekey-pem-pass.txt
  * **Appen Cert lösenord**: hello lösenord för att skapa hello .pfx-fil.
  * **ID för appen Cert lösenord**: du måste överföra hello lösenord, liknande toohow de överför andra Media Services-nycklar. Använd hello **ContentKeyType.FairPlayPfxPassword** enum-värde tooget hello Media Services-ID. Det här är de behöver toouse inuti hello viktiga princip leveransalternativ.
  * **IV**: Detta är ett slumpmässigt värde av 16 byte. Den måste matcha hello iv i hello tillgångsleveransprincip. Du genererar hello iv och placera den på båda platserna: hello tillgångsleveransprincip och hello viktiga princip leveransalternativ.
  * **Be**: den här nyckeln tas emot när du skapar hello certifikatutfärdare med hjälp av hello Apple Developer-portalen. Varje Utvecklingsteamet får en unik be. Spara en kopia av hello be och lagra den på en säker plats. Du behöver tooconfigure be som FairPlayAsk tooMedia tjänster senare.
  * **Be ID**: Detta ID hämtas när du överför be till Media Services. Du måste överföra be med hjälp av hello **ContentKeyType.FairPlayAsk** enum-värde. Hello Media Services ID returneras som hello resultat, och det här är vad ska användas när hello viktiga princip leveransalternativ.

hello måste följande anges av hello FPS på klientsidan:

  * **Appen Cert (nät)**: Detta är en.cer/.der-fil som innehåller hello offentliga nyckel, vilket hello operativsystem använder tooencrypt vissa nyttolast. Media Services måste tooknow om den eftersom det krävs av hello player. hello viktiga leverans tjänsten dekrypterar den med hjälp av hello motsvarande privata nyckel.

tooplay tillbaka en krypterad dataström FairPlay, skaffa en verklig be först och sedan generera en verklig certifikat. Den här processen skapar alla tre delar:

  * .der fil
  * .pfx-fil
  * lösenordet för hello .pfx

hello följande klienter har stöd för HLS med **AES-128 CBC** kryptering: Safari på OS X, Apple TV iOS.

## <a name="configure-fairplay-dynamic-encryption-and-license-delivery-services"></a>Konfigurera FairPlay dynamisk kryptering och licensen för leverans av tjänster
hello följande är allmänna steg för att skydda dina tillgångar med FairPlay med hjälp av hello Media Services licensleveranstjänst och även med hjälp av dynamisk kryptering.

1. Skapa en tillgång och överföra filer till hello tillgång.
2. Koda hello tillgång som innehåller hello filen toohello anpassningsbar bithastighet MP4-uppsättningen.
3. Skapa en innehållsnyckel och associera den med hello kodade tillgången.  
4. Konfigurera hello innehållsnyckelns auktoriseringsprincip. Ange hello följande:

   * hello leveransmetod (i det här fallet FairPlay).
   * FairPlay alternativkonfiguration. Mer information om hur tooconfigure FairPlay, se hello **ConfigureFairPlayPolicyOptions()** metod i hello exemplet nedan.

     > [!NOTE]
     > Vanligtvis skulle vill tooconfigure FairPlay princip alternativ bara en gång, eftersom du bara har en uppsättning av en certifikatutfärdare och en fråga.
     >
     >
   * Begränsningar (öppen eller token).
   * Information specifik toohello nyckelleveranstypen och som definierar hur hello nyckeln levereras toohello klienten.
5. Konfigurera principen för tillgångsleverans hello. hello konfigurationen för leveransprincipen omfattar:

   * Hej leveransprotokoll (HLS).
   * hello typen av dynamisk kryptering (vanliga CBC kryptering).
   * URL för anskaffning av hello licens.

     > [!NOTE]
     > Om du vill toodeliver en ström som har krypterats med FairPlay och en annan Digital Rights Management (DRM)-system kan ha tooconfigure separat leveransprinciperna:
     >
     > * En IAssetDeliveryPolicy tooconfigure dynamiska anpassningsbar strömning via HTTP-(bindestreck) med vanliga kryptering (CENC) (PlayReady och Widevine) och Smooth med PlayReady
     > * En annan IAssetDeliveryPolicy tooconfigure FairPlay för HLS
     >
     >
6. Skapa en OnDemand-positionerare tooget en strömnings-URL.

## <a name="use-fairplay-key-delivery-by-player-apps"></a>Använd FairPlay viktiga leverans av player appar
Du kan utveckla player appar med hjälp av hello iOS SDK. toobe kan tooplay FairPlay innehåll, har du tooimplement hello licens exchange-protokollet. Det här protokollet har inte angetts av Apple. Är det upp tooeach app hur toosend viktiga leverans begäranden. hello Media Services FairPlay viktiga delivery service förväntas hello SPC toocome som meddelandet www-form-url kodad post i hello följande format:

    spc=<Base64 encoded SPC>

> [!NOTE]
> Azure Media Player stöder inte FairPlay uppspelning out of box hello. tooget FairPlay uppspelning på MAC OS X, hämta hello exempel player från hello Apple developer-konto.
>
>

## <a name="streaming-urls"></a>Strömmande URL: er
Om din tillgång har krypterats med flera DRM, bör du använda en tagg för kryptering i hello strömnings-URL: (format = 'm3u8 aapl', kryptering = ”xxx”).

det gäller hello följande överväganden:

* Endast noll eller en typ av enhetskryptering kan anges.
* hello krypteringstyp saknar toobe som anges i hello URL Om bara en kryptering har tillämpade toohello tillgången.
* hello krypteringstypen är skiftlägeskänsligt.
* hello följande krypteringstyper kan anges:  
  * **cenc**: Common encryption (PlayReady eller Widevine)
  * **cbcs aapl**: FairPlay
  * **CBC**: AES envelope kryptering

## <a name="create-and-configure-a-visual-studio-project"></a>Skapa och konfigurera ett Visual Studio-projekt

1. Konfigurera utvecklingsmiljön och fylla hello app.config-fil med anslutningsinformation, enligt beskrivningen i [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md). 
2. Lägg till följande element för hello**appSettings** definieras i filen app.config:

        <add key="Issuer" value="http://testacs.com"/>
        <add key="Audience" value="urn:test"/>

## <a name="example"></a>Exempel

hello som följande exempel visar hello möjlighet toouse Media Services toodeliver ditt innehåll som krypterats med FairPlay. Den här funktionen introducerades i hello Azure Media Services SDK för .NET version 3.6.0. 

Skriv över hello koden i Program.cs-filen med hello koden som visas i det här avsnittet.

>[!NOTE]
>Det finns en gräns på 1 000 000 principer för olika AMS-principer (till exempel för positionerarprincipen eller ContentKeyAuthorizationPolicy). Du bör använda hello samma princip-ID om du alltid använder hello samma dagar / åtkomstbehörigheter, till exempel principer för lokaliserare som är avsedda tooremain på plats för lång tid (icke-överföringen principer). Mer information finns i [detta](media-services-dotnet-manage-entities.md#limit-access-policies) avsnitt.

Se till att tooupdate variabler toopoint toofolders där dina indatafiler finns.

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Threading;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
    using Microsoft.WindowsAzure.MediaServices.Client.FairPlay;
    using Newtonsoft.Json;
    using System.Security.Cryptography.X509Certificates;

    namespace DynamicEncryptionWithFairPlay
    {
        class Program
        {
        // Read values from hello App.config file.
        private static readonly string _AADTenantDomain =
        ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
        ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

        private static readonly Uri _sampleIssuer =
            new Uri(ConfigurationManager.AppSettings["Issuer"]);
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

            IContentKey key = CreateCommonCBCTypeContentKey(encodedAsset);
            Console.WriteLine("Created key {0} for hello asset {1} ", key.Id, encodedAsset.Id);
            Console.WriteLine("FairPlay License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay));
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

            // Generate a test token based on hello hello data in hello given TokenRestrictionTemplate.
            // Note, you need toopass hello key id Guid because we specified
            // TokenClaim.ContentKeyIdentifierClaim in during hello creation of TokenRestrictionTemplate.
            Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
            string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey,
                                        DateTime.UtcNow.AddDays(365));
            Console.WriteLine("hello authorization token is:\nBearer {0}", testToken);
            Console.WriteLine();
            }

            string url = GetStreamingOriginLocator(encodedAsset);
            Console.WriteLine("Encrypted HLS URL: {0}/manifest(format=m3u8-aapl)", url);

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
            IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.None);

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Created assetFile {0}", assetFile.Name);

            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            return inputAsset;
        }

        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset inputAsset)
        {
            var encodingPreset = "Adaptive Streaming";

            IJob job = _context.Jobs.Create(String.Format("Encoding {0}", inputAsset.Name));

            var mediaProcessors =
            _context.MediaProcessors.Where(p => p.Name.Contains("Media Encoder Standard")).ToList();

            var latestMediaProcessor =
            mediaProcessors.OrderBy(mp => new Version(mp.Version)).LastOrDefault();

            ITask encodeTask = job.Tasks.AddNew("Encoding", latestMediaProcessor, encodingPreset, TaskOptions.None);
            encodeTask.InputAssets.Add(inputAsset);
            encodeTask.OutputAssets.AddNew(String.Format("{0} as {1}", inputAsset.Name, encodingPreset), AssetCreationOptions.StorageEncrypted);

            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
            job.Submit();
            job.GetExecutionProgressTask(CancellationToken.None).Wait();

            return job.OutputMediaAssets[0];
        }

        static public IContentKey CreateCommonCBCTypeContentKey(IAsset asset)
        {
            // Create HLS SAMPLE AES encryption content key
            Guid keyId = Guid.NewGuid();
            byte[] contentKey = GetRandomBuffer(16);

            IContentKey key = _context.ContentKeys.Create(
                        keyId,
                        contentKey,
                        "ContentKey",
                        ContentKeyType.CommonEncryptionCbcs);

            // Associate hello key with hello asset.
            asset.ContentKeys.Add(key);

            return key;
        }


        static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
        {
            // Create ContentKeyAuthorizationPolicy with Open restrictions
            // and create authorization policy          

            List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                    {
                    new ContentKeyAuthorizationPolicyRestriction
                    {
                        Name = "Open",
                        KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                        Requirements = null
                    }
                    };


            // Configure FairPlay policy option.
            string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();

            IContentKeyAuthorizationPolicyOption FairPlayPolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
            ContentKeyDeliveryType.FairPlay,
            restrictions,
            FairPlayConfiguration);


            IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Deliver Common CBC Content Key with no restrictions").
                Result;

            contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);

            // Associate hello content key authorization policy with hello content key.
            contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
            contentKey = contentKey.UpdateAsync().Result;
        }

        public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
        {
            string tokenTemplateString = GenerateTokenRequirements();

            List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                    {
                    new ContentKeyAuthorizationPolicyRestriction
                    {
                        Name = "Token Authorization Policy",
                        KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                        Requirements = tokenTemplateString,
                    }
                    };

            // Configure FairPlay policy option.
            string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();


            IContentKeyAuthorizationPolicyOption FairPlayPolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                   ContentKeyDeliveryType.FairPlay,
                   restrictions,
                   FairPlayConfiguration);

            IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Deliver Common CBC Content Key with token restrictions").
                Result;

            contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);

            // Associate hello content key authorization policy with hello content key
            contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
            contentKey = contentKey.UpdateAsync().Result;

            return tokenTemplateString;
        }

        private static string ConfigureFairPlayPolicyOptions()
        {
            // For testing you can provide all zeroes for ASK bytes together with hello cert from Apple FPS SDK.
            // However, for production you must use a real ASK from Apple bound tooa real prod certificate.
            byte[] askBytes = Guid.NewGuid().ToByteArray();
            var askId = Guid.NewGuid();
            // Key delivery retrieves askKey by askId and uses this key toogenerate hello response.
            IContentKey askKey = _context.ContentKeys.Create(
                        askId,
                        askBytes,
                        "askKey",
                        ContentKeyType.FairPlayASk);

            //Customer password for creating hello .pfx file.
            string pfxPassword = "<customer password for creating hello .pfx file>";
            // Key delivery retrieves pfxPasswordKey by pfxPasswordId and uses this key toogenerate hello response.
            var pfxPasswordId = Guid.NewGuid();
            byte[] pfxPasswordBytes = System.Text.Encoding.UTF8.GetBytes(pfxPassword);
            IContentKey pfxPasswordKey = _context.ContentKeys.Create(
                        pfxPasswordId,
                        pfxPasswordBytes,
                        "pfxPasswordKey",
                        ContentKeyType.FairPlayPfxPassword);

            // iv - 16 bytes random value, must match hello iv in hello asset delivery policy.
            byte[] iv = Guid.NewGuid().ToByteArray();

            //Specify hello .pfx file created by hello customer.
            var appCert = new X509Certificate2("path toohello .pfx file created by hello customer", pfxPassword, X509KeyStorageFlags.Exportable);

            string FairPlayConfiguration =
            Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(
                appCert,
                pfxPassword,
                pfxPasswordId,
                askId,
                iv);

            return FairPlayConfiguration;
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

        static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
        {
            var kdPolicy = _context.ContentKeyAuthorizationPolicies.Where(p => p.Id == key.AuthorizationPolicyId).Single();

            var kdOption = kdPolicy.Options.Single(o => o.KeyDeliveryType == ContentKeyDeliveryType.FairPlay);

            FairPlayConfiguration configFP = JsonConvert.DeserializeObject<FairPlayConfiguration>(kdOption.KeyDeliveryConfiguration);

            // Get hello FairPlay license service URL.
            Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay);

            // hello reason hello below code replaces "https://" with "skd://" is because
            // in hello IOS player sample code which you obtained in Apple developer account,
            // hello player only recognizes a Key URL that starts with skd://.
            // However, if you are using a customized player,
            // you can choose whatever protocol you want.
            // For example, "https".

            Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
            {
                    {AssetDeliveryPolicyConfigurationKey.FairPlayLicenseAcquisitionUrl, acquisitionUrl.ToString().Replace("https://", "skd://")},
                    {AssetDeliveryPolicyConfigurationKey.CommonEncryptionIVForCbcs, configFP.ContentEncryptionIV}
            };

            var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryptionCbcs,
            AssetDeliveryProtocol.HLS,
            assetDeliveryPolicyConfiguration);

            // Add AssetDelivery Policy toohello asset
            asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        }


        /// <summary>
        /// Gets hello streaming origin locator.
        /// </summary>
        /// <param name="assets"></param>
        /// <returns></returns>
        static public string GetStreamingOriginLocator(IAsset asset)
        {

            // Get a reference toohello streaming manifest file from hello  
            // collection of files in hello asset.

            var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                         EndsWith(".ism")).
                         FirstOrDefault();

            // Create a 30-day readonly access policy.
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

        static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
            ((IJob)sender).Name,
            e.CurrentState,
            DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
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
        }
    }


## <a name="next-steps-media-services-learning-paths"></a>Nästa steg: Utbildningsvägar för Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
