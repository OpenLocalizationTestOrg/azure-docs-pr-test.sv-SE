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
# <a name="widevine-license-template-overview"></a><span data-ttu-id="ec04f-103">Widevine-licens mall översikt</span><span class="sxs-lookup"><span data-stu-id="ec04f-103">Widevine license template overview</span></span>
## <a name="overview"></a><span data-ttu-id="ec04f-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="ec04f-104">Overview</span></span>
<span data-ttu-id="ec04f-105">Azure Media Services kan du nu tooconfigure och begäran Widevine-licenser.</span><span class="sxs-lookup"><span data-stu-id="ec04f-105">Azure Media Services now enables you tooconfigure and request Widevine licenses.</span></span> <span data-ttu-id="ec04f-106">När hello slutanvändaren player försöker tooplay är Widevine skyddat innehåll, en begäran skickades toohello licens leverans av tjänsten tooobtain en licens.</span><span class="sxs-lookup"><span data-stu-id="ec04f-106">When hello end user player tries tooplay your Widevine protected content, a request is sent toohello license delivery service tooobtain a license.</span></span> <span data-ttu-id="ec04f-107">Om hello Licenstjänsten godkänner hello begäran, den utfärdar hello-licens som skickade toohello klient och kan vara används toodecrypt och play hello angivna innehållet.</span><span class="sxs-lookup"><span data-stu-id="ec04f-107">If hello license service approves hello request, it issues hello license which is sent toohello client and can be used toodecrypt and play hello specified content.</span></span>

<span data-ttu-id="ec04f-108">Widevine-licensbegäran formateras som ett JSON-meddelande.</span><span class="sxs-lookup"><span data-stu-id="ec04f-108">Widevine license request is formatted as a JSON message.</span></span>  

>[!NOTE]
> <span data-ttu-id="ec04f-109">Du kan välja toocreate ett tomt meddelande med inga värden bara ”{}” och en licensmall för kommer att skapas med alla standardvärden.</span><span class="sxs-lookup"><span data-stu-id="ec04f-109">You can choose toocreate an empty message with no values just "{}" and a license template will be created with all defaults.</span></span> <span data-ttu-id="ec04f-110">hello standardinställningen fungerar i de flesta fall.</span><span class="sxs-lookup"><span data-stu-id="ec04f-110">hello default works for most cases.</span></span> <span data-ttu-id="ec04f-111">Till exempel för baserade MS licens leverans scenarier som ska alltid vara standard.</span><span class="sxs-lookup"><span data-stu-id="ec04f-111">For example, for MS based license delivery scenarios that should always be default.</span></span> <span data-ttu-id="ec04f-112">Om du behöver tooset hello ”-providern” och ”content_id” värden, måste en provider matcha Googles Widevine autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="ec04f-112">If you do need tooset hello "provider" and "content_id" values, a provider must match Google's Widevine credentials.</span></span>

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

