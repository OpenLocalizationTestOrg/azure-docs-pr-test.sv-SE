---
title: "aaaWidevine licens mall översikt | Microsoft Docs"
description: "Det här avsnittet ger en översikt över en Widevine-licensmall som används för tooconfigure Widevine-licenser."
author: juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 0e6f1f05-7ed6-4ed6-82a0-0cc2182b075a
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 67a6ae38cf3d3c21e1b7282aef15f79b21776414
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="widevine-license-template-overview"></a>Widevine-licens mall översikt
## <a name="overview"></a>Översikt
Azure Media Services kan du nu tooconfigure och begäran Widevine-licenser. När hello slutanvändaren player försöker tooplay är Widevine skyddat innehåll, en begäran skickades toohello licens leverans av tjänsten tooobtain en licens. Om hello Licenstjänsten godkänner hello begäran, den utfärdar hello-licens som skickade toohello klient och kan vara används toodecrypt och play hello angivna innehållet.

Widevine-licensbegäran formateras som ett JSON-meddelande.  

>[!NOTE]
> Du kan välja toocreate ett tomt meddelande med inga värden bara ”{}” och en licensmall för kommer att skapas med alla standardvärden. hello standardinställningen fungerar i de flesta fall. Till exempel för baserade MS licens leverans scenarier som ska alltid vara standard. Om du behöver tooset hello ”-providern” och ”content_id” värden, måste en provider matcha Googles Widevine autentiseringsuppgifter.

    {  
       “payload”:“<license challenge>”,
       “content_id”: “<content id>” 
       “provider”: ”<provider>”
       “allowed_track_types”:“<types>”,
       “content_key_specs”:[  
          {  
             “track_type”:“<track type 1>”
          },
          {  
             “track_type”:“<track type 2>”
          },
          …
       ],
       “policy_overrides”:{  
          “can_play”:<can play>,
          “can persist”:<can persist>,
          “can_renew”:<can renew>,
          “rental_duration_seconds”:<rental duration>,
          “playback_duration_seconds”:<playback duration>,
          “license_duration_seconds”:<license duration>,
          “renewal_recovery_duration_seconds”:<renewal recovery duration>,
          “renewal_server_url”:”<renewal server url>”,
          “renewal_delay_seconds”:<renewal delay>,
          “renewal_retry_interval_seconds”:<renewal retry interval>,
          “renew_with_usage”:<renew with usage>
       }
    }

## <a name="json-message"></a>JSON-meddelande
| Namn | Värde | Beskrivning |
| --- | --- | --- |
| nyttolast |Base64-kodad sträng |hello licensbegäran som skickades av en klient. |
| content_id |Base64-kodad sträng |Identifieraren används tooderive KeyId(s) och innehåll nycklar för varje content_key_specs.track_type. |
| Providern |Sträng |Använda toolook in innehåll nycklar och principer. Den här parametern ignoreras om MS viktiga leverans används för leverans av Widevine-licens. |
| principens_namn |Sträng |Namnet på en tidigare registrerad princip. Valfri |
| allowed_track_types |Enum |SD_ONLY eller SD_HD. Kontroller som innehåll nycklar som ska ingå i en licens |
| content_key_specs |matris av JSON-strukturer, se **innehåll nyckeln specifikationerna** nedan |En ökad kornat kontroll på vilket innehåll nycklar tooreturn. Mer information finns i innehåll nyckeln Spec nedan.  Endast ett av allowed_track_types och content_key_specs kan anges. |
| use_policy_overrides_exclusively |Booleskt värde. True eller false |Använd attribut som angetts av policy_overrides och utelämna alla tidigare lagrad princip. |
| policy_overrides |JSON-strukturen finns **principen åsidosätter** nedan |Principinställningar för denna licens.  I händelse av hello tillgången har en fördefinierad princip, de angivna värdena används. |
| session_init |JSON-strukturen finns **Session-initialisering** nedan |Valfria data skickas toolicense. |
| parse_only |Booleskt värde. True eller false |begäran om hello licensserver tolkas men ingen licens har utfärdats. Dock returneras värden formuläret hello licensbegäran hello svar. |

## <a name="content-key-specs"></a>Specifikationer för innehållsnyckeln
Om en befintlig princip finns finns det inga måste toospecify någon hello värdena i hello innehåll nyckel-specifikationen.  hello befintlig principen som är associerad med det här innehållet kommer att använda toodetermine hello utdata skydd, till exempel HDCP och CGMS.  Om en befintlig princip inte är registrerad med hello Widevine licensservern hello innehållsleverantören mata in hello värden i hello licensbegäran.   

Varje content_key_specs måste anges för alla spår oavsett hello alternativet use_policy_overrides_exclusively. 

| Namn | Värde | Beskrivning |
| --- | --- | --- |
| content_key_specs. track_type |Sträng |Ett namn för spåra. Om content_key_specs har angetts i hello licensbegäran, kontrollerar du att toospecify alla spåra typer explicit. Fel toodo så kommer fel tooplayback senaste 10 sekunder. |
| content_key_specs  <br/> security_level |UInt32 |Definierar stabilitet klientkrav för uppspelning. <br/> 1 - programvarubaserad whitebox crypto krävs. <br/> 2 - programvara crypto och en dolda avkodare krävs. <br/> 3 - hello viktiga material och krypto åtgärder måste utföras i en miljö med maskinvara säkerhetskopieras betrodda körning. <br/> 4 - hello kryptografi och avkodning av innehåll måste utföras i en miljö med maskinvara säkerhetskopieras betrodda körning.  <br/> 5 - hello kryptografi, avkodning och alla hantering av hello media (komprimerade och okomprimerade) måste hanteras i en miljö med maskinvara säkerhetskopieras betrodda körning. |
| content_key_specs <br/> required_output_protection.hDC |String - en av: HDCP_NONE, HDCP_V1, HDCP_V2 |Anger om HDCP behövs |
| content_key_specs <br/>key |Base64 <br/>kodad sträng |Innehåll viktiga toouse för den här spår. Om anges hello track_type eller key_id krävs.  Det här alternativet kan hello innehållsleverantören tooinject hello innehållsnyckeln för spåret i stället för att låta Widevine licensservern Generera eller söka efter en nyckel. |
| content_key_specs.key_id |Base64-kodad sträng binära, 16 byte |Unik identifierare för hello nyckeln. |

