---
title: aaaUse Azure Media Services toodeliver DRM-licenser eller AES-nycklar
description: "Den här artikeln beskriver hur du kan använda Azure Media Services (AMS) toodeliver PlayReady och/eller Widevine-licenser och AES-nycklar men hello rest (kodning, kryptering, strömning) med hjälp av lokala servrar."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 8546c2c1-430b-4254-a88d-4436a83f9192
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: juliako
ms.openlocfilehash: a81da2973c79e5182ae58aeca7a0f14f3fc7c9ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-media-services-toodeliver-drm-licenses-or-aes-keys"></a>Använda Azure Media Services toodeliver DRM-licenser eller AES-nycklar
Azure Media Services (AMS) kan du tooingest, koda, lägga till innehållsskydd och strömma ditt innehåll (se [detta](media-services-protect-with-drm.md) artikeln för information). Det finns dock kunder som bara vill toouse AMS toodeliver licenser och/eller nycklar och göra kodning, kryptering och strömning med lokala servrar. Den här artikeln beskriver hur du kan använda AMS toodeliver PlayReady och/eller Widevine-licenser men hello resten med dina lokala servrar. 

## <a name="overview"></a>Översikt
Media Services tillhandahåller en tjänst för att leverera PlayReady och Widevine DRM-licenser och AES-128 nycklar. Media Services tillhandahåller också API: er som gör att du kan konfigurera hello rättigheter och begränsningar som du vill använda för hello DRM runtime tooenforce när en användare spelar upp hello DRM-skyddat innehåll. När en användare begäranden hello skyddat innehåll, spelarappen hello en licens från hello AMS-Licenstjänsten. hello AMS-Licenstjänsten utfärdar hello licens toohello player (om den är auktoriserad). hello PlayReady och Widevine-licenser innehålla hello dekrypteringsnyckel som kan användas av hello klienten player toodecrypt och dataströmmen hello-innehåll.

Media Services stöder flera olika sätt att auktorisera användare som gör licens eller viktiga förfrågningar. Du konfigurerar hello innehållsnyckelns auktoriseringsprincip och hello princip kan ha en eller flera begränsningar: öppen eller tokenbegränsning. Hej tokenbegränsade principen måste åtföljas av en token som utfärdas av en säker säkerhetstokentjänst (STS). Media Services stöder token i hello Simple Web Tokens (SWT) format och JSON-Webbtoken (JWT)-format.

hello följande diagram visar hello steg du måste tootake toouse AMS toodeliver PlayReady och/eller Widevine-licenser men hello resten med dina lokala servrar.

![Skydda med PlayReady](./media/media-services-deliver-keys-and-licenses/media-services-diagram1.png)