## <a name="json-message"></a><span data-ttu-id="ec04f-113">JSON-meddelande</span><span class="sxs-lookup"><span data-stu-id="ec04f-113">JSON message</span></span>
| <span data-ttu-id="ec04f-114">Namn</span><span class="sxs-lookup"><span data-stu-id="ec04f-114">Name</span></span> | <span data-ttu-id="ec04f-115">Värde</span><span class="sxs-lookup"><span data-stu-id="ec04f-115">Value</span></span> | <span data-ttu-id="ec04f-116">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ec04f-116">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ec04f-117">nyttolast</span><span class="sxs-lookup"><span data-stu-id="ec04f-117">payload</span></span> |<span data-ttu-id="ec04f-118">Base64-kodad sträng</span><span class="sxs-lookup"><span data-stu-id="ec04f-118">Base64 encoded string</span></span> |<span data-ttu-id="ec04f-119">hello licensbegäran som skickades av en klient.</span><span class="sxs-lookup"><span data-stu-id="ec04f-119">hello license request sent by a client.</span></span> |
| <span data-ttu-id="ec04f-120">content_id</span><span class="sxs-lookup"><span data-stu-id="ec04f-120">content_id</span></span> |<span data-ttu-id="ec04f-121">Base64-kodad sträng</span><span class="sxs-lookup"><span data-stu-id="ec04f-121">Base64 encoded string</span></span> |<span data-ttu-id="ec04f-122">Identifieraren används tooderive KeyId(s) och innehåll nycklar för varje content_key_specs.track_type.</span><span class="sxs-lookup"><span data-stu-id="ec04f-122">Identifier used tooderive KeyId(s) and Content Key(s) for each content_key_specs.track_type.</span></span> |
| <span data-ttu-id="ec04f-123">Providern</span><span class="sxs-lookup"><span data-stu-id="ec04f-123">provider</span></span> |<span data-ttu-id="ec04f-124">Sträng</span><span class="sxs-lookup"><span data-stu-id="ec04f-124">string</span></span> |<span data-ttu-id="ec04f-125">Använda toolook in innehåll nycklar och principer.</span><span class="sxs-lookup"><span data-stu-id="ec04f-125">Used toolook up content keys and policies.</span></span> <span data-ttu-id="ec04f-126">Den här parametern ignoreras om MS viktiga leverans används för leverans av Widevine-licens.</span><span class="sxs-lookup"><span data-stu-id="ec04f-126">If MS key delivery is used for Widevine license delivery, this parameter is ignored.</span></span> |
| <span data-ttu-id="ec04f-127">principens_namn</span><span class="sxs-lookup"><span data-stu-id="ec04f-127">policy_name</span></span> |<span data-ttu-id="ec04f-128">Sträng</span><span class="sxs-lookup"><span data-stu-id="ec04f-128">string</span></span> |<span data-ttu-id="ec04f-129">Namnet på en tidigare registrerad princip.</span><span class="sxs-lookup"><span data-stu-id="ec04f-129">Name of a previously registered policy.</span></span> <span data-ttu-id="ec04f-130">Valfri</span><span class="sxs-lookup"><span data-stu-id="ec04f-130">Optional</span></span> |
| <span data-ttu-id="ec04f-131">allowed_track_types</span><span class="sxs-lookup"><span data-stu-id="ec04f-131">allowed_track_types</span></span> |<span data-ttu-id="ec04f-132">Enum</span><span class="sxs-lookup"><span data-stu-id="ec04f-132">enum</span></span> |<span data-ttu-id="ec04f-133">SD_ONLY eller SD_HD.</span><span class="sxs-lookup"><span data-stu-id="ec04f-133">SD_ONLY or SD_HD.</span></span> <span data-ttu-id="ec04f-134">Kontroller som innehåll nycklar som ska ingå i en licens</span><span class="sxs-lookup"><span data-stu-id="ec04f-134">Controls which content keys should be included in a license</span></span> |
| <span data-ttu-id="ec04f-135">content_key_specs</span><span class="sxs-lookup"><span data-stu-id="ec04f-135">content_key_specs</span></span> |<span data-ttu-id="ec04f-136">matris av JSON-strukturer, se **innehåll nyckeln specifikationerna** nedan</span><span class="sxs-lookup"><span data-stu-id="ec04f-136">array of JSON structures, see **Content Key Specs** below</span></span> |<span data-ttu-id="ec04f-137">En ökad kornat kontroll på vilket innehåll nycklar tooreturn.</span><span class="sxs-lookup"><span data-stu-id="ec04f-137">A finer grained control on what content keys tooreturn.</span></span> <span data-ttu-id="ec04f-138">Mer information finns i innehåll nyckeln Spec nedan.</span><span class="sxs-lookup"><span data-stu-id="ec04f-138">See Content Key Spec below for details.</span></span>  <span data-ttu-id="ec04f-139">Endast ett av allowed_track_types och content_key_specs kan anges.</span><span class="sxs-lookup"><span data-stu-id="ec04f-139">Only one of allowed_track_types and content_key_specs can be specified.</span></span> |
| <span data-ttu-id="ec04f-140">use_policy_overrides_exclusively</span><span class="sxs-lookup"><span data-stu-id="ec04f-140">use_policy_overrides_exclusively</span></span> |<span data-ttu-id="ec04f-141">Booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="ec04f-141">boolean.</span></span> <span data-ttu-id="ec04f-142">True eller false</span><span class="sxs-lookup"><span data-stu-id="ec04f-142">true or false</span></span> |<span data-ttu-id="ec04f-143">Använd attribut som angetts av policy_overrides och utelämna alla tidigare lagrad princip.</span><span class="sxs-lookup"><span data-stu-id="ec04f-143">Use policy attributes specified by policy_overrides and omit all previously stored policy.</span></span> |
| <span data-ttu-id="ec04f-144">policy_overrides</span><span class="sxs-lookup"><span data-stu-id="ec04f-144">policy_overrides</span></span> |<span data-ttu-id="ec04f-145">JSON-strukturen finns **principen åsidosätter** nedan</span><span class="sxs-lookup"><span data-stu-id="ec04f-145">JSON structure, see **Policy Overrides** below</span></span> |<span data-ttu-id="ec04f-146">Principinställningar för denna licens.</span><span class="sxs-lookup"><span data-stu-id="ec04f-146">Policy settings for this license.</span></span>  <span data-ttu-id="ec04f-147">I händelse av hello tillgången har en fördefinierad princip, de angivna värdena används.</span><span class="sxs-lookup"><span data-stu-id="ec04f-147">In hello event this asset has a pre-defined policy, these specified values will be used.</span></span> |
| <span data-ttu-id="ec04f-148">session_init</span><span class="sxs-lookup"><span data-stu-id="ec04f-148">session_init</span></span> |<span data-ttu-id="ec04f-149">JSON-strukturen finns **Session-initialisering** nedan</span><span class="sxs-lookup"><span data-stu-id="ec04f-149">JSON structure, see **Session Initialization** below</span></span> |<span data-ttu-id="ec04f-150">Valfria data skickas toolicense.</span><span class="sxs-lookup"><span data-stu-id="ec04f-150">Optional data passed toolicense.</span></span> |
| <span data-ttu-id="ec04f-151">parse_only</span><span class="sxs-lookup"><span data-stu-id="ec04f-151">parse_only</span></span> |<span data-ttu-id="ec04f-152">Booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="ec04f-152">boolean.</span></span> <span data-ttu-id="ec04f-153">True eller false</span><span class="sxs-lookup"><span data-stu-id="ec04f-153">true or false</span></span> |<span data-ttu-id="ec04f-154">begäran om hello licensserver tolkas men ingen licens har utfärdats.</span><span class="sxs-lookup"><span data-stu-id="ec04f-154">hello license request is parsed but no license is issued.</span></span> <span data-ttu-id="ec04f-155">Dock returneras värden formuläret hello licensbegäran hello svar.</span><span class="sxs-lookup"><span data-stu-id="ec04f-155">However, values form hello license request are returned in hello response.</span></span> |

