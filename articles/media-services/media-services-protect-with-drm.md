---
title: aaaUsing PlayReady och/eller Widevine dynamic common kryptering | Microsoft Docs
description: "Microsoft Azure Media Services kan du toodeliver MPEG DASH, Smooth Streaming och Http Live Streaming (HLS) dataströmmar som skyddas med Microsoft PlayReady DRM. Du kan också toodelivery DASH krypterat med Widevine DRM. Det här avsnittet visar hur toodynamically kryptera med PlayReady och Widevine DRM."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 548d1a12-e2cb-45fe-9307-4ec0320567a2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: juliako
ms.openlocfilehash: 0475e6ec80dcf39eb4e5c4ad4d17f821502951bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-playready-andor-widevine-dynamic-common-encryption"></a>Använda PlayReady och/eller Widevine Dynamic Common Encryption

> [!div class="op_single_selector"]
> * [NET](media-services-protect-with-drm.md)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
>
>

Microsoft Azure Media Services kan du toodeliver MPEG DASH, Smooth Streaming och HTTP Live Streaming (HLS)-dataströmmar som skyddas med [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/). Du kan också toodeliver krypterade DASH-dataströmmar med Widevine DRM-licenser. Både PlayReady och Widevine krypteras enligt hello Common Encryption (ISO/IEC 23001 7 CENC)-specifikationen. Du kan använda [AMS .NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (startar med hello version 3.5.1) eller REST API tooconfigure din AssetDeliveryConfiguration toouse Widevine.

Media Services är en tjänst för att leverera PlayReady- och Widevine DRM-licenser. Media Services tillhandahåller också API: er som gör att du kan konfigurera hello rättigheter och begränsningar som du vill använda för hello PlayReady eller Widevine DRM runtime tooenforce när en användare spelar upp skyddat innehåll. När en användare begär ett DRM-skyddat innehåll, spelarappen hello en licens från hello AMS-Licenstjänsten. hello AMS-Licenstjänsten utfärdar en licens toohello spelare om den är auktoriserad. En PlayReady eller Widevine-licens innehåller hello dekrypteringsnyckel som kan användas av hello klienten player toodecrypt och dataströmmen hello-innehåll.

Du kan också använda följande AMS-partner toohelp du leverera Widevine-licenser hello: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/). Mer information finns i Integrering med [Axinom](media-services-axinom-integration.md) och [castLabs](media-services-castlabs-integration.md).

Media Services stöder flera olika sätt att auktorisera användare som begär nycklar. Hej innehållsnyckelns auktoriseringsprincip kan ha en eller flera auktoriseringsbegränsningar: öppen eller tokenbegränsning. Hej tokenbegränsade principen måste åtföljas av en token som utfärdas av en säker säkerhetstokentjänst (STS). Media Services stöder token i hello [Simple Web Tokens](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) format och [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT)-format. Mer information finns i Konfigurera hello innehållsnyckelns auktoriseringsprincip.

tootake nytta av dynamisk kryptering behöver du toohave en tillgång som innehåller en uppsättning med flera bithastigheter MP4-filer eller Smooth Streaming källfiler i multibithastighet. Du måste också tooconfigure hello leveransprinciperna för hello tillgången (beskrivs senare i det här avsnittet). Sedan, baserat på hello-format som specificerats i hello strömnings-URL, servern för strömning på begäran av hello säkerställer hello dataströmmen levereras i hello-protokollet som du har valt. Därför behöver du bara toostore och betala för hello filer i ett enda lagringsformat och Media Services skapar och ger hello lämplig HTTP-respons baserat på varje begäran från en klient.

Det här avsnittet är användbara toodevelopers som arbetar på appar som levererar media som skyddas av flera DRM: er, som PlayReady och Widevine. hello avsnittet visar hur tooconfigure hello Playreadys licensleveranstjänst med auktoriseringsprinciper så att endast auktoriserade klienter kan få PlayReady eller Widevine-licenser. Den visar även hur toouse dynamiska kryptering med PlayReady eller Widevine DRM över DASH.

>[!NOTE]
>När AMS-kontot skapas en **standard** strömningsslutpunkt har lagts till tooyour konto i hello **stoppad** tillstånd. toostart strömning ditt innehåll och dra nytta av dynamisk paketering och dynamisk kryptering hello strömningsslutpunkt som du vill toostream innehåll har toobe i hello **kör** tillstånd. 