## <a name="download-sample"></a>Hämta exempel
Du kan hämta hello-exempel som beskrivs i den här artikeln från [här](https://github.com/Azure/media-services-dotnet-deliver-drm-licenses).

## <a name="create-and-configure-a-visual-studio-project"></a>Skapa och konfigurera ett Visual Studio-projekt

1. Konfigurera utvecklingsmiljön och fylla hello app.config-fil med anslutningsinformation, enligt beskrivningen i [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md). 
2. Lägg till följande element för hello**appSettings** definieras i filen app.config:

    <add key="Issuer" value="http://testacs.com"/> <add key="Audience" value="urn:test"/>

## <a name="net-code-example"></a>.NET-kodexempel

hello följande kodexempel visar hur toocreate en gemensam innehåll nyckel och få PlayReady eller Widevine-licens förvärv URL: er. Du behöver tooget hello följande typer av information från AMS och konfigurera lokal server: **innehållsnyckeln**, **nyckel-id**, **licens URL för anskaffning**. När du konfigurerar lokala servern kan du strömma från strömmande servern. Eftersom hello krypterad dataström punkter tooAMS licensservern begär spelaren en licens från AMS. Om du väljer tokenautentisering hello AMS licensservern validerar hello-token som du har skickat via HTTPS och (om det är giltigt) levererar hello licens tillbaka tooyour player. (hello kodexemplet visar bara hur toocreate en gemensam innehåll nyckel och få PlayReady eller Widevine-licens förvärv URL: er. Om du vill toodelivery AES-128 nycklar, du behöver toocreate ett envelope innehållsnyckeln och få en nyckel förvärv URL och [detta](media-services-protect-with-aes128.md) artikeln visar hur toodo den).

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
    using Microsoft.WindowsAzure.MediaServices.Client.Widevine;
    using Newtonsoft.Json;

    namespace DeliverDRMLicenses
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

            static void Main(string[] args)
            {
                var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
                var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

                _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

                bool tokenRestriction = true;
                string tokenTemplateString = null;


                IContentKey key = CreateCommonTypeContentKey();

                // Print out hello key ID and Key in base64 string format
                Console.WriteLine("Created key {0} with key value {1} ",
                    key.Id, System.Convert.ToBase64String(key.GetClearKeyValue()));

                Console.WriteLine("PlayReady License Key delivery URL: {0}",
                    key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense));

                Console.WriteLine("Widevine License Key delivery URL: {0}",
                    key.GetKeyDeliveryUrl(ContentKeyDeliveryType.Widevine));

                if (tokenRestriction)
                    tokenTemplateString = AddTokenRestrictedAuthorizationPolicy(key);
                else
                    AddOpenAuthorizationPolicy(key);

                Console.WriteLine("Added authorization policy: {0}",
                    key.AuthorizationPolicyId);
                Console.WriteLine();
                Console.ReadLine();
            }

            static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
            {

                // Create ContentKeyAuthorizationPolicy with Open restrictions 
                // and create authorization policy          

                List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                    new List<ContentKeyAuthorizationPolicyRestriction>
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

                List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                    new List<ContentKeyAuthorizationPolicyRestriction>
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

                //hello PlayReadyLicenseResponseTemplate class represents hello template 
                //for hello response sent back toohello end user. 
                //It contains a field for a custom data string between hello license server 
                //and hello application (may be useful for custom app logic) 
                //as well as a list of one or more license templates.

                PlayReadyLicenseResponseTemplate responseTemplate =
                    new PlayReadyLicenseResponseTemplate();

                // hello PlayReadyLicenseTemplate class represents a license template 
                // for creating PlayReady licenses
                // toobe returned toohello end users. 
                // It contains hello data on hello content key in hello license 
                // and any rights or restrictions toobe 
                // enforced by hello PlayReady DRM runtime when using hello content key.
                PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();

                // Configure whether hello license is persistent 
                // (saved in persistent storage on hello client) 
                // or non-persistent (only held in memory while hello player is using hello license).  
                licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;

                // AllowTestDevices controls whether test devices can use hello license or not.  
                // If true, hello MinimumSecurityLevel property of hello license
                // is set too150.  If false (hello default), 
                // hello MinimumSecurityLevel property of hello license is set too2000.
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

                // IMPORTANT: These types of restrictions can be very powerful 
                // but can also affect hello consumer experience. 
                // If hello output protections are configured too restrictive, 
                // hello content might be unplayable on some clients. 
                // For more information, see hello PlayReady Compliance Rules document.

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
                                required_output_protection =
                                    new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
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


            static public IContentKey CreateCommonTypeContentKey()
            {
                // Create envelope encryption content key
                Guid keyId = Guid.NewGuid();
                byte[] contentKey = GetRandomBuffer(16);

                IContentKey key = _context.ContentKeys.Create(
                                        keyId,
                                        contentKey,
                                        "ContentKey",
                                        ContentKeyType.CommonEncryption);

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
        }
    }

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Se även
[Använda PlayReady och/eller Widevine Dynamic Common Encryption](media-services-protect-with-drm.md)

[Med hjälp av AES-128 dynamisk kryptering och viktiga Delivery Service](media-services-protect-with-aes128.md)

[Med hjälp av partners toodeliver Widevine-licenser tooAzure Media Services](media-services-licenses-partner-integration.md)