## <a name="content-key-specs"></a><span data-ttu-id="ec04f-156">Specifikationer för innehållsnyckeln</span><span class="sxs-lookup"><span data-stu-id="ec04f-156">Content Key Specs</span></span>
<span data-ttu-id="ec04f-157">Om en befintlig princip finns finns det inga måste toospecify någon hello värdena i hello innehåll nyckel-specifikationen.  hello befintlig principen som är associerad med det här innehållet kommer att använda toodetermine hello utdata skydd, till exempel HDCP och CGMS.</span><span class="sxs-lookup"><span data-stu-id="ec04f-157">If a pre-existing policy exist, there is no need toospecify any of hello values in hello Content Key Spec.  hello pre-existing policy associated with this content will be used toodetermine hello output protection such as HDCP and CGMS.</span></span>  <span data-ttu-id="ec04f-158">Om en befintlig princip inte är registrerad med hello Widevine licensservern hello innehållsleverantören mata in hello värden i hello licensbegäran.</span><span class="sxs-lookup"><span data-stu-id="ec04f-158">If a pre-existing policy is not registered with hello Widevine License Server, hello content provider can inject hello values into hello license request.</span></span>   

<span data-ttu-id="ec04f-159">Varje content_key_specs måste anges för alla spår oavsett hello alternativet use_policy_overrides_exclusively.</span><span class="sxs-lookup"><span data-stu-id="ec04f-159">Each content_key_specs must be specified for all tracks, regardless of hello option use_policy_overrides_exclusively.</span></span> 