## <a name="download-sample"></a>Hämta exempel
Du kan hämta hello-exempel som beskrivs i den här artikeln från [här](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm).

## <a name="configuring-dynamic-common-encryption-and-drm-license-delivery-services"></a>Konfigurera Dynamic Common Encryption och DRM-licensleveranstjänster 

hello följande är allmänna steg som behöver du tooperform när du skyddar dina tillgångar med PlayReady med hello Media Services licensleveranstjänst och även använder dynamisk kryptering.

1. Skapa en tillgång och överföra filer till hello tillgång.
2. Koda hello tillgång som innehåller hello filen toohello anpassningsbar bithastighet MP4-uppsättningen.
3. Skapa en innehållsnyckel och associera den med hello kodade tillgången. I Media Services innehåller innehållsnyckeln hello hello tillgångens krypteringsnyckel.
4. Konfigurera hello innehållsnyckelns auktoriseringsprincip. Hej innehållsnyckelns auktoriseringsprincip måste konfigureras av dig och uppfyllas av hello klienten för hello innehåll viktiga toobe levererat toohello klienten.

    När du skapar hello innehållsnyckelns auktoriseringsprincip måste toospecify hello följande: leverans metod (PlayReady eller Widevine), begränsningar (öppen eller token) och information specifik toohello nyckelleveranstypen och som definierar hur hello nyckeln levereras toohello klienten ([PlayReady](media-services-playready-license-template-overview.md) eller [Widevine](media-services-widevine-license-template-overview.md) licensmall).

5. Konfigurera hello leveransprincipen för en tillgång. hello konfigurationen för leveransprincipen omfattar: leveransprotokoll (till exempel MPEG DASH, HLS, Smooth Streaming eller alla), hello typen av dynamisk kryptering (till exempel Common Encryption) och PlayReady- eller URL för anskaffning av Widevine-licens.

    Du kan använda olika principer tooeach protokoll på hello samma tillgång. Du kan till exempel tillämpa PlayReady-kryptering tooSmooth/DASH och AES Envelope tooHLS. Alla protokoll som inte har definierats i en leveransprincip (exempelvis du lägga till en enda princip som endast anger HLS som hello protokoll) kommer att blockeras från strömning. hello undantag toothis är om du har någon tillgångsleveransprincip alls definierats. Sedan tillåts alla protokoll i hello Rensa.

6. Skapa en OnDemand-positionerare i ordning tooget en strömnings-URL.

Du hittar ett komplett .NET-exempel hello slutet av hello-avsnittet.

hello följande bild visar hello arbetsflödet som beskrivs ovan. Här används hello token för autentisering.

![Skydda med PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-drm.png)

hello resten av det här avsnittet innehåller detaljerade förklaringar, kodexempel och länkar tootopics som visar hur tooachieve hello uppgifter som beskrivs ovan.

## <a name="current-limitations"></a>Aktuella begränsningar
Om du lägger till eller uppdaterar en tillgångsleveransprincip måste du ta bort hello associerade lokaliserare (om sådan finns) och skapa en ny.

Begränsning vid kryptering med Widevine med Azure Media Services: för närvarande stöds inte nycklar för multiinnehåll.

## <a name="create-an-asset-and-upload-files-into-hello-asset"></a>Skapa en tillgång och överföra filer till hello tillgång
I ordning toomanage, koda och strömma videor, måste du först överföra innehållet till Microsoft Azure Media Services. När du har överfört är ditt innehåll lagras på ett säkert sätt i hello molnet för vidare bearbetning och strömning.

Utförlig information finns i [Överföra filer till ett Media Services-konto](media-services-dotnet-upload-files.md).

## <a name="encode-hello-asset-containing-hello-file-toohello-adaptive-bitrate-mp4-set"></a>Koda hello tillgång som innehåller hello filen toohello anpassningsbar bithastighet MP4-uppsättningen
Med dynamisk kryptering är allt du behöver toocreate en tillgång som innehåller en uppsättning med flera bithastigheter MP4-filer eller Smooth Streaming källfiler i multibithastighet. Sedan, baserat på hello format som anges i manifestet hello och Fragmentera begäran, hello strömning på begäran kommer servern att säkerställa att du har fått hello dataström i hello-protokollet som du har valt. Därför behöver du bara toostore och betala för hello filer i ett enda lagringsformat och Media Services-tjänsten skapar och ger hello lämplig respons baserat på begäranden från en klient. Mer information finns i hello [översikt över dynamisk paketering](media-services-dynamic-packaging-overview.md) avsnittet.

