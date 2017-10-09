---
title: aaaUsing castLabs toodeliver Widevine-licenser tooAzure Media Services | Microsoft Docs
description: "Den här artikeln beskriver hur du kan använda Azure Media Services (AMS) toodeliver en dataström som krypteras dynamiskt med AMS med både PlayReady och Widevine DRMs. hello PlayReady-licens kommer från licensserver för Media Services PlayReady och Widevine-licens levereras med castLabs licensservern."
services: media-services
documentationcenter: 
author: Mingfeiy
manager: cfowler
editor: 
ms.assetid: 2a9a408a-a995-49e1-8d8f-ac5b51e17d40
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: Mingfeiy;willzhan;Juliako
ms.openlocfilehash: 80d2778fb283a96361e7e511990a36c2f551a310
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-castlabs-toodeliver-widevine-licenses-tooazure-media-services"></a><span data-ttu-id="7d5c0-104">Med hjälp av castLabs toodeliver Widevine-licenser tooAzure Media Services</span><span class="sxs-lookup"><span data-stu-id="7d5c0-104">Using castLabs toodeliver Widevine licenses tooAzure Media Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7d5c0-105">Axinom</span><span class="sxs-lookup"><span data-stu-id="7d5c0-105">Axinom</span></span>](media-services-axinom-integration.md)
> * [<span data-ttu-id="7d5c0-106">castLabs</span><span class="sxs-lookup"><span data-stu-id="7d5c0-106">castLabs</span></span>](media-services-castlabs-integration.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="7d5c0-107">Översikt</span><span class="sxs-lookup"><span data-stu-id="7d5c0-107">Overview</span></span>
<span data-ttu-id="7d5c0-108">Den här artikeln beskriver hur du kan använda Azure Media Services (AMS) toodeliver en dataström som krypteras dynamiskt med AMS med både PlayReady och Widevine DRMs.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-108">This article describes how you can use Azure Media Services (AMS) toodeliver a stream that is dynamically encrypted by AMS with both PlayReady and Widevine DRMs.</span></span> <span data-ttu-id="7d5c0-109">hello PlayReady-licens kommer från licensserver för Media Services PlayReady och Widevine-licens levereras av **castLabs** licensservern.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-109">hello PlayReady license comes from Media Services PlayReady license server and Widevine license is delivered by **castLabs** license server.</span></span>

<span data-ttu-id="7d5c0-110">tooplayback strömning innehåll som skyddas av CENC (PlayReady eller Widevine), kan du använda [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="7d5c0-110">tooplayback streaming content protected by CENC (PlayReady and/or Widevine), you can use  [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span> <span data-ttu-id="7d5c0-111">Se [AMP dokumentet](http://amp.azure.net/libs/amp/latest/docs/) mer information.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-111">See [AMP document](http://amp.azure.net/libs/amp/latest/docs/) for details.</span></span>

<span data-ttu-id="7d5c0-112">hello följande diagram visar en övergripande arkitektur för Azure Media Services och castLabs integrering.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-112">hello following diagram demonstrates a high-level Azure Media Services and castLabs integration architecture.</span></span>

![Integration](./media/media-services-castlabs-integration/media-services-castlabs-integration.png)

## <a name="typical-system-set-up"></a><span data-ttu-id="7d5c0-114">Vanliga systemet</span><span class="sxs-lookup"><span data-stu-id="7d5c0-114">Typical system set up</span></span>
* <span data-ttu-id="7d5c0-115">Medieinnehåll lagras i AMS.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-115">Media content is stored in AMS.</span></span>
* <span data-ttu-id="7d5c0-116">Nyckel-ID: N innehåll nycklar lagras i både castLabs och AMS.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-116">Key IDs of content keys are stored in both castLabs and AMS.</span></span>
* <span data-ttu-id="7d5c0-117">castLabs och AMS har inbyggda-tokenautentisering.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-117">castLabs and AMS both have token authentication built in.</span></span> <span data-ttu-id="7d5c0-118">hello följande avsnitt beskrivs autentiseringstoken.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-118">hello following sections discuss authentication tokens.</span></span> 
* <span data-ttu-id="7d5c0-119">När en klient begär toostream hello video, hello innehåll krypteras dynamiskt med **Common Encryption** (CENC) och dynamiskt paketerats av AMS tooSmooth Streaming och bindestreck.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-119">When a client requests toostream hello video, hello content is dynamically encrypted with **Common Encryption** (CENC) and dynamically packaged by AMS tooSmooth Streaming and DASH.</span></span> <span data-ttu-id="7d5c0-120">Vi kan också leverera PlayReady M2TS elementära dataströmmen kryptering för HLS strömningsprotokoll.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-120">We also deliver PlayReady M2TS elementary stream encryption for HLS streaming protocol.</span></span>
* <span data-ttu-id="7d5c0-121">PlayReady-licens har hämtats från AMS licensservern och Widevine-licens har hämtats från castLabs licensservern.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-121">PlayReady license is retrieved from AMS license server and Widevine license is retrieved from castLabs license server.</span></span> 
* <span data-ttu-id="7d5c0-122">Media Player avgör vilka licens toofetch baserat på hello klienten plattformskapacitet automatiskt.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-122">Media Player automatically decides which license toofetch based on hello client platform capability.</span></span> 

## <a name="authentication-token-generation-for-getting-a-license"></a><span data-ttu-id="7d5c0-123">Autentisering-token generering för att hämta en licens</span><span class="sxs-lookup"><span data-stu-id="7d5c0-123">Authentication token generation for getting a license</span></span>
<span data-ttu-id="7d5c0-124">Både castLabs och AMS stöder JWT (JSON Web Token) token formatet tooauthorize en licens.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-124">Both castLabs and AMS support JWT (JSON Web Token) token format used tooauthorize a license.</span></span> 

### <a name="jwt-token-in-ams"></a><span data-ttu-id="7d5c0-125">JWT-token i AMS</span><span class="sxs-lookup"><span data-stu-id="7d5c0-125">JWT token in AMS</span></span>
<span data-ttu-id="7d5c0-126">hello i den följande tabellen beskrivs JWT-token i AMS.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-126">hello following table describes JWT token in AMS.</span></span> 

| <span data-ttu-id="7d5c0-127">Utfärdaren</span><span class="sxs-lookup"><span data-stu-id="7d5c0-127">Issuer</span></span> | <span data-ttu-id="7d5c0-128">Utfärdaren sträng från hello valt Secure säkerhetstokentjänst (STS)</span><span class="sxs-lookup"><span data-stu-id="7d5c0-128">Issuer string from hello chosen Secure Token Service (STS)</span></span> |
| --- | --- |
| <span data-ttu-id="7d5c0-129">Målgrupp</span><span class="sxs-lookup"><span data-stu-id="7d5c0-129">Audience</span></span> |<span data-ttu-id="7d5c0-130">Använda målgruppen sträng från hello STS</span><span class="sxs-lookup"><span data-stu-id="7d5c0-130">Audience string from hello used STS</span></span> |
| <span data-ttu-id="7d5c0-131">Anspråk</span><span class="sxs-lookup"><span data-stu-id="7d5c0-131">Claims</span></span> |<span data-ttu-id="7d5c0-132">En uppsättning anspråk</span><span class="sxs-lookup"><span data-stu-id="7d5c0-132">A set of claims</span></span> |
| <span data-ttu-id="7d5c0-133">Inte före</span><span class="sxs-lookup"><span data-stu-id="7d5c0-133">NotBefore</span></span> |<span data-ttu-id="7d5c0-134">Starta giltighet hello-token</span><span class="sxs-lookup"><span data-stu-id="7d5c0-134">Start validity of hello token</span></span> |
| <span data-ttu-id="7d5c0-135">Upphör att gälla</span><span class="sxs-lookup"><span data-stu-id="7d5c0-135">Expires</span></span> |<span data-ttu-id="7d5c0-136">End giltighet hello-token</span><span class="sxs-lookup"><span data-stu-id="7d5c0-136">End validity of hello token</span></span> |
| <span data-ttu-id="7d5c0-137">SigningCredentials</span><span class="sxs-lookup"><span data-stu-id="7d5c0-137">SigningCredentials</span></span> |<span data-ttu-id="7d5c0-138">hello-nyckel som delas med PlayReady licensservern castLabs licensservern och STS, det kan vara antingen symmetriskt eller asymmetriskt nyckel.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-138">hello key that is shared among PlayReady License Server, castLabs License Server and STS, it could be either symmetric or asymmetric key.</span></span> |

### <a name="jwt-token-in-castlabs"></a><span data-ttu-id="7d5c0-139">JWT-token i castLabs</span><span class="sxs-lookup"><span data-stu-id="7d5c0-139">JWT token in castLabs</span></span>
<span data-ttu-id="7d5c0-140">hello i den följande tabellen beskrivs JWT-token i castLabs.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-140">hello following table describes JWT token in castLabs.</span></span> 

| <span data-ttu-id="7d5c0-141">Namn</span><span class="sxs-lookup"><span data-stu-id="7d5c0-141">Name</span></span> | <span data-ttu-id="7d5c0-142">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7d5c0-142">Description</span></span> |
| --- | --- |
| <span data-ttu-id="7d5c0-143">optData</span><span class="sxs-lookup"><span data-stu-id="7d5c0-143">optData</span></span> |<span data-ttu-id="7d5c0-144">En JSON-sträng som innehåller information om dig.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-144">A JSON string containing information about you.</span></span> |
| <span data-ttu-id="7d5c0-145">CRT</span><span class="sxs-lookup"><span data-stu-id="7d5c0-145">crt</span></span> |<span data-ttu-id="7d5c0-146">En JSON-sträng som innehåller information om hello tillgång, dess licens info och uppspelning rättigheter.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-146">A JSON string containing information about hello asset, its license info and playback rights.</span></span> |
| <span data-ttu-id="7d5c0-147">IAT</span><span class="sxs-lookup"><span data-stu-id="7d5c0-147">iat</span></span> |<span data-ttu-id="7d5c0-148">hello aktuella datetime i epok.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-148">hello current datetime in epoch.</span></span> |
| <span data-ttu-id="7d5c0-149">jti</span><span class="sxs-lookup"><span data-stu-id="7d5c0-149">jti</span></span> |<span data-ttu-id="7d5c0-150">En unik identifierare om denna token (varje token kan bara användas en gång i hello castLabs system).</span><span class="sxs-lookup"><span data-stu-id="7d5c0-150">A unique identifier about this token (every token can only be used once in hello castLabs system).</span></span> |

## <a name="sample-solution-set-up"></a><span data-ttu-id="7d5c0-151">Ställ in exempellösning</span><span class="sxs-lookup"><span data-stu-id="7d5c0-151">Sample solution set up</span></span>
<span data-ttu-id="7d5c0-152">Hej [exempel lösning](https://github.com/AzureMediaServicesSamples/CastlabsIntegration) består av två projekt:</span><span class="sxs-lookup"><span data-stu-id="7d5c0-152">hello [sample solution](https://github.com/AzureMediaServicesSamples/CastlabsIntegration) consists of two projects:</span></span>

* <span data-ttu-id="7d5c0-153">En konsolapp som kan använda tooset DRM begränsningar på en redan infogade tillgång för både PlayReady och Widevine.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-153">A console app that can be used tooset DRM restrictions on an already ingested asset, for both PlayReady and Widevine.</span></span>
* <span data-ttu-id="7d5c0-154">Ett webbprogram som lämnar ut tokens som kan ses som en mycket förenklad version av en Säkerhetstokentjänst.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-154">A Web Application that hands out tokens, which could be seen as a VERY SIMPLIFIED version of an STS.</span></span>

<span data-ttu-id="7d5c0-155">toouse hello konsolprogram:</span><span class="sxs-lookup"><span data-stu-id="7d5c0-155">toouse hello console application:</span></span>

1. <span data-ttu-id="7d5c0-156">Ändra hello app.config toosetup AMS autentiseringsuppgifter, castLabs autentiseringsuppgifter, STS-konfiguration och delad nyckel.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-156">Change hello app.config toosetup AMS credentials, castLabs credentials, STS configuration and shared key.</span></span>
2. <span data-ttu-id="7d5c0-157">Ladda upp en tillgång till AMS.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-157">Upload an Asset into AMS.</span></span>
3. <span data-ttu-id="7d5c0-158">Get hello UUID från hello överförs tillgången och ändra raden i filen Program.cs för hello 32:</span><span class="sxs-lookup"><span data-stu-id="7d5c0-158">Get hello UUID from hello uploaded Asset, and change Line 32 in hello Program.cs file:</span></span>
   
      <span data-ttu-id="7d5c0-159">var objIAsset = _context. Assets.Where (x = > x.Id == ”nb:cid:UUID:dac53a5d-1 500-80bd-b864-f1e4b62594cf”). FirstOrDefault();</span><span class="sxs-lookup"><span data-stu-id="7d5c0-159">var objIAsset = _context.Assets.Where(x => x.Id == "nb:cid:UUID:dac53a5d-1500-80bd-b864-f1e4b62594cf").FirstOrDefault();</span></span>
4. <span data-ttu-id="7d5c0-160">Använd en AssetId för namngivning av hello tillgången i hello castLabs system (rad 44 i hello Program.cs-filen).</span><span class="sxs-lookup"><span data-stu-id="7d5c0-160">Use an AssetId for naming hello asset in hello castLabs system (Line 44 in hello Program.cs file).</span></span>
   
   <span data-ttu-id="7d5c0-161">Du måste ange AssetId för **castLabs**; måste toobe en unik alfanumerisk sträng.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-161">You must set AssetId for **castLabs**; it needs toobe a unique alphanumeric string.</span></span>
5. <span data-ttu-id="7d5c0-162">Kör programmet hello.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-162">Run hello program.</span></span>

<span data-ttu-id="7d5c0-163">toouse hello Web Application (STS):</span><span class="sxs-lookup"><span data-stu-id="7d5c0-163">toouse hello Web Application (STS):</span></span>

1. <span data-ttu-id="7d5c0-164">Ändra hello web.config toosetup castlabs tillhör affärsenhet ID, hello STS konfiguration och hello delad nyckel.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-164">Change hello web.config toosetup castlabs merchant ID, hello STS configuration and hello shared key.</span></span>
2. <span data-ttu-id="7d5c0-165">Distribuera tooAzure webbplatser.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-165">Deploy tooAzure Websites.</span></span>
3. <span data-ttu-id="7d5c0-166">Navigera toohello webbplats.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-166">Navigate toohello website.</span></span>

## <a name="playing-back-a-video"></a><span data-ttu-id="7d5c0-167">Spela upp en video</span><span class="sxs-lookup"><span data-stu-id="7d5c0-167">Playing back a video</span></span>
<span data-ttu-id="7d5c0-168">tooplayback en video som krypterats med vanliga kryptering (PlayReady eller Widevine), kan du använda hello [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="7d5c0-168">tooplayback a video encrypted with common encryption (PlayReady and/or Widevine), you can use hello [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span> <span data-ttu-id="7d5c0-169">När du kör hello konsolprogram eko hello innehåll nyckel-ID och hello Manifest URL.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-169">When running hello console app, hello Content Key ID and hello Manifest URL are echoed.</span></span>

1. <span data-ttu-id="7d5c0-170">Öppna en ny flik och starta din STS: http://[yourStsName].azurewebsites.net/api/token/assetid/[yourCastLabsAssetId]/contentkeyid/[thecontentkeyid].</span><span class="sxs-lookup"><span data-stu-id="7d5c0-170">Open a new tab and launch your STS: http://[yourStsName].azurewebsites.net/api/token/assetid/[yourCastLabsAssetId]/contentkeyid/[thecontentkeyid].</span></span>
2. <span data-ttu-id="7d5c0-171">Gå för[Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="7d5c0-171">Go too[Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span>
3. <span data-ttu-id="7d5c0-172">Klistra in i hello strömnings-URL.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-172">Paste in hello streaming URL.</span></span>
4. <span data-ttu-id="7d5c0-173">Klicka på hello **avancerade alternativ** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-173">Click hello **Advanced Options** checkbox.</span></span>
5. <span data-ttu-id="7d5c0-174">I hello **skydd** listrutan, Välj PlayReady och/eller Widevine.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-174">In hello **Protection** dropdown, select PlayReady and/or Widevine.</span></span>
6. <span data-ttu-id="7d5c0-175">Klistra in hello-token som du har fått från din STS i hello Token textruta.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-175">Paste hello token that you got from your STS in hello Token textbox.</span></span> 
   
   <span data-ttu-id="7d5c0-176">Hej castLab licensservern inte behöver hello ”ägar =” prefix framför hello-token.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-176">hello castLab license server does not need hello “Bearer=” prefix in front of hello token.</span></span> <span data-ttu-id="7d5c0-177">Så ta bort som innan du skickar hello-token.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-177">So please remove that before submitting hello token.</span></span>
7. <span data-ttu-id="7d5c0-178">Uppdatera hello player.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-178">Update hello player.</span></span>
8. <span data-ttu-id="7d5c0-179">hello video ska spelas.</span><span class="sxs-lookup"><span data-stu-id="7d5c0-179">hello video should be playing.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="7d5c0-180">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="7d5c0-180">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="7d5c0-181">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="7d5c0-181">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