| <span data-ttu-id="ec04f-160">Namn</span><span class="sxs-lookup"><span data-stu-id="ec04f-160">Name</span></span> | <span data-ttu-id="ec04f-161">Värde</span><span class="sxs-lookup"><span data-stu-id="ec04f-161">Value</span></span> | <span data-ttu-id="ec04f-162">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ec04f-162">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ec04f-163">content_key_specs.</span><span class="sxs-lookup"><span data-stu-id="ec04f-163">content_key_specs.</span></span> <span data-ttu-id="ec04f-164">track_type</span><span class="sxs-lookup"><span data-stu-id="ec04f-164">track_type</span></span> |<span data-ttu-id="ec04f-165">Sträng</span><span class="sxs-lookup"><span data-stu-id="ec04f-165">string</span></span> |<span data-ttu-id="ec04f-166">Ett namn för spåra.</span><span class="sxs-lookup"><span data-stu-id="ec04f-166">A track type name.</span></span> <span data-ttu-id="ec04f-167">Om content_key_specs har angetts i hello licensbegäran, kontrollerar du att toospecify alla spåra typer explicit.</span><span class="sxs-lookup"><span data-stu-id="ec04f-167">If content_key_specs is specified in hello license request, make sure toospecify all track types explicitly.</span></span> <span data-ttu-id="ec04f-168">Fel toodo så kommer fel tooplayback senaste 10 sekunder.</span><span class="sxs-lookup"><span data-stu-id="ec04f-168">Failure toodo so will result in failure tooplayback past 10 seconds.</span></span> |
| <span data-ttu-id="ec04f-169">content_key_specs</span><span class="sxs-lookup"><span data-stu-id="ec04f-169">content_key_specs</span></span>  <br/> <span data-ttu-id="ec04f-170">security_level</span><span class="sxs-lookup"><span data-stu-id="ec04f-170">security_level</span></span> |<span data-ttu-id="ec04f-171">UInt32</span><span class="sxs-lookup"><span data-stu-id="ec04f-171">uint32</span></span> |<span data-ttu-id="ec04f-172">Definierar stabilitet klientkrav för uppspelning.</span><span class="sxs-lookup"><span data-stu-id="ec04f-172">Defines client robustness requirements for playback.</span></span> <br/> <span data-ttu-id="ec04f-173">1 - programvarubaserad whitebox crypto krävs.</span><span class="sxs-lookup"><span data-stu-id="ec04f-173">1 - Software-based whitebox crypto is required.</span></span> <br/> <span data-ttu-id="ec04f-174">2 - programvara crypto och en dolda avkodare krävs.</span><span class="sxs-lookup"><span data-stu-id="ec04f-174">2 - Software crypto and an obfuscated decoder is required.</span></span> <br/> <span data-ttu-id="ec04f-175">3 - hello viktiga material och krypto åtgärder måste utföras i en miljö med maskinvara säkerhetskopieras betrodda körning.</span><span class="sxs-lookup"><span data-stu-id="ec04f-175">3 - hello key material and crypto operations must be performed within a hardware backed trusted execution environment.</span></span> <br/> <span data-ttu-id="ec04f-176">4 - hello kryptografi och avkodning av innehåll måste utföras i en miljö med maskinvara säkerhetskopieras betrodda körning.</span><span class="sxs-lookup"><span data-stu-id="ec04f-176">4 - hello crypto and decoding of content must be performed within a hardware backed trusted execution environment.</span></span>  <br/> <span data-ttu-id="ec04f-177">5 - hello kryptografi, avkodning och alla hantering av hello media (komprimerade och okomprimerade) måste hanteras i en miljö med maskinvara säkerhetskopieras betrodda körning.</span><span class="sxs-lookup"><span data-stu-id="ec04f-177">5 - hello crypto, decoding and all handling of hello media (compressed and uncompressed) must be handled within a hardware backed trusted execution environment.</span></span> |
| <span data-ttu-id="ec04f-178">content_key_specs</span><span class="sxs-lookup"><span data-stu-id="ec04f-178">content_key_specs</span></span> <br/> <span data-ttu-id="ec04f-179">required_output_protection.hDC</span><span class="sxs-lookup"><span data-stu-id="ec04f-179">required_output_protection.hdc</span></span> |<span data-ttu-id="ec04f-180">String - en av: HDCP_NONE, HDCP_V1, HDCP_V2</span><span class="sxs-lookup"><span data-stu-id="ec04f-180">string - one of: HDCP_NONE, HDCP_V1, HDCP_V2</span></span> |<span data-ttu-id="ec04f-181">Anger om HDCP behövs</span><span class="sxs-lookup"><span data-stu-id="ec04f-181">Indicates whether HDCP is require</span></span> |
| <span data-ttu-id="ec04f-182">content_key_specs</span><span class="sxs-lookup"><span data-stu-id="ec04f-182">content_key_specs</span></span> <br/><span data-ttu-id="ec04f-183">key</span><span class="sxs-lookup"><span data-stu-id="ec04f-183">key</span></span> |<span data-ttu-id="ec04f-184">Base64</span><span class="sxs-lookup"><span data-stu-id="ec04f-184">Base64</span></span> <br/><span data-ttu-id="ec04f-185">kodad sträng</span><span class="sxs-lookup"><span data-stu-id="ec04f-185">encoded string</span></span> |<span data-ttu-id="ec04f-186">Innehåll viktiga toouse för den här spår. Om anges hello track_type eller key_id krävs.</span><span class="sxs-lookup"><span data-stu-id="ec04f-186">Content key toouse for this track. If specified, hello track_type or key_id is required.</span></span>  <span data-ttu-id="ec04f-187">Det här alternativet kan hello innehållsleverantören tooinject hello innehållsnyckeln för spåret i stället för att låta Widevine licensservern Generera eller söka efter en nyckel.</span><span class="sxs-lookup"><span data-stu-id="ec04f-187">This option allows hello content provider tooinject hello content key for this track instead of letting Widevine license server generate or lookup a key.</span></span> |
| <span data-ttu-id="ec04f-188">content_key_specs.key_id</span><span class="sxs-lookup"><span data-stu-id="ec04f-188">content_key_specs.key_id</span></span> |<span data-ttu-id="ec04f-189">Base64-kodad sträng binära, 16 byte</span><span class="sxs-lookup"><span data-stu-id="ec04f-189">Base64 encoded string  binary, 16 bytes</span></span> |<span data-ttu-id="ec04f-190">Unik identifierare för hello nyckeln.</span><span class="sxs-lookup"><span data-stu-id="ec04f-190">Unique identifier for hello key.</span></span> |

