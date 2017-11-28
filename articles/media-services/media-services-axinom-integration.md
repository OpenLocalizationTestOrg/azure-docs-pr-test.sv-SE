---
title: aaaUsing Axinom toodeliver Widevine-licenser tooAzure Media Services | Microsoft Docs
description: "Den här artikeln beskriver hur du kan använda Azure Media Services (AMS) toodeliver en dataström som krypteras dynamiskt med AMS med både PlayReady och Widevine DRMs. hello PlayReady-licens kommer från licensserver för Media Services PlayReady och Widevine-licens levereras med Axinom licensservern."
services: media-services
documentationcenter: 
author: willzhan
manager: cfowler
editor: 
ms.assetid: 9c93fa4e-b4da-4774-ab6d-8b12b371631d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: willzhan;Mingfeiy;rajputam;Juliako
ms.openlocfilehash: 2245d9269c30712ef779973ae021c00c76174d0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-axinom-toodeliver-widevine-licenses-tooazure-media-services"></a><span data-ttu-id="f4072-104">Med hjälp av Axinom toodeliver Widevine-licenser tooAzure Media Services</span><span class="sxs-lookup"><span data-stu-id="f4072-104">Using Axinom toodeliver Widevine licenses tooAzure Media Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f4072-105">castLabs</span><span class="sxs-lookup"><span data-stu-id="f4072-105">castLabs</span></span>](media-services-castlabs-integration.md)
> * [<span data-ttu-id="f4072-106">Axinom</span><span class="sxs-lookup"><span data-stu-id="f4072-106">Axinom</span></span>](media-services-axinom-integration.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="f4072-107">Översikt</span><span class="sxs-lookup"><span data-stu-id="f4072-107">Overview</span></span>
<span data-ttu-id="f4072-108">Azure Media Services (AMS) har lagt till Google Widevines dynamiska skydd (se [Mingfei's blog](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/) information).</span><span class="sxs-lookup"><span data-stu-id="f4072-108">Azure Media Services (AMS) has added Google Widevine dynamic protection (see [Mingfei’s blog](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/) for details).</span></span> <span data-ttu-id="f4072-109">Dessutom Azure Media Player (APM) också har lagt till stöd för Widevine (se [AMP dokumentet](http://amp.azure.net/libs/amp/latest/docs/) information).</span><span class="sxs-lookup"><span data-stu-id="f4072-109">In addition, Azure Media Player (AMP) has also added Widevine support (see [AMP document](http://amp.azure.net/libs/amp/latest/docs/) for details).</span></span> <span data-ttu-id="f4072-110">Det här är en större fullgöra med strömmande DASH-innehåll som skyddas av CENC med flera-native-DRM (PlayReady och Widevine) på moderna webbläsare utrustad med mus och EME.</span><span class="sxs-lookup"><span data-stu-id="f4072-110">This is a major accomplishment in streaming DASH content protected by CENC with multi-native-DRM (PlayReady and Widevine) on modern browsers equipped with MSE and EME.</span></span>

<span data-ttu-id="f4072-111">Från och med hello Media Services .NET SDK version 3.5.2 Media Services kan du tooconfigure Widevine-licensmall och hämta Widevine-licenser.</span><span class="sxs-lookup"><span data-stu-id="f4072-111">Starting with hello Media Services .NET SDK version 3.5.2, Media Services enables you tooconfigure Widevine license template and get Widevine licenses.</span></span> <span data-ttu-id="f4072-112">Du kan också använda följande AMS-partner toohelp du leverera Widevine-licenser hello: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).</span><span class="sxs-lookup"><span data-stu-id="f4072-112">You can also use hello following AMS partners toohelp you deliver Widevine licenses: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).</span></span>

<span data-ttu-id="f4072-113">Den här artikeln beskriver hur toointegrate och testa Widevine-licens server som hanteras av Axinom.</span><span class="sxs-lookup"><span data-stu-id="f4072-113">This article describes how toointegrate and test Widevine license server managed by Axinom.</span></span> <span data-ttu-id="f4072-114">Exempel:</span><span class="sxs-lookup"><span data-stu-id="f4072-114">Specifically, it covers:</span></span>  

* <span data-ttu-id="f4072-115">Konfigurera dynamic Common Encryption med multi-DRM (PlayReady och Widevine) med motsvarande licens förvärv webbadresserna.</span><span class="sxs-lookup"><span data-stu-id="f4072-115">Configuring dynamic Common Encryption with multi-DRM (PlayReady and Widevine) with corresponding license acquisition URLs;</span></span>
* <span data-ttu-id="f4072-116">Generera en JWT-token i ordning toomeet hello server licenskrav;</span><span class="sxs-lookup"><span data-stu-id="f4072-116">Generating a JWT token in order toomeet hello license server requirements;</span></span>
* <span data-ttu-id="f4072-117">Utveckla Azure Media Player-app som hanterar licenser med JWT-token autentisering.</span><span class="sxs-lookup"><span data-stu-id="f4072-117">Developing Azure Media Player app which handles license acquisition with JWT token authentication;</span></span>

<span data-ttu-id="f4072-118">Hej hela systemet och hello flödet av innehåll som är viktiga, nyckel-ID, nyckel seed, JTW token och dess anspråk kan beskrivas bäst med hello följande diagram.</span><span class="sxs-lookup"><span data-stu-id="f4072-118">hello complete system and hello flow of content key, key ID, key seed, JTW token and its claims can be best described by hello following diagram.</span></span>

![DASH och CENC](./media/media-services-axinom-integration/media-services-axinom1.png)

## <a name="content-protection"></a><span data-ttu-id="f4072-120">Content Protection</span><span class="sxs-lookup"><span data-stu-id="f4072-120">Content Protection</span></span>
<span data-ttu-id="f4072-121">Konfigurera dynamisk skydd och viktiga leveransprincipen finns Mingfei's blog: [hur tooconfigure Widevine-paketering med Azure Media Services](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services).</span><span class="sxs-lookup"><span data-stu-id="f4072-121">For configuring dynamic protection and key delivery policy, please see Mingfei’s blog: [How tooconfigure Widevine packaging with Azure Media Services](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services).</span></span>

<span data-ttu-id="f4072-122">Du kan konfigurera dynamisk CENC skydd med multi-DRM för DASH strömning båda hello följande:</span><span class="sxs-lookup"><span data-stu-id="f4072-122">You can configure dynamic CENC protection with multi-DRM for DASH streaming having both of hello following:</span></span>

1. <span data-ttu-id="f4072-123">PlayReady-skydd för MS Edge och IE11 som kan ha en token auktoriseringsbegränsningar.</span><span class="sxs-lookup"><span data-stu-id="f4072-123">PlayReady protection for MS Edge and IE11, that could have a token authorization restrictions.</span></span> <span data-ttu-id="f4072-124">Hej tokenbegränsade principen måste åtföljas av en token som utfärdas av en säker säkerhetstokentjänst (STS), till exempel Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f4072-124">hello token restricted policy must be accompanied by a token issued by a Secure Token Service (STS), such as Azure Active Directory;</span></span>
2. <span data-ttu-id="f4072-125">Widevine skydd för Chrome, kan det kräva token autentisering med token som utfärdas av en annan STS.</span><span class="sxs-lookup"><span data-stu-id="f4072-125">Widevine protection for Chrome, it can require token authentication with token issued by another STS.</span></span> 

<span data-ttu-id="f4072-126">Se [JWT-Token generering](media-services-axinom-integration.md#jwt-token-generation) avsnittet varför Azure Active Directory kan inte användas som en STS för Axinom's Widevine licensservern.</span><span class="sxs-lookup"><span data-stu-id="f4072-126">Please see [JWT Token Generation](media-services-axinom-integration.md#jwt-token-generation) section for why Azure Active Directory cannot be used as an STS for Axinom’s Widevine license server.</span></span>

### <a name="considerations"></a><span data-ttu-id="f4072-127">Överväganden</span><span class="sxs-lookup"><span data-stu-id="f4072-127">Considerations</span></span>
1. <span data-ttu-id="f4072-128">Du måste använda hello Axinom angetts viktiga startvärde (8888000000000000000000000000000000000000) och genererade eller markerade nyckel ID toogenerate hello innehållet nyckel för att konfigurera viktiga delivery service.</span><span class="sxs-lookup"><span data-stu-id="f4072-128">You must use hello Axinom specified key seed (8888000000000000000000000000000000000000) and your generated or selected key ID toogenerate hello content key for configuring key delivery service.</span></span> <span data-ttu-id="f4072-129">Axinom server kommer utfärda alla licenser som innehåller innehåll nycklar baserat på hello samma nyckel startvärde som inte är giltig för testning och produktion.</span><span class="sxs-lookup"><span data-stu-id="f4072-129">Axinom license server will issue all licenses containing content keys based on hello same key seed, which is valid for both testing and production.</span></span>
2. <span data-ttu-id="f4072-130">Hej Widevine-licens URL för anskaffning för testning: [https://drm-widevine-licensing.axtest.net/AcquireLicense](https://drm-widevine-licensing.axtest.net/AcquireLicense).</span><span class="sxs-lookup"><span data-stu-id="f4072-130">hello Widevine license acquisition URL for testing: [https://drm-widevine-licensing.axtest.net/AcquireLicense](https://drm-widevine-licensing.axtest.net/AcquireLicense).</span></span> <span data-ttu-id="f4072-131">Både HTTP och HTTS tillåts.</span><span class="sxs-lookup"><span data-stu-id="f4072-131">Both HTTP and HTTS are allowed.</span></span>

## <a name="azure-media-player-preparation"></a><span data-ttu-id="f4072-132">Förberedelser för Azure Media Player</span><span class="sxs-lookup"><span data-stu-id="f4072-132">Azure Media Player Preparation</span></span>
<span data-ttu-id="f4072-133">AMP v1.4.0 stöder uppspelning av AMS-innehåll som är packade dynamiskt med PlayReady och Widevine DRM.</span><span class="sxs-lookup"><span data-stu-id="f4072-133">AMP v1.4.0 supports playback of AMS content that is dynamically packaged with both PlayReady and Widevine DRM.</span></span>
<span data-ttu-id="f4072-134">Om Widevine licensservern inte kräver tokenautentisering, är det ingenting ytterligare måste toodo tootest ett STRECK innehåll skyddas av Widevine.</span><span class="sxs-lookup"><span data-stu-id="f4072-134">If Widevine license server does not require token authentication, there is nothing additional you need toodo tootest a DASH content protected by Widevine.</span></span> <span data-ttu-id="f4072-135">Ett exempel hello AMP tillhandahåller en enkel [exempel](http://amp.azure.net/libs/amp/latest/samples/dynamic_multiDRM_PlayReadyWidevine_notoken.html), där du kan se den arbetar i kant och IE11 med PlayReady och Chrome med Widevine.</span><span class="sxs-lookup"><span data-stu-id="f4072-135">For an example, hello AMP team provides a simple [sample](http://amp.azure.net/libs/amp/latest/samples/dynamic_multiDRM_PlayReadyWidevine_notoken.html), where you can see it working in Edge and IE11 with PlayReady and Chrome with Widevine.</span></span>
<span data-ttu-id="f4072-136">hello Widevine-licens som tillhandahålls av Axinom JWT token autentisering krävs.</span><span class="sxs-lookup"><span data-stu-id="f4072-136">hello Widevine license server provided by Axinom requires JWT token authentication.</span></span> <span data-ttu-id="f4072-137">Hej JWT-token måste toobe som skickades med begäran om licensserver via en HTTP-huvudet ”X-AxDRM-meddelandet”.</span><span class="sxs-lookup"><span data-stu-id="f4072-137">hello JWT token needs toobe submitted with license request through an HTTP header “X-AxDRM-Message”.</span></span> <span data-ttu-id="f4072-138">Du behöver tooadd hello följande javascript i hello webbsida värd AMP innan du anger hello källa för detta ändamål:</span><span class="sxs-lookup"><span data-stu-id="f4072-138">For this purpose, you need tooadd hello following javascript in hello web page hosting AMP before setting hello source:</span></span>

    <script>AzureHtml5JS.KeySystem.WidevineCustomAuthorizationHeader = "X-AxDRM-Message"</script>

<span data-ttu-id="f4072-139">hello resten av AMP koden är standard AMP API som AMP dokumentet [här](http://amp.azure.net/libs/amp/latest/docs/).</span><span class="sxs-lookup"><span data-stu-id="f4072-139">hello rest of AMP code is standard AMP API as in AMP document [here](http://amp.azure.net/libs/amp/latest/docs/).</span></span>

<span data-ttu-id="f4072-140">Observera att hello ovan javascript för anpassade authorization-huvud är fortfarande en kort sikt metod innan hello officiella långsiktig strategi i AMP släpps.</span><span class="sxs-lookup"><span data-stu-id="f4072-140">Note that hello above javascript for setting custom authorization header is still a short term approach before hello official long term approach in AMP is released.</span></span>

## <a name="jwt-token-generation"></a><span data-ttu-id="f4072-141">JWT-Token generering</span><span class="sxs-lookup"><span data-stu-id="f4072-141">JWT Token Generation</span></span>
<span data-ttu-id="f4072-142">Axinom Widevine krävs licens för att testa autentisering för JWT-token.</span><span class="sxs-lookup"><span data-stu-id="f4072-142">Axinom Widevine license server for testing requires JWT token authentication.</span></span> <span data-ttu-id="f4072-143">Dessutom kan är en av hello anspråk i hello JWT-token av en sammansatt objekttyp i stället för primitiv datatyp.</span><span class="sxs-lookup"><span data-stu-id="f4072-143">In addition, one of hello claims in hello JWT token is of a complex object type instead of primitive data type.</span></span>

<span data-ttu-id="f4072-144">Azure AD kan tyvärr bara utfärda JWT-token med primitiva typer.</span><span class="sxs-lookup"><span data-stu-id="f4072-144">Unfortunately, Azure AD can only issue JWT tokens with primitive types.</span></span> <span data-ttu-id="f4072-145">På liknande sätt, .NET Framework-API (System.IdentityModel.Tokens.SecurityTokenHandler och JwtPayload) kan du bara tooinput komplexa objekttyp som anspråk.</span><span class="sxs-lookup"><span data-stu-id="f4072-145">Similarly, .NET Framework API (System.IdentityModel.Tokens.SecurityTokenHandler and JwtPayload) only allows you tooinput complex object type as claims.</span></span> <span data-ttu-id="f4072-146">Hello anspråk serialiseras fortfarande som sträng.</span><span class="sxs-lookup"><span data-stu-id="f4072-146">However, hello claims are still serialized as string.</span></span> <span data-ttu-id="f4072-147">Därför kan vi använda någon av hello två för genererar hello JWT-token för begäran för Widevine-licens.</span><span class="sxs-lookup"><span data-stu-id="f4072-147">Therefore we cannot use any of hello two for generating hello JWT token for Widevine license request.</span></span>

<span data-ttu-id="f4072-148">John Sheehan [JWT Nuget-paketet](https://www.nuget.org/packages/JWT) uppfyller hello behov så vi toouse Nuget-paketet.</span><span class="sxs-lookup"><span data-stu-id="f4072-148">John Sheehan’s [JWT Nuget package](https://www.nuget.org/packages/JWT) meets hello needs so we are going toouse this Nuget package.</span></span>

<span data-ttu-id="f4072-149">Nedan visas hello koden för genererar JWT-token med hello behövs anspråk som krävs av Axinom Widevine licensserver för testning:</span><span class="sxs-lookup"><span data-stu-id="f4072-149">Below is hello code for generating JWT token with hello needed claims as required by Axinom Widevine license server for testing:</span></span>

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.IdentityModel.Tokens;
    using System.IdentityModel.Protocols.WSTrust;
    using System.Security.Claims;

    namespace OpenIdConnectWeb.Utils
    {
        public class JwtUtils
        {
            //using John Sheehan's NuGet JWT library: https://www.nuget.org/packages/JWT/
            public static string CreateJwtSheehan(string symmetricKeyHex, string key_id)
            {
                byte[] symmetricKey = ConvertHexStringToByteArray(symmetricKeyHex);  //hex string toobyte[] Note: Note that hello key is a hex string, however it must be treated as a series of bytes not a string when encoding.

                var payload = new Dictionary<string, object>()
                             {
                                 { "version", 1 },
                                 { "com_key_id", System.Configuration.ConfigurationManager.AppSettings["ax:com_key_id"] },
                                 { "message", new { type = "entitlement_message", key_ids = new string[] { key_id } }  }
                             };

                string token = JWT.JsonWebToken.Encode(payload, symmetricKey, JWT.JwtHashAlgorithm.HS256);

                return token;
            }

            //convert hex string toobyte[]
            public static byte[] ConvertHexStringToByteArray(string hexString)
            {
                if (hexString.Length % 2 != 0)
                {
                    throw new ArgumentException(String.Format(System.Globalization.CultureInfo.InvariantCulture, "hello binary key cannot have an odd number of digits: {0}", hexString));
                }

                byte[] HexAsBytes = new byte[hexString.Length / 2];
                for (int index = 0; index < HexAsBytes.Length; index++)
                {
                    string byteValue = hexString.Substring(index * 2, 2);
                    HexAsBytes[index] = byte.Parse(byteValue, System.Globalization.NumberStyles.HexNumber, System.Globalization.CultureInfo.InvariantCulture);
                }

                return HexAsBytes;
            }

        }  

    }  

<span data-ttu-id="f4072-150">Axinom Widevine licensserver</span><span class="sxs-lookup"><span data-stu-id="f4072-150">Axinom Widevine license server</span></span>

    <add key="ax:laurl" value="http://drm-widevine-licensing.axtest.net/AcquireLicense" />
    <add key="ax:com_key_id" value="69e54088-e9e0-4530-8c1a-1eb6dcd0d14e" />
    <add key="ax:com_key" value="4861292d027e269791093327e62ceefdbea489a4c7e5a4974cc904b840fd7c0f" />
    <add key="ax:keyseed" value="8888000000000000000000000000000000000000" />

### <a name="considerations"></a><span data-ttu-id="f4072-151">Överväganden</span><span class="sxs-lookup"><span data-stu-id="f4072-151">Considerations</span></span>
1. <span data-ttu-id="f4072-152">Även om AMS Playreadys licensleveranstjänst kräver ”ägar =” före en autentiseringstoken Axinom Widevine licensservern inte använder den.</span><span class="sxs-lookup"><span data-stu-id="f4072-152">Even though AMS PlayReady license delivery service requires “Bearer=” preceding an authentication token, Axinom Widevine license server does not use it.</span></span>
2. <span data-ttu-id="f4072-153">Hej Axinom kommunikation nyckeln används som nyckel för signeringscertifikatet.</span><span class="sxs-lookup"><span data-stu-id="f4072-153">hello Axinom communication key is used as signing key.</span></span> <span data-ttu-id="f4072-154">Observera hello nyckeln är en hexadecimal sträng, men den måste behandlas som en serie byte inte en sträng när kodning.</span><span class="sxs-lookup"><span data-stu-id="f4072-154">Note that hello key is a hex string, however it must be treated as a series of bytes not a string when encoding.</span></span> <span data-ttu-id="f4072-155">Detta uppnås av hello metoden ConvertHexStringToByteArray.</span><span class="sxs-lookup"><span data-stu-id="f4072-155">This is achieved by hello method ConvertHexStringToByteArray.</span></span>

## <a name="retrieving-key-id"></a><span data-ttu-id="f4072-156">Hämta nyckel-ID</span><span class="sxs-lookup"><span data-stu-id="f4072-156">Retrieving Key ID</span></span>
<span data-ttu-id="f4072-157">Kanske såg du att i hello-kod för att generera en JWT-token, nyckel-ID krävs.</span><span class="sxs-lookup"><span data-stu-id="f4072-157">You may have noticed that in hello code for generating a JWT token, key ID is required.</span></span> <span data-ttu-id="f4072-158">Eftersom hello JWT-token måste toobe redo innan du läser in AMP player, ordna viktiga ID behov toobe hämtas i toogenerate JWT-token.</span><span class="sxs-lookup"><span data-stu-id="f4072-158">Since hello JWT token needs toobe ready before loading AMP player, key ID needs toobe retrieved in order toogenerate JWT token.</span></span>

<span data-ttu-id="f4072-159">I kursen finns flera olika sätt tooget håller nyckel-ID.</span><span class="sxs-lookup"><span data-stu-id="f4072-159">Of course there are multiple ways tooget hold of key ID.</span></span> <span data-ttu-id="f4072-160">Till exempel en kan lagra nyckeln ID tillsammans med innehållet metadata i en databas.</span><span class="sxs-lookup"><span data-stu-id="f4072-160">For example, one may store key ID together with content metadata in a database.</span></span> <span data-ttu-id="f4072-161">Eller du kan hämta nyckeln ID från DASH MPD (Media presentationsbeskrivning)-fil.</span><span class="sxs-lookup"><span data-stu-id="f4072-161">Or you can retrieve key ID from DASH MPD (Media Presentation Description) file.</span></span> <span data-ttu-id="f4072-162">hello är nedan för hello senare.</span><span class="sxs-lookup"><span data-stu-id="f4072-162">hello code below is for hello latter.</span></span>

    //get key_id from DASH MPD
    public static string GetKeyID(string dashUrl)
    {
        if (!dashUrl.EndsWith("(format=mpd-time-csf)"))
        {
            dashUrl += "(format=mpd-time-csf)";
        }

        XPathDocument objXPathDocument = new XPathDocument(dashUrl);
        XPathNavigator objXPathNavigator = objXPathDocument.CreateNavigator();
        XmlNamespaceManager objXmlNamespaceManager = new XmlNamespaceManager(objXPathNavigator.NameTable);
        objXmlNamespaceManager.AddNamespace("",     "urn:mpeg:dash:schema:mpd:2011");
        objXmlNamespaceManager.AddNamespace("ns1",  "urn:mpeg:dash:schema:mpd:2011");
        objXmlNamespaceManager.AddNamespace("cenc", "urn:mpeg:cenc:2013");
        objXmlNamespaceManager.AddNamespace("ms",   "urn:microsoft");
        objXmlNamespaceManager.AddNamespace("mspr", "urn:microsoft:playready");
        objXmlNamespaceManager.AddNamespace("xsi",  "http://www.w3.org/2001/XMLSchema-instance");
        objXmlNamespaceManager.PushScope();

        XPathNodeIterator objXPathNodeIterator;
        objXPathNodeIterator = objXPathNavigator.Select("//ns1:MPD/ns1:Period/ns1:AdaptationSet/ns1:ContentProtection[@value='cenc']", objXmlNamespaceManager);

        string key_id = string.Empty;
        if (objXPathNodeIterator.MoveNext())
        {
            key_id = objXPathNodeIterator.Current.GetAttribute("default_KID", "urn:mpeg:cenc:2013");
        }

        return key_id;
    }

## <a name="summary"></a><span data-ttu-id="f4072-163">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="f4072-163">Summary</span></span>
<span data-ttu-id="f4072-164">Med Widevine-stöd i Azure Media Services-innehållsskydd och Azure Media Player senaste dessutom vi kan tooimplement strömning av DASH + flera native DRM (PlayReady och Widevine) med både PlayReady Licenstjänsten i AMS och Widevine-licens servern från Axinom för hello följande moderna webbläsare:</span><span class="sxs-lookup"><span data-stu-id="f4072-164">With latest addition of Widevine support in both Azure Media Services Content Protection and Azure Media Player, we are able tooimplement streaming of DASH + Multi-native-DRM (PlayReady + Widevine) with both PlayReady license service in AMS and Widevine license server from Axinom for hello following modern browsers:</span></span>

* <span data-ttu-id="f4072-165">Chrome</span><span class="sxs-lookup"><span data-stu-id="f4072-165">Chrome</span></span>
* <span data-ttu-id="f4072-166">Microsoft Edge på Windows 10</span><span class="sxs-lookup"><span data-stu-id="f4072-166">Microsoft Edge on Windows 10</span></span>
* <span data-ttu-id="f4072-167">Internet Explorer 11 på Windows 8.1 och Windows 10</span><span class="sxs-lookup"><span data-stu-id="f4072-167">IE 11 on Windows 8.1 and Windows 10</span></span>
* <span data-ttu-id="f4072-168">Både Firefox (stationär dator) och Safari på Mac (inte iOS) också stöds via Silverlight och hello samma URL med Azure Media Player</span><span class="sxs-lookup"><span data-stu-id="f4072-168">Both Firefox (Desktop) and Safari on Mac (not iOS) are also supported via Silverlight and hello same URL with Azure Media Player</span></span>

<span data-ttu-id="f4072-169">hello som följande parametrar krävs i hello Mini lösning användning Axinom Widevine licensservern.</span><span class="sxs-lookup"><span data-stu-id="f4072-169">hello following parameters are required in hello mini-solution leveraging Axinom Widevine license server.</span></span> <span data-ttu-id="f4072-170">Förutom nyckel-ID, hello resten av parametrarna som tillhandahålls av Axinom baserat på deras Widevine-serverinstallation.</span><span class="sxs-lookup"><span data-stu-id="f4072-170">Except for key ID, hello rest of parameters are provided by Axinom based on their Widevine server setup.</span></span>

| <span data-ttu-id="f4072-171">Parameter</span><span class="sxs-lookup"><span data-stu-id="f4072-171">Parameter</span></span> | <span data-ttu-id="f4072-172">Hur den används</span><span class="sxs-lookup"><span data-stu-id="f4072-172">How it is used</span></span> |
| --- | --- |
| <span data-ttu-id="f4072-173">Kommunikation nyckeln ID</span><span class="sxs-lookup"><span data-stu-id="f4072-173">Communication key ID</span></span> |<span data-ttu-id="f4072-174">Måste tas med som värde för hello anspråk ”com_key_id” i JWT-token (se [detta](media-services-axinom-integration.md#jwt-token-generation) avsnitt).</span><span class="sxs-lookup"><span data-stu-id="f4072-174">Must be included as value of hello claim "com_key_id" in JWT token (see [this](media-services-axinom-integration.md#jwt-token-generation) section).</span></span> |
| <span data-ttu-id="f4072-175">Kommunikation-nyckel</span><span class="sxs-lookup"><span data-stu-id="f4072-175">Communication key</span></span> |<span data-ttu-id="f4072-176">Måste användas som hello signeringsnyckeln för JWT-token (se [detta](media-services-axinom-integration.md#jwt-token-generation) avsnitt).</span><span class="sxs-lookup"><span data-stu-id="f4072-176">Must be used as hello signing key of JWT token (see [this](media-services-axinom-integration.md#jwt-token-generation) section).</span></span> |
| <span data-ttu-id="f4072-177">Nyckel-dirigering</span><span class="sxs-lookup"><span data-stu-id="f4072-177">Key seed</span></span> |<span data-ttu-id="f4072-178">Använda toogenerate innehållsnyckeln med alla ges innehåll nyckeln ID (se [detta](media-services-axinom-integration.md#content-protection) avsnittet).</span><span class="sxs-lookup"><span data-stu-id="f4072-178">Must be used toogenerate content key with any given content key ID (see  [this](media-services-axinom-integration.md#content-protection) section).</span></span> |
| <span data-ttu-id="f4072-179">URL för anskaffning av Widevine-licens</span><span class="sxs-lookup"><span data-stu-id="f4072-179">Widevine License acquisition URL</span></span> |<span data-ttu-id="f4072-180">Måste användas i Konfigurera tillgångsleveransprincip för direktuppspelning av STRECK (se [detta](media-services-axinom-integration.md#content-protection) avsnitt).</span><span class="sxs-lookup"><span data-stu-id="f4072-180">Must be used in configuring asset delivery policy for DASH streaming (see  [this](media-services-axinom-integration.md#content-protection) section ).</span></span> |
| <span data-ttu-id="f4072-181">Innehåll nyckel-ID</span><span class="sxs-lookup"><span data-stu-id="f4072-181">Content Key ID</span></span> |<span data-ttu-id="f4072-182">Måste vara en del av hello värdet för rätt meddelande anspråk av JWT-token (se [detta](media-services-axinom-integration.md#jwt-token-generation) avsnitt).</span><span class="sxs-lookup"><span data-stu-id="f4072-182">Must be included as part of hello value of Entitlement Message claim of JWT token (see [this](media-services-axinom-integration.md#jwt-token-generation) section).</span></span> |

## <a name="media-services-learning-paths"></a><span data-ttu-id="f4072-183">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="f4072-183">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="f4072-184">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="f4072-184">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

### <a name="acknowledgments"></a><span data-ttu-id="f4072-185">Bekräftelser</span><span class="sxs-lookup"><span data-stu-id="f4072-185">Acknowledgments</span></span>
<span data-ttu-id="f4072-186">Vi vill gärna tooacknowledge hello följande personer som har bidragit till att skapa det här dokumentet: Kristjan Jõgi av Axinom och Mingfei Yan Amit Rajput.</span><span class="sxs-lookup"><span data-stu-id="f4072-186">We would like tooacknowledge hello following people who contributed towards creating this document: Kristjan Jõgi of Axinom, Mingfei Yan, and Amit Rajput.</span></span>