Mer information om hur tooencode, se [hur tooencode en tillgång med Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md).

## <a id="create_contentkey"></a>Skapa en innehållsnyckel och associera den med hello kodade tillgången
I Media Services innehåller innehållsnyckeln för hello hello-nyckel som du vill tooencrypt en tillgång med.

Utförlig information finns i [Skapa en innehållsnyckel](media-services-dotnet-create-contentkey.md).

## <a id="configure_key_auth_policy"></a>Konfigurera hello innehållsnyckelns auktoriseringsprincip
Media Services stöder flera olika sätt att auktorisera användare som begär nycklar. Hej innehållsnyckelns auktoriseringsprincip måste konfigureras av dig och uppfyllas av hello klienten (spelaren) för hello viktiga toobe levereras toohello klienten. Hej innehållsnyckelns auktoriseringsprincip kan ha en eller flera auktoriseringsbegränsningar: öppen eller tokenbegränsning.

Mer information finns i [Konfigurera  innehållsnyckelns auktoriseringsprincip](media-services-dotnet-configure-content-key-auth-policy.md#playready-dynamic-encryption).

## <a id="configure_asset_delivery_policy"></a>Konfigurera tillgångsleveransprincip
Konfigurera hello leveransprincipen för din tillgång. Vissa saker som hello tillgången konfigurationen för leveransprincipen omfattar:

* hello DRM-licens förvärv URL.
* hello protokollet för tillgångsleverans (till exempel MPEG DASH, HLS, Smooth Streaming eller alla).
* hello typen av dynamisk kryptering (i det här fallet Common Encryption).

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

## <a name="create-and-configure-a-visual-studio-project"></a>Skapa och konfigurera ett Visual Studio-projekt

1. Konfigurera utvecklingsmiljön och fylla hello app.config-fil med anslutningsinformation, enligt beskrivningen i [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md). 
2. Lägg till följande element för hello**appSettings** definieras i filen app.config:

        <add key="Issuer" value="http://testacs.com"/>
        <add key="Audience" value="urn:test"/>

## <a name="example"></a>Exempel

hello följande exempel visas de funktioner som infördes i Azure Media Services SDK för .net-Version 3.5.2 (särskilt hello möjlighet toodefine en Widevine licens mall och begära en Widevine-licens från Azure Media Services).

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
    using Microsoft.WindowsAzure.MediaServices.Client.Widevine;
    using Newtonsoft.Json;

    namespace DynamicEncryptionWithDRM
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

            IContentKey key = CreateCommonTypeContentKey(encodedAsset);
            Console.WriteLine("Created key {0} for hello asset {1} ", key.Id, encodedAsset.Id);
            Console.WriteLine("PlayReady License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense));
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

            // You can use hello http://amsplayer.azurewebsites.net/azuremediaplayer.html player tootest streams.
            // Note that DASH works on IE 11 (via PlayReady), Edge (via PlayReady), Chrome (via Widevine).

            string url = GetStreamingOriginLocator(encodedAsset);
            Console.WriteLine("Encrypted DASH URL: {0}/manifest(format=mpd-time-csf)", url);

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

            IJob job = _context.Jobs.Create(String.Format("Encoding into Mp4 {0} too{1}",
                        inputAsset.Name,
                        encodingPreset));

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


        static public IContentKey CreateCommonTypeContentKey(IAsset asset)
        {

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

            // Configure PlayReady and Widevine license templates.
            string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();

            string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();

            IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
                ContentKeyDeliveryType.PlayReadyLicense,
                restrictions, PlayReadyLicenseTemplate);

            IContentKeyAuthorizationPolicyOption WidevinePolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
                ContentKeyDeliveryType.Widevine,
                restrictions, WidevineLicenseTemplate);

            IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Deliver Common Content Key with no restrictions").
                Result;


            contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
            contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);
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

            // Configure PlayReady and Widevine license templates.
            string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();

            string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();

            IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                ContentKeyDeliveryType.PlayReadyLicense,
                restrictions, PlayReadyLicenseTemplate);

            IContentKeyAuthorizationPolicyOption WidevinePolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                ContentKeyDeliveryType.Widevine,
                restrictions, WidevineLicenseTemplate);

            IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Deliver Common Content Key with token restrictions").
                Result;

            contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
            contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);

            // Associate hello content key authorization policy with hello content key
            contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
            contentKey = contentKey.UpdateAsync().Result;

            return tokenTemplateString;
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

        static private string ConfigurePlayReadyLicenseTemplate()
        {
            // hello following code configures PlayReady License Template using .NET classes
            // and returns hello XML string.

            //hello PlayReadyLicenseResponseTemplate class represents hello template for hello response sent back toohello end user.
            //It contains a field for a custom data string between hello license server and hello application
            //(may be useful for custom app logic) as well as a list of one or more license templates.
            PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();

            // hello PlayReadyLicenseTemplate class represents a license template for creating PlayReady licenses
            // toobe returned toohello end users.
            //It contains hello data on hello content key in hello license and any rights or restrictions toobe
            //enforced by hello PlayReady DRM runtime when using hello content key.
            PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
            //Configure whether hello license is persistent (saved in persistent storage on hello client)
            //or non-persistent (only held in memory while hello player is using hello license).  
            licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;

            // AllowTestDevices controls whether test devices can use hello license or not.  
            // If true, hello MinimumSecurityLevel property of hello license
            // is set too150.  If false (hello default), hello MinimumSecurityLevel property of hello license is set too2000.
            licenseTemplate.AllowTestDevices = true;

            // You can also configure hello Play Right in hello PlayReady license by using hello PlayReadyPlayRight class.
            // It grants hello user hello ability tooplayback hello content subject toohello zero or more restrictions
            // configured in hello license and on hello PlayRight itself (for playback specific policy).
            // Much of hello policy on hello PlayRight has toodo with output restrictions
            // which control hello types of outputs that hello content can be played over and
            // any restrictions that must be put in place when using a given output.
            // For example, if hello DigitalVideoOnlyContentRestriction is enabled,
            //then hello DRM runtime will only allow hello video toobe displayed over digital outputs
            //(analog video outputs won’t be allowed toopass hello content).

            //IMPORTANT: These types of restrictions can be very powerful but can also affect hello consumer experience.
            // If hello output protections are configured too restrictive,
            // hello content might be unplayable on some clients. For more information, see hello PlayReady Compliance Rules document.

            // For example:
            //licenseTemplate.PlayRight.AgcAndColorStripeRestriction = new AgcAndColorStripeRestriction(1);

            responseTemplate.LicenseTemplates.Add(licenseTemplate);

            return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
        }

        private static string ConfigureWidevineLicenseTemplate()
        {
            var template = new WidevineMessage
            {
            allowed_track_types = AllowedTrackTypes.SD_HD,
            content_key_specs = new[]
            {
                    new ContentKeySpecs
                    {
                    required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                    security_level = 1,
                    track_type = "SD"
                    }
                },
            policy_overrides = new
            {
                can_play = true,
                can_persist = true,
                can_renew = false
            }
            };

            string configuration = JsonConvert.SerializeObject(template);
            return configuration;
        }

        static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
        {
            // Get hello PlayReady license service URL.
            Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);

            // GetKeyDeliveryUrl for Widevine attaches hello KID toohello URL.
            // For example: https://amsaccount1.keydelivery.mediaservices.windows.net/Widevine/?KID=268a6dcb-18c8-4648-8c95-f46429e4927c.  
            // hello WidevineBaseLicenseAcquisitionUrl (used below) also tells Dynamaic Encryption
            // tooappend /? KID =< keyId > toohello end of hello url when creating hello manifest.
            // As a result Widevine license acquisition URL will have KID appended twice,
            // so we need tooremove hello KID that in hello URL when we call GetKeyDeliveryUrl.

            Uri widevineUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.Widevine);
            UriBuilder uriBuilder = new UriBuilder(widevineUrl);
            uriBuilder.Query = String.Empty;
            widevineUrl = uriBuilder.Uri;

            Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
            {
                    {AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},
                    {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl, widevineUrl.ToString()}

            };

            // In this case we only specify Dash streaming protocol in hello delivery policy,
            // All other protocols will be blocked from streaming.
            var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.Dash,
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


## <a name="next-step"></a>Nästa steg
Granska sökvägarna för Media Services-utbildning.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Se även
[CENC med Multi-DRM och Access Control](media-services-cenc-with-multidrm-access-control.md)

[Konfigurera Widevine-paketering med AMS](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services)

[Vi ber att få presentera Google Widevines licensleveranstjänster i Azure Media Services](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/)