## <a name="policy-overrides"></a><span data-ttu-id="ec04f-191">Princip för åsidosättningar</span><span class="sxs-lookup"><span data-stu-id="ec04f-191">Policy Overrides</span></span>
| <span data-ttu-id="ec04f-192">Namn</span><span class="sxs-lookup"><span data-stu-id="ec04f-192">Name</span></span> | <span data-ttu-id="ec04f-193">Värde</span><span class="sxs-lookup"><span data-stu-id="ec04f-193">Value</span></span> | <span data-ttu-id="ec04f-194">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ec04f-194">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ec04f-195">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="ec04f-195">policy_overrides.</span></span> <span data-ttu-id="ec04f-196">can_play</span><span class="sxs-lookup"><span data-stu-id="ec04f-196">can_play</span></span> |<span data-ttu-id="ec04f-197">Booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="ec04f-197">boolean.</span></span> <span data-ttu-id="ec04f-198">True eller false</span><span class="sxs-lookup"><span data-stu-id="ec04f-198">true or false</span></span> |<span data-ttu-id="ec04f-199">Anger att uppspelningen hello innehåll är tillåtna.</span><span class="sxs-lookup"><span data-stu-id="ec04f-199">Indicates that playback of hello content is allowed.</span></span> <span data-ttu-id="ec04f-200">Standardvärdet är false.</span><span class="sxs-lookup"><span data-stu-id="ec04f-200">Default is false.</span></span> |
| <span data-ttu-id="ec04f-201">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="ec04f-201">policy_overrides.</span></span> <span data-ttu-id="ec04f-202">can_persist</span><span class="sxs-lookup"><span data-stu-id="ec04f-202">can_persist</span></span> |<span data-ttu-id="ec04f-203">Booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="ec04f-203">boolean.</span></span> <span data-ttu-id="ec04f-204">True eller false</span><span class="sxs-lookup"><span data-stu-id="ec04f-204">true or false</span></span> |<span data-ttu-id="ec04f-205">Anger att hello-licens kan vara beständiga toonon volatilitet lagring för användning offline.</span><span class="sxs-lookup"><span data-stu-id="ec04f-205">Indicates that hello license may be persisted toonon-volatile storage for offline use.</span></span> <span data-ttu-id="ec04f-206">Standardvärdet är false.</span><span class="sxs-lookup"><span data-stu-id="ec04f-206">Default is false.</span></span> |
| <span data-ttu-id="ec04f-207">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="ec04f-207">policy_overrides.</span></span> <span data-ttu-id="ec04f-208">can_renew</span><span class="sxs-lookup"><span data-stu-id="ec04f-208">can_renew</span></span> |<span data-ttu-id="ec04f-209">booleska true eller false</span><span class="sxs-lookup"><span data-stu-id="ec04f-209">boolean true or false</span></span> |<span data-ttu-id="ec04f-210">Anger att förnyelse av denna licens tillåts.</span><span class="sxs-lookup"><span data-stu-id="ec04f-210">Indicates that renewal of this license is allowed.</span></span> <span data-ttu-id="ec04f-211">Om värdet är true kan hello varaktighet hello-licens utökas genom pulsslag.</span><span class="sxs-lookup"><span data-stu-id="ec04f-211">If true, hello duration of hello license can be extended by heartbeat.</span></span> <span data-ttu-id="ec04f-212">Standardvärdet är false.</span><span class="sxs-lookup"><span data-stu-id="ec04f-212">Default is false.</span></span> |
| <span data-ttu-id="ec04f-213">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="ec04f-213">policy_overrides.</span></span> <span data-ttu-id="ec04f-214">license_duration_seconds</span><span class="sxs-lookup"><span data-stu-id="ec04f-214">license_duration_seconds</span></span> |<span data-ttu-id="ec04f-215">Int64</span><span class="sxs-lookup"><span data-stu-id="ec04f-215">int64</span></span> |<span data-ttu-id="ec04f-216">Anger hello tidsfönstret för den här specifika licensen.</span><span class="sxs-lookup"><span data-stu-id="ec04f-216">Indicates hello time window for this specific license.</span></span> <span data-ttu-id="ec04f-217">Värdet 0 anger att det finns ingen gräns toohello varaktighet.</span><span class="sxs-lookup"><span data-stu-id="ec04f-217">A value of 0 indicates that there is no limit toohello duration.</span></span> <span data-ttu-id="ec04f-218">Standardvärdet är 0.</span><span class="sxs-lookup"><span data-stu-id="ec04f-218">Default is 0.</span></span> |
| <span data-ttu-id="ec04f-219">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="ec04f-219">policy_overrides.</span></span> <span data-ttu-id="ec04f-220">rental_duration_seconds</span><span class="sxs-lookup"><span data-stu-id="ec04f-220">rental_duration_seconds</span></span> |<span data-ttu-id="ec04f-221">Int64</span><span class="sxs-lookup"><span data-stu-id="ec04f-221">int64</span></span> |<span data-ttu-id="ec04f-222">Anger hello tidsfönstret när uppspelningen är tillåtet.</span><span class="sxs-lookup"><span data-stu-id="ec04f-222">Indicates hello time window while playback is permitted.</span></span> <span data-ttu-id="ec04f-223">Värdet 0 anger att det finns ingen gräns toohello varaktighet.</span><span class="sxs-lookup"><span data-stu-id="ec04f-223">A value of 0 indicates that there is no limit toohello duration.</span></span> <span data-ttu-id="ec04f-224">Standardvärdet är 0.</span><span class="sxs-lookup"><span data-stu-id="ec04f-224">Default is 0.</span></span> |
| <span data-ttu-id="ec04f-225">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="ec04f-225">policy_overrides.</span></span> <span data-ttu-id="ec04f-226">playback_duration_seconds</span><span class="sxs-lookup"><span data-stu-id="ec04f-226">playback_duration_seconds</span></span> |<span data-ttu-id="ec04f-227">Int64</span><span class="sxs-lookup"><span data-stu-id="ec04f-227">int64</span></span> |<span data-ttu-id="ec04f-228">hello visar fönstret tid när uppspelning startas inom hello licens varaktighet.</span><span class="sxs-lookup"><span data-stu-id="ec04f-228">hello viewing window of time once playback starts within hello license duration.</span></span> <span data-ttu-id="ec04f-229">Värdet 0 anger att det finns ingen gräns toohello varaktighet.</span><span class="sxs-lookup"><span data-stu-id="ec04f-229">A value of 0 indicates that there is no limit toohello duration.</span></span> <span data-ttu-id="ec04f-230">Standardvärdet är 0.</span><span class="sxs-lookup"><span data-stu-id="ec04f-230">Default is 0.</span></span> |
| <span data-ttu-id="ec04f-231">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="ec04f-231">policy_overrides.</span></span> <span data-ttu-id="ec04f-232">renewal_server_url</span><span class="sxs-lookup"><span data-stu-id="ec04f-232">renewal_server_url</span></span> |<span data-ttu-id="ec04f-233">Sträng</span><span class="sxs-lookup"><span data-stu-id="ec04f-233">string</span></span> |<span data-ttu-id="ec04f-234">Alla pulsslag (förnyelse)-begäranden för denna licens skall dirigeras toohello specificerat URL: en.</span><span class="sxs-lookup"><span data-stu-id="ec04f-234">All heartbeat (renewal) requests for this license shall be directed toohello specified URL.</span></span> <span data-ttu-id="ec04f-235">Det här fältet används endast om can_renew är true.</span><span class="sxs-lookup"><span data-stu-id="ec04f-235">This field is only used if can_renew is true.</span></span> |
| <span data-ttu-id="ec04f-236">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="ec04f-236">policy_overrides.</span></span> <span data-ttu-id="ec04f-237">renewal_delay_seconds</span><span class="sxs-lookup"><span data-stu-id="ec04f-237">renewal_delay_seconds</span></span> |<span data-ttu-id="ec04f-238">Int64</span><span class="sxs-lookup"><span data-stu-id="ec04f-238">int64</span></span> |<span data-ttu-id="ec04f-239">Hur många sekunder efter license_start_time innan förnyelse prövas först.</span><span class="sxs-lookup"><span data-stu-id="ec04f-239">How many seconds after license_start_time, before renewal is first attempted.</span></span> <span data-ttu-id="ec04f-240">Det här fältet används endast om can_renew är true.</span><span class="sxs-lookup"><span data-stu-id="ec04f-240">This field is only used if can_renew is true.</span></span> <span data-ttu-id="ec04f-241">Standardvärdet är 0</span><span class="sxs-lookup"><span data-stu-id="ec04f-241">Default is 0</span></span> |
| <span data-ttu-id="ec04f-242">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="ec04f-242">policy_overrides.</span></span> <span data-ttu-id="ec04f-243">renewal_retry_interval_seconds</span><span class="sxs-lookup"><span data-stu-id="ec04f-243">renewal_retry_interval_seconds</span></span> |<span data-ttu-id="ec04f-244">Int64</span><span class="sxs-lookup"><span data-stu-id="ec04f-244">int64</span></span> |<span data-ttu-id="ec04f-245">Anger hello fördröjning i sekunder mellan efterföljande licens förnyelse begäranden om fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="ec04f-245">Specifies hello delay in seconds between subsequent license renewal requests, in case of failure.</span></span> <span data-ttu-id="ec04f-246">Det här fältet används endast om can_renew är true.</span><span class="sxs-lookup"><span data-stu-id="ec04f-246">This field is only used if can_renew is true.</span></span> |
| <span data-ttu-id="ec04f-247">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="ec04f-247">policy_overrides.</span></span> <span data-ttu-id="ec04f-248">renewal_recovery_duration_seconds</span><span class="sxs-lookup"><span data-stu-id="ec04f-248">renewal_recovery_duration_seconds</span></span> |<span data-ttu-id="ec04f-249">Int64</span><span class="sxs-lookup"><span data-stu-id="ec04f-249">int64</span></span> |<span data-ttu-id="ec04f-250">hello tidsfönstret, som uppspelning tillåts toocontinue vid förnyelse är försök, ännu misslyckas på grund av toobackend problem med hello licensservern.</span><span class="sxs-lookup"><span data-stu-id="ec04f-250">hello window of time, in which playback is allowed toocontinue while renewal is attempted, yet unsuccessful due toobackend problems with hello license server.</span></span> <span data-ttu-id="ec04f-251">Värdet 0 anger att det finns ingen gräns toohello varaktighet.</span><span class="sxs-lookup"><span data-stu-id="ec04f-251">A value of 0 indicates that there is no limit toohello duration.</span></span> <span data-ttu-id="ec04f-252">Det här fältet används endast om can_renew är true.</span><span class="sxs-lookup"><span data-stu-id="ec04f-252">This field is only used if can_renew is true.</span></span> |
| <span data-ttu-id="ec04f-253">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="ec04f-253">policy_overrides.</span></span> <span data-ttu-id="ec04f-254">renew_with_usage</span><span class="sxs-lookup"><span data-stu-id="ec04f-254">renew_with_usage</span></span> |<span data-ttu-id="ec04f-255">booleska true eller false</span><span class="sxs-lookup"><span data-stu-id="ec04f-255">boolean true or false</span></span> |<span data-ttu-id="ec04f-256">Anger den hello licensen skall skickas för förnyelse när användningen är igång.</span><span class="sxs-lookup"><span data-stu-id="ec04f-256">Indicates that hello license shall be sent for renewal when usage is started.</span></span> <span data-ttu-id="ec04f-257">Det här fältet används endast om can_renew är true.</span><span class="sxs-lookup"><span data-stu-id="ec04f-257">This field is only used if can_renew is true.</span></span> |