## <a name="policy-overrides"></a>Princip för åsidosättningar
| Namn | Värde | Beskrivning |
| --- | --- | --- |
| policy_overrides. can_play |Booleskt värde. True eller false |Anger att uppspelningen hello innehåll är tillåtna. Standardvärdet är false. |
| policy_overrides. can_persist |Booleskt värde. True eller false |Anger att hello-licens kan vara beständiga toonon volatilitet lagring för användning offline. Standardvärdet är false. |
| policy_overrides. can_renew |booleska true eller false |Anger att förnyelse av denna licens tillåts. Om värdet är true kan hello varaktighet hello-licens utökas genom pulsslag. Standardvärdet är false. |
| policy_overrides. license_duration_seconds |Int64 |Anger hello tidsfönstret för den här specifika licensen. Värdet 0 anger att det finns ingen gräns toohello varaktighet. Standardvärdet är 0. |
| policy_overrides. rental_duration_seconds |Int64 |Anger hello tidsfönstret när uppspelningen är tillåtet. Värdet 0 anger att det finns ingen gräns toohello varaktighet. Standardvärdet är 0. |
| policy_overrides. playback_duration_seconds |Int64 |hello visar fönstret tid när uppspelning startas inom hello licens varaktighet. Värdet 0 anger att det finns ingen gräns toohello varaktighet. Standardvärdet är 0. |
| policy_overrides. renewal_server_url |Sträng |Alla pulsslag (förnyelse)-begäranden för denna licens skall dirigeras toohello specificerat URL: en. Det här fältet används endast om can_renew är true. |
| policy_overrides. renewal_delay_seconds |Int64 |Hur många sekunder efter license_start_time innan förnyelse prövas först. Det här fältet används endast om can_renew är true. Standardvärdet är 0 |
| policy_overrides. renewal_retry_interval_seconds |Int64 |Anger hello fördröjning i sekunder mellan efterföljande licens förnyelse begäranden om fel uppstår. Det här fältet används endast om can_renew är true. |
| policy_overrides. renewal_recovery_duration_seconds |Int64 |hello tidsfönstret, som uppspelning tillåts toocontinue vid förnyelse är försök, ännu misslyckas på grund av toobackend problem med hello licensservern. Värdet 0 anger att det finns ingen gräns toohello varaktighet. Det här fältet används endast om can_renew är true. |
| policy_overrides. renew_with_usage |booleska true eller false |Anger den hello licensen skall skickas för förnyelse när användningen är igång. Det här fältet används endast om can_renew är true. |

## <a name="session-initialization"></a>Sessionsinitieringen av
| Namn | Värde | Beskrivning |
| --- | --- | --- |
| provider_session_token |Base64-kodad sträng |Den här sessionstoken som skickas tillbaka hello licens och kommer att finnas i efterföljande uppdateringar.  kommer inte att spara hello sessionstoken utöver sessioner. |
| provider_client_token |Base64-kodad sträng |Klienten token toosend igen hello licens svar.  Värdet ignoreras om hello licensbegäran innehåller en klienttoken. Hej klienttoken behålls utöver licens sessioner. |
| override_provider_client_token |Booleskt värde. True eller false |Om false och hello licens-begäran innehåller en klienttoken, använder du hello-token från hello begäran även om en klienttoken har angetts i den här strukturen.  Om värdet är true Använd alltid hello-token som angetts i den här strukturen. |

## <a name="configure-your-widevine-licenses-using-net-types"></a>Konfigurera dina Widevine-licenser med hjälp av .NET-typer
Media Services tillhandahåller .NET API: er som kan du konfigurera Widevine-licenser. 

### <a name="classes-as-defined-in-hello-media-services-net-sdk"></a>Klasser som definieras i hello Media Services .NET SDK
hello följande är hello definitioner av de här typerna.

    public class WidevineMessage
    {
        public WidevineMessage();

        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public AllowedTrackTypes? allowed_track_types { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public ContentKeySpecs[] content_key_specs { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public object policy_overrides { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum AllowedTrackTypes
    {
        SD_ONLY = 0,
        SD_HD = 1
    }
    public class ContentKeySpecs
    {
        public ContentKeySpecs();

        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string key_id { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public RequiredOutputProtection required_output_protection { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public int? security_level { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string track_type { get; set; }
    }

    public class RequiredOutputProtection
    {
        public RequiredOutputProtection();

        public Hdcp hdcp { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum Hdcp
    {
        HDCP_NONE = 0,
        HDCP_V1 = 1,
        HDCP_V2 = 2
    }

### <a name="example"></a>Exempel
följande exempel visar hur hello toouse .NET API: er tooconfigure en enkel Widevine-licens.

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


## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Se även
[Använda PlayReady och/eller Widevine Dynamic Common Encryption](media-services-protect-with-drm.md)