## <a name="session-initialization"></a><span data-ttu-id="ec04f-258">Sessionsinitieringen av</span><span class="sxs-lookup"><span data-stu-id="ec04f-258">Session Initialization</span></span>
| <span data-ttu-id="ec04f-259">Namn</span><span class="sxs-lookup"><span data-stu-id="ec04f-259">Name</span></span> | <span data-ttu-id="ec04f-260">Värde</span><span class="sxs-lookup"><span data-stu-id="ec04f-260">Value</span></span> | <span data-ttu-id="ec04f-261">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ec04f-261">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ec04f-262">provider_session_token</span><span class="sxs-lookup"><span data-stu-id="ec04f-262">provider_session_token</span></span> |<span data-ttu-id="ec04f-263">Base64-kodad sträng</span><span class="sxs-lookup"><span data-stu-id="ec04f-263">Base64 encoded string</span></span> |<span data-ttu-id="ec04f-264">Den här sessionstoken som skickas tillbaka hello licens och kommer att finnas i efterföljande uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="ec04f-264">This session token is passed back in hello license and will exist in subsequent renewals.</span></span>  <span data-ttu-id="ec04f-265">kommer inte att spara hello sessionstoken utöver sessioner.</span><span class="sxs-lookup"><span data-stu-id="ec04f-265">hello session token will not persist beyond sessions.</span></span> |
| <span data-ttu-id="ec04f-266">provider_client_token</span><span class="sxs-lookup"><span data-stu-id="ec04f-266">provider_client_token</span></span> |<span data-ttu-id="ec04f-267">Base64-kodad sträng</span><span class="sxs-lookup"><span data-stu-id="ec04f-267">Base64 encoded string</span></span> |<span data-ttu-id="ec04f-268">Klienten token toosend igen hello licens svar.</span><span class="sxs-lookup"><span data-stu-id="ec04f-268">Client token toosend back in hello license response.</span></span>  <span data-ttu-id="ec04f-269">Värdet ignoreras om hello licensbegäran innehåller en klienttoken.</span><span class="sxs-lookup"><span data-stu-id="ec04f-269">If hello license request contains a client token, this value is ignored.</span></span> <span data-ttu-id="ec04f-270">Hej klienttoken behålls utöver licens sessioner.</span><span class="sxs-lookup"><span data-stu-id="ec04f-270">hello client token will persist beyond license sessions.</span></span> |
| <span data-ttu-id="ec04f-271">override_provider_client_token</span><span class="sxs-lookup"><span data-stu-id="ec04f-271">override_provider_client_token</span></span> |<span data-ttu-id="ec04f-272">Booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="ec04f-272">boolean.</span></span> <span data-ttu-id="ec04f-273">True eller false</span><span class="sxs-lookup"><span data-stu-id="ec04f-273">true or false</span></span> |<span data-ttu-id="ec04f-274">Om false och hello licens-begäran innehåller en klienttoken, använder du hello-token från hello begäran även om en klienttoken har angetts i den här strukturen.</span><span class="sxs-lookup"><span data-stu-id="ec04f-274">If false and hello license request contains a client token, use hello token from hello request even if a client token was specified in this structure.</span></span>  <span data-ttu-id="ec04f-275">Om värdet är true Använd alltid hello-token som angetts i den här strukturen.</span><span class="sxs-lookup"><span data-stu-id="ec04f-275">If true, always use hello token specified in this structure.</span></span> |

## <a name="configure-your-widevine-licenses-using-net-types"></a><span data-ttu-id="ec04f-276">Konfigurera dina Widevine-licenser med hjälp av .NET-typer</span><span class="sxs-lookup"><span data-stu-id="ec04f-276">Configure your Widevine licenses using .NET types</span></span>
<span data-ttu-id="ec04f-277">Media Services tillhandahåller .NET API: er som kan du konfigurera Widevine-licenser.</span><span class="sxs-lookup"><span data-stu-id="ec04f-277">Media Services provides .NET APIs that let you configure your Widevine licenses.</span></span> 

### <a name="classes-as-defined-in-hello-media-services-net-sdk"></a><span data-ttu-id="ec04f-278">Klasser som definieras i hello Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="ec04f-278">Classes as defined in hello Media Services .NET SDK</span></span>
<span data-ttu-id="ec04f-279">hello följande är hello definitioner av de här typerna.</span><span class="sxs-lookup"><span data-stu-id="ec04f-279">hello following are hello definitions of these types.</span></span>

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

### <a name="example"></a><span data-ttu-id="ec04f-280">Exempel</span><span class="sxs-lookup"><span data-stu-id="ec04f-280">Example</span></span>
<span data-ttu-id="ec04f-281">följande exempel visar hur hello toouse .NET API: er tooconfigure en enkel Widevine-licens.</span><span class="sxs-lookup"><span data-stu-id="ec04f-281">hello following example shows how toouse .NET APIs tooconfigure  a simple Widevine license.</span></span>

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


## <a name="media-services-learning-paths"></a><span data-ttu-id="ec04f-282">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="ec04f-282">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="ec04f-283">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="ec04f-283">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="ec04f-284">Se även</span><span class="sxs-lookup"><span data-stu-id="ec04f-284">See also</span></span>
[<span data-ttu-id="ec04f-285">Använda PlayReady och/eller Widevine Dynamic Common Encryption</span><span class="sxs-lookup"><span data-stu-id="ec04f-285">Using PlayReady and/or Widevine Dynamic Common Encryption</span></span>](media-services-protect-with-drm.md)

